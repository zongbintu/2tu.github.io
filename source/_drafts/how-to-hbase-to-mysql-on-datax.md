title: 怎样使用DataX将HBase数据导入到MySQL
categories: Development
tags: [DataX]

date: 2023-08-04 18:03:44
---

有需求使用DataX将HBase表中数据迁移到MySQL中，有的表在Phoenix中定义，使用hbase20xsqlreader读取。有的没有在Phoenix中定义，数据是动态插入，列是不固定的。

- hbase20xsqlreader，读取Phoenix
- hbase11xreader，读取HBase

不管读取HBase还是Phoenix都要求填写column，但是可以写sql占位，传进来。

我这里由于业务原因就没有使用占位符的方式传递。

现在的业务需求是我HBase横表迁移到MySQL纵表中。

动态列中的列名有在MySQL表中记录，所以实现流程就是

1. shell 脚本调用mysql读取出要在HBase中读取的列
2. 循环生成hbase11xreader读取-写入txt的DataX json
3. 调用DataX执行上面的json文件导出txt
4. 循环txt为每行加入MySQL要插入的列key
5. 生成读取txt，写入MySQL

<!-- more -->

代码

``` shell
#!/bin/bash

# MySQL连接信息
DB_USER=""
DB_PASS=""
DB_HOST=""
DB_NAME=""

# 执行的SQL语句
SQL_QUERY="SELECT columnKey,otherColumn FROM KV;"

# 执行SQL并将结果存储到关联数组
result=($(mysql -u${DB_USER} -p${DB_PASS} -h${DB_HOST} -D${DB_NAME} -N -B -e "${SQL_QUERY}"))

# 遍历结果，生成文件
for ((i=0; i<${#result[@]}; i+=2)); do
    columnKey=${result[i]}
    otherColumn=${result[i+1]}
    
    # 生成JSON内容
    json_content="{
        \"job\": {
            \"setting\": {
                \"speed\": {
                    \"channel\": 1
                }
            },
            \"content\": [
                {
                    \"reader\": {
                        \"name\": \"hbase11xreader\",
                        \"parameter\": {
                            \"hbaseConfig\": {
                                \"hbase.zookeeper.quorum\": \"localhost\"
                            },
                            \"table\": \"Table\",
                            \"encoding\": \"utf-8\",
                            \"mode\": \"normal\",
                            \"column\": [
                                {
                                    \"name\": \"cf: column1\",
                                    \"type\": \"string\"
                                },
                               {
                                    \"name\": \"cf: column2\",
                                    \"type\": \"date\",
                                    \"format\":\"yyyy-MM-dd HH:mm:ss\"
                                },
                               {
                                    \"name\": \"cf: ${columnKey}\",
                                    \"type\": \"string\"
                                }
                            ],
                            \"range\": {
                                \"startRowkey\": \"\",
                                \"endRowkey\": \"\",
                                \"isBinaryRowkey\": true
                            }
                        }
                    },
                    \"writer\": {
                        \"name\": \"txtfilewriter\",
                        \"parameter\": {
                            \"path\": \"datax_trans_file/tmp\",
                            \"fileName\": \"dataCustom_${columnKey}_${otherColumn}\",
                            \"writeMode\": \"truncate\",
                            \"fieldDelimiter\": \"|\"
                        }
                    }
                }
            ]
        }
    }"
    
    # 将JSON内容写入文件
    echo "$json_content" > "datax_trans_file/dataCustom_${columnKey}_${otherColumn}_2txt.json"

    # 执行datax 生成txt文件
    dataxtool/datax/bin/datax.py datax_trans_file/dataCustom_${columnKey}_${otherColumn}_2txt.json
    
     # 读出对应txt 插入统计编码和名称

done

# 列出txt文件列表 插入数据


# 遍历指定目录下的匹配文件，并处理文件内容
for file in datax_trans_file/tmp/dataCustom_*; do
    if [[ -f "$file" ]]; then
        # 提取otherColumn和columnKey
        filename=$(basename "$file")
        IFS="_" read -ra parts <<< "$filename"
        columnKey="${parts[1]}"
        otherColumn="${parts[2]}"

          # 为每一行添加信息
        while IFS= read -r line; do
            echo "$line|$columnKey|$otherColumn|1"
        done < "$file" > "$file.tmp"

        # 将临时文件替换原文件
        mv "$file.tmp" "$file"

        echo "Added info to: $file"
    fi
done

# 执行写入mysql
dataxtool/datax/bin/datax.py datax_trans_file/dataCustom2mysql.json

```

