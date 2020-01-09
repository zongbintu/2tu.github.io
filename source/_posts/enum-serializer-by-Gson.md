title: Gson序列化、反序列化枚举
tags: [Gson序列化枚举,Gson serialization/deserialization enum,Gson反序列化枚举,JsonDeserializer,JsonSerializer,registerTypeAdapter]
date: 2020-01-08 22:16:28
categories: Java
---
####  后端返回了N多枚举，前端Gson解析报错？  

后端接口返回很多枚举，Android复用实体，解析时却报错  

```
com.google.gson.JsonSyntaxException: java.lang.IllegalStateException: 
Expected a string but was BEGIN_OBJECT at line 1 column 70 path $.data.type
```

先定义一个枚举例子  Type.java  

```
public enum Type{
	GSON(0, "Gson"),
	JACKSON(1, "Jackson"),
	FASTJSON(2, "FastJson");
	private final int code;
	private final String name;
	
	Type(int code, String name){
		this.code = code;
		this.name = name;
	}
	
	public static Type code(int code){
		for (Type i : Type.values()){
			if(i.code() == code){
				return i;
			}
		}
		return null;
	}
	
	public static Type name(String name){
		for (Type i : Type.values()){
			if(i.name().equals(name)){
				return i;
			}
		}
		return null;
	}
	
	public int code(){
		return code;
	}
	
	public String name(){
		return name;
	}
}
```

在序列化时服务端有两种试返回  
1、完整返回枚举

```
{"type":{"code":0,"name":"Gson"}}  
```

2、序列化code或name

```
{"type":0}
//{"type":"Gson"}
```

####	最好的解决办法——解决提出问题的人
很不幸后端是我负责的，所以我们有三个解决方法  
1、让前端自己解决   
2、为前端提供技术方案  
3、修改后端枚举   
我应该怎么办？  
以上，当然是认怂，直接奉上代码喽  
Json是一种广泛使用的数据交换格式，在Java中序列化和反序列化的各API名称也都类似，下面正式介绍Gson如何序列化、反序列化枚举  

<!-- more -->

#####	实现JsonSerializer/JsonDeserializer
######		序列化及反序列化如何实现
实现JsonSerializer/JsonDeserializer，在序列化和反序列化时会调用重载的serialize、deserialize方法  

```
import com.google.gson.JsonDeserializationContext;
import com.google.gson.JsonDeserializer;
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonParseException;
import com.google.gson.JsonPrimitive;
import com.google.gson.JsonSerializationContext;
import com.google.gson.JsonSerializer;

public class TypeSerializer implements JsonDeserializer<Type>, JsonSerializer<Type> {

  @Override public Type deserialize(JsonElement json, java.lang.reflect.Type typeOfT,
      JsonDeserializationContext context) throws JsonParseException {
    //{"type":{"code":0,"name":"Gson"}}
    /**
    return Type.code(Integer.valueOf(((JsonObject) json).get("code").toString()));
    return Type.name(((JsonObject) json).get("name").toString());
     **/
     
     //{"type":0}
	 //{"type":"Gson"}
    /**
    return Type.code(json.getAsNumber());
    return Type.name(json.getAsString());
     **/
    return null;
  }

  @Override public JsonElement serialize(Type src, java.lang.reflect.Type typeOfSrc,
      JsonSerializationContext context) {
      //{"type":{"code":0,"name":"Gson"}}
    /**
     JsonObject jsonObject = new JsonObject();
     jsonObject.addProperty("code", src.code());
     jsonObject.addProperty("name", src.name());
     return jsonObject;
     **/
     
     //{"type":0}
	 //{"type":"Gson"}
    /**
    JsonPrimitive jsonPrimitive = new JsonPrimitive(src.code());
    JsonPrimitive jsonPrimitive = new JsonPrimitive(src.name());
    return jsonPrimitive;
     **/
    return null;
  }
}
```

#####	registerTypeAdapter
还需要将上面的Serializer注册到Gson中  

```
new GsonBuilder()
    .registerTypeAdapter(Type.class, new TypeSerializer())
    .create();
```

#####	解析
经过以上步骤后就解决了枚举序列化和反序列化问题  
gson.fromJson时遇到枚举Type类型会调用TypeSerializer中的serialize方法  
gson.toJson时遇到要解析枚举Type类型时会调用TypeSerializer中的deserialize方法

###### 转载请标明出处： 
###### http://blog.520wa.com/
###### 本文出自Tu's blog