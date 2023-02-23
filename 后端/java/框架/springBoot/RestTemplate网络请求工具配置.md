



在使用RestTemplate自动注入时要手动注入

```java
package com.huayouedu.web.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.client.SimpleClientHttpRequestFactory;
import org.springframework.web.client.RestTemplate;

/**
 * RestTemplate网络请求工具配置累
 */
@Configuration
public class RestClientConfig {

    @Bean
    public RestTemplate restTemplate() {
        SimpleClientHttpRequestFactory factory = new SimpleClientHttpRequestFactory();
        return new RestTemplate(factory);
    }

}
```





```java
 //设置请求头参数
        HttpHeaders requestHeaders = new HttpHeaders();
        requestHeaders.add("access_token", accessToken);
        HttpEntity request = new HttpEntity(requestHeaders);

        String url = String.format(this.url+"/base/api/v1/resourcePlatform/getUserByToken?access_token=%s", accessToken);
        ResponseEntity<String> exchange = restTemplate.exchange(url, HttpMethod.GET,request, String.class);  //git请求



```





```java
 // 请求地址      @SpringBootTest
class PostTests {

   @Resource
   private RestTemplate restTemplate;

   @Test
   void testSimple()  {
      // 请求地址
      String url = "http://jsonplaceholder.typicode.com/posts";

      // 要发送的数据对象
      PostDTO postDTO = new PostDTO();
      postDTO.setUserId(110);
      postDTO.setTitle("zimug 发布文章");
      postDTO.setBody("zimug 发布文章 测试内容");

      // 发送post请求，并输出结果
      PostDTO result = restTemplate.postForObject(url, postDTO, PostDTO.class);
      System.out.println(result);
   }
}
```

