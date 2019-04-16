## springboot使用fastjson
### HttpMessageConverter中json转换使用fastjson
在fastjson的 wiki 中的集成方式是
```
 FastJsonHttpMessageConverter converter = new FastJsonHttpMessageConverter();
 converters.add(0, converter);
```
[在 Spring 中集成 Fastjson](https://github.com/alibaba/fastjson/wiki/%E5%9C%A8-Spring-%E4%B8%AD%E9%9B%86%E6%88%90-Fastjson)

springboot项目启动时，会加载10个message convert。这里将FastJsonHttpMessageConverter设置在第0位，优先级高于 jackson 的转换器。但是直接第0级的话，请求返回值为 String 时，会多加一层引号。对接微信公众号等，服务器验证时，因为多加一层引号则报错。

选择的方式是在StringHttpMessageConverter后加入FastJsonHttpMessageConverter。
```
for(int i = 0; i < converters.size(); i++) {
	HttpMessageConverter currentConverter = converters.get(i);
	if(currentConverter instanceof StringHttpMessageConverter) {
		converters.set(i + 1, converter);
   }
}
```
### spring redis中使用 fastjson
如 wiki 中写的在RedisTemplate中使用GenericFastJsonRedisSerializer，同样是放入 String 类型的结果时，会多加一层引号。单个项目中的存取没啥问题，可是当其他业务使用该缓存时，会有对接问题。
选择的方式是使用 StringRedisTemplate,所有值都是String序列化，封装底层方法来使用 fastjson 转换。