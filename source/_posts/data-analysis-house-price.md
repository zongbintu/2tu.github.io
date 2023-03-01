title: 使用Java、Excel数据分析如何有效投资房产  
date: 2018-01-27 21:47:06  
categories: Data 
tags: [Excel,数据分析,Java数据分析房价]
---

#### 写在前面的话 
2017年IT业中多起猝死、自杀，创业的九死一生，各大公司清算、裁员等。年底也总是充斥着各种活动刷屏，年终总结、账单等。30岁的程序猿什么的，不幸我们可能也是其中之一。要改变现状，离不开钱与投资，提到投资就不得不提房。过去数十年的房价颠覆了努力工作改变生活的思维。作为程序猿比他人多的优势在于可以利用互联网数据进行数据分析决策，接下来看看<font color="red">基于链家中重庆渝北区人和二手房及国家全国商品房销售面积统计的简单数据分析</font>。
<!--more-->

#### 数据收集  
##### 链家公共数据
链家的url很标准，我们要获取的数据如下[【人和二手房_重庆人和二手房出售】(重庆链家网)](https://cq.lianjia.com/ershoufang/renhe/)，2018年1月27日数据为472条，共16页，url格式为  
https://cq.lianjia.com/ershoufang/renhe/pg1/  
...  
https://cq.lianjia.com/ershoufang/renhe/pg16/  
##### Java Jsoup爬虫  
现在全职Java，且为了快速实现采用Jsoup获取Html数据，代码非常简单

```
Document document = Jsoup.connect(domain + uri)
   .userAgent("Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36")
   .timeout(30000).get();
```

##### 保存Html数据至本地文件  
厚道的链家并未做防爬虫，也有可能是为了对搜索引擎友好，提高曝光率。数据获取后要保存至本地文件就简单了，合并后代码如下

```
  public String saveHtml(String domain, String uri) {
    FileOutputStream out = null;
    File dest = new File("src/temp_html/" + (uri.endsWith("/") ? uri.substring(0, uri.length() - 1) : uri) + ".html");
    try {
      Document document = Jsoup.connect(domain + uri)
          .userAgent("Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36")
          .timeout(30000).get();

      if (!dest.getParentFile().exists()) {
        dest.getParentFile().mkdirs();
      }

      if (!dest.exists()) {
        dest.createNewFile();
      }
      out = new FileOutputStream(dest, false);
      out.write(document.toString().getBytes("UTF-8"));
      out.close();


    } catch (IOException e) {
      e.printStackTrace();
      return null;
    } finally {
      IOUtils.closeQuietly(out);
    }

    return dest.getPath();
  }
```

##### 通过Jsoup解析数据至Excel  
再将html中数据解析使用POI写入Excel就相对简单了，主要在于规则匹配
######  解析数据

```
public List<House> parseHtml(String fileName) {
    List<House> houses = null;
    //读取本地html的路径
    File file = new File(fileName);
    //写个循环读取这些文件的名字
    try {
      if (file.isFile()) {
        logger.info("文件{}开始解析", fileName);

        //下面开始解析本地的html
        Document doc = Jsoup.parse(file, "UTF-8");
        Elements sellListContents = doc.select("ul.sellListContent");
        if (!ObjectUtils.isEmpty(sellListContents)) {
          Element sellListContent = sellListContents.first();
          Elements sellContents = sellListContent.select("li.clear");
          if (!ObjectUtils.isEmpty(sellListContents)) {
            houses = new ArrayList<>(sellContents.size());
            for (int i = 0; i < sellContents.size(); i++) {
              Element sellList = sellContents.get(i);
              Elements sellElements = sellList.select("div.clear");
              if (null != sellElements && sellElements.size() == 1) {
                House house = new House();
                Element infoElement = sellElements.first();

                Element titleElement = infoElement.select("div.title").first().select("a[href]").first();
                house.setTitle(titleElement.text());
                house.setUrl(titleElement.attr("href"));

                Element houseInfoElement = infoElement.selectFirst("div.houseInfo");
                house.setVillage(houseInfoElement.select("a").first().text());
                String houseInfo = houseInfoElement.text();
                String[] houseInfos = houseInfo.split("\\|");

                for (int j = 1; j < houseInfos.length; j++
                    ) {
                  switch (j) {
                    case 1:
                      house.setStructure(houseInfos[j].trim());
                      break;
                    case 2:
                      house.setAcreage(houseInfos[j].replace("平米", "").trim());
                      break;
                    case 3:
                      house.setOrientation(houseInfos[j].trim());
                      break;
                    case 4:
                      house.setDecoration(houseInfos[j].trim());
                      break;
                    case 5:
                      house.setElevator(houseInfos[j].trim());
                      break;
                  }
                }
                Element positionInfoElement = infoElement.selectFirst("div.positionInfo");
                house.setRegion(positionInfoElement.select("a").first().text());
                String position = positionInfoElement.text().split(house.getRegion())[0].trim();

                house
                    .setPosition(position.lastIndexOf("-") == position.length() - 1 ? position.substring(0, position.length() - 1).trim() : position);

                house.setTag(infoElement.selectFirst("div.tag").text());

                Element priceInfoElement = infoElement.selectFirst("div.priceInfo");
                house.setTotalPrice(priceInfoElement.selectFirst("div.totalPrice").text().replace("万", ""));

                house.setUnitPrice(priceInfoElement.selectFirst("div.unitPrice").text().replace("单价", "").replace("元/平米", "").trim());

                logger.info("解析第{}个元素，结果为：{}", i, house.toString());
                houses.add(house);
              }
            }
          }
        }
      }
    } catch (Exception e) {
      logger.error("文件{}解析错误", fileName, e);
    }
    return houses;
  }
```

######  写入Excel

```
public String writeExcel(String fileName, List<House> houses) {
    logger.info("文件{}开始写入", fileName);
    try (POIFSFileSystem fs = new POIFSFileSystem(new FileInputStream(fileName))) {
      Workbook workbook = new HSSFWorkbook(fs);
      HSSFSheet sheet = (HSSFSheet) workbook.getSheetAt(0);
      final int preLastRowNum = sheet.getLastRowNum();
      logger.info("文件{},上次行数为{}", fileName, preLastRowNum);
      for (int i = 0; i < houses.size(); i++) {
        Row row = sheet.createRow(preLastRowNum + i + 1);
        House house = houses.get(i);
        row.createCell(0).setCellValue(preLastRowNum + i + 1);
        row.createCell(1).setCellValue(house.getTitle());
        row.createCell(2).setCellValue(house.getTotalPrice());
        row.createCell(3).setCellValue(house.getUnitPrice());
        row.createCell(4).setCellValue(house.getVillage());
        row.createCell(5).setCellValue(house.getStructure());
        row.createCell(6).setCellValue(house.getAcreage());
        row.createCell(7).setCellValue(house.getOrientation());
        row.createCell(8).setCellValue(house.getDecoration());
        row.createCell(9).setCellValue(house.getElevator());
        row.createCell(10).setCellValue(house.getPosition());
        row.createCell(11).setCellValue(house.getRegion());
        row.createCell(12).setCellValue(house.getTag());
        row.createCell(13).setCellValue(house.getUrl());
      }
      FileOutputStream fileOut = new FileOutputStream(fileName);
      workbook.write(fileOut);
      IOUtils.closeQuietly(fileOut);
      logger.info("文件{}写入完成,{}-{}行", fileName, preLastRowNum + 1, preLastRowNum + houses.size());
    } catch (IOException e) {
      logger.error("excel-{}处理出错", fileName, e);
    }

    return fileName;
  }
```

#### 数据处理  
写入后核对数据没什么问题，然后需要对数据进行简单处理。  
##### 删除车位、别墅  
观察发现数据中有车位、别墅存在，不在我们投资范围，需要从类别中筛选出并删除
##### 重复数据  
小区名称中发现有如棕榈泉一期、棕榈泉二期、棕榈泉三期字样，我们此次的初步分析暂时不需要这样统计，在后期具体决策时再才需要参考某期是否离轻轨、学校更近，车位比更高等。所以也需要通过函数全部替换成“棕榈泉”。

#### 数据分析展现  
数据分析在多年拉卡拉、中汇支付风控、合规经验的老婆手把手教导下完成，大量运用透视图  

##### 小区平均单价分析 
初步数据处理后，不会再有总价500w这种不忍直视的内容，此时我们需要来个直观的小区平均单价柱状图冲击一下  
![小区平均单价柱状图](/css/images/20180127_village_unitprice_histogram.png)    
小区平均单价  

| 类型 | 平均单价（元） | 小区 |  
| -------- | ----- | ----- |  
|  最高  | 20,000 | 比毕得豪园 |
|  最低  | 6,480 | 人和家园 |
|  平均  | 14,151.46 |
当然这个价格并不准确，其中有房龄、交通、装修、是否带学校指标等其它因素，但是至少可以让我们跨出第一步让自己对市场价格有一定的认知，计算自己可承受的价位

##### 房源数量分析  
接下来看看房源多的一些小区，经过我们之前处理过后不分几期几期后只取前20，分布如下  
![房源前20小区饼状图](/css/images/20180127_village_sum_pie_chat.png)  
棕榈泉的楼盘本身大房子数量多，且含有别墅洋房及高层，出售最多，占比27.32%力夺第二的9.02%。想必其中很多都是投资来的，如果分析清水与装修过的比例、持有房子年限等应该是可以分析出来   

##### 户型占比分析  
作为投资户型极其重要，市场上哪些户型受欢迎，新楼盘也一样会对人群分析，对楼盘人群定位。  
![户型饼状图](/css/images/20180127_structure_pie_chart.png)  
3室2厅大比分夺得冠军，4室2厅为第二出乎意料，应该具体分析这些房子的出处是否为棕榈泉的洋房。除去4室2厅外，3室2厅和2室2厅都在我们接受的范围，像我们没有多少财力的年轻人很多第一套房会选择2室2厅，够自己和孩子住，但是如果加入父母就不够住了，多半会卖掉换一个3室2厅。  
由此可以得出结论，如果目标为这些改善型的人群，3室2厅是最佳选择  
如果财力有限可以可以买2室2厅  
如果钱更少是否需要买个1室来投资出租，就该考虑地段及租金的关联关系了。  

##### 全国商品房销售面积分析  
2月3日对全国商品房销售面积抓取，制作简单折线图  
![全国销售面积](/css/images/20180127_acreage_china_line_chart.png)  
![重庆销售面积](/css/images/20180127_acreage_cq_line_chart.png)  
  
###### 其实还有很多可分析，想必房子的热门程度和价格是有一定关系，多久可以收回成本等。程序猿总是想太多，一个简单需求整成了一个价格监控、预测系统。所以本文只做简单分析，代码也很简单，2小时左右。

####  源码
Github:[house](https://github.com/2tu/house)
