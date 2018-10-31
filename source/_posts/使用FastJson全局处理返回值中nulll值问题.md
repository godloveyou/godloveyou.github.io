---
title: 阿里FastJson如何处理返回值中nulll值问题
tags: java
categories: fastJson
---

1. springBoot项目,全局处理策略

```
@Configuration
public class HttpMessageConverterConfig {

    @Bean
    public HttpMessageConverters fastJsonHttpMessageConverters() {
        //1、定义一个convert转换消息的对象
        FastJsonHttpMessageConverter fastConverter = new FastJsonHttpMessageConverter();

        //2、添加fastjson的配置信息
        FastJsonConfig fastJsonConfig = new FastJsonConfig();

        SerializerFeature[] serializerFeatures = new SerializerFeature[]{
                //    输出key是包含双引号
				// SerializerFeature.QuoteFieldNames,
                //    是否输出为null的字段,若为null 则显示该字段
				// SerializerFeature.WriteMapNullValue,
                //    数值字段如果为null，则输出为0
                SerializerFeature.WriteNullNumberAsZero,
                //     List字段如果为null,输出为[],而非null
                SerializerFeature.WriteNullListAsEmpty,
                //    字符类型字段如果为null,输出为"",而非null
                SerializerFeature.WriteNullStringAsEmpty,
                //    Boolean字段如果为null,输出为false,而非null
                SerializerFeature.WriteNullBooleanAsFalse,
                //    Date的日期转换器
                SerializerFeature.WriteDateUseDateFormat,
                //    循环引用
                SerializerFeature.DisableCircularReferenceDetect,
        };

        fastJsonConfig.setSerializerFeatures(serializerFeatures);
        fastJsonConfig.setCharset(Charset.forName("UTF-8"));

        //3、在convert中添加配置信息
        fastConverter.setFastJsonConfig(fastJsonConfig);

        //4、将convert添加到converters中
        HttpMessageConverter<?> converter = fastConverter;

        return new HttpMessageConverters(converter);
    }


}
```


2. 作为工具方法 单独使用JSON.toJsonString转换对象为json字符串时用法

```
SerializerFeature[] serializerFeatures = new SerializerFeature[]{
						//    输出key是包含双引号
						//SerializerFeature.QuoteFieldNames,
						//    是否输出为null的字段,若为null 则显示该字段
						//SerializerFeature.WriteMapNullValue,
						//    数值字段如果为null，则输出为0
						SerializerFeature.WriteNullNumberAsZero,
						//     List字段如果为null,输出为[],而非null
						SerializerFeature.WriteNullListAsEmpty,
						//    字符类型字段如果为null,输出为"",而非null
						SerializerFeature.WriteNullStringAsEmpty,
						//    Boolean字段如果为null,输出为false,而非null
						SerializerFeature.WriteNullBooleanAsFalse,
						//    Date的日期转换器
						SerializerFeature.WriteDateUseDateFormat,
						//    循环引用
					SerializerFeature.DisableCircularReferenceDetect};

					//重点在这里,这种设计真的挺灵活的
String content = JSON.toJSONString(body,serializerFeatures);
```
