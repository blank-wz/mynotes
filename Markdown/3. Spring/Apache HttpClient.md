# 一, HttpClient 简介

## 1.1 官网

http://hc.apache.org/	使用版本: 4.5.13



## 1.2 使用场景

+ 爬虫
+ 多系统之间接口交互



## 1.3 Maven 地址

````xml
<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.13</version>
</dependency>
````





# 二, JDK 原生 API 发送请求

## 2.1 HttpURLConnection

````java
public class HttpURLConnTest {
    /**
     * 使用jdk原生自带的api来请求网页
     */
    @Test
    public void test() throws Exception {
        String urlStr = "https://www.baidu.com/";
        URL url = new URL(urlStr);
        URLConnection urlConnection = url.openConnection();
        HttpURLConnection httpURLConnection = (HttpURLConnection) urlConnection;

        // 设置请求类型
        httpURLConnection.setRequestMethod("GET");
        httpURLConnection.setRequestProperty("Accept-Charset", "utf-8");

        // 获取httpURLConnection的输入流
        try (
                InputStream is = httpURLConnection.getInputStream();
                InputStreamReader isr = new InputStreamReader(is, StandardCharsets.UTF_8);
                BufferedReader br = new BufferedReader(isr);
        ) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        }
    }
}
````



# 三, 发送 Get 请求

## 3.1 无参

````java
package com.huawei.httpstudy;

import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import org.junit.Test;

import java.io.IOException;
import java.nio.charset.StandardCharsets;

public class HttpClientTest {
    /**
     * 使用 HttpClient 发送 get 请求
     */
    @Test
    public void testGet1() {
        // 可关闭的httpclient客户端, 相当于打开的一个浏览器
        CloseableHttpClient closeableHttpClient = HttpClients.createDefault();
        String urlStr = "https://www.baidu.com/";
        // 构造HttpGet对象
        HttpGet httpGet = new HttpGet(urlStr);
        // 可关闭的响应
        CloseableHttpResponse response = null;
        try {
            response = closeableHttpClient.execute(httpGet);
            // 获取响应结果 : entity 为 HttpEntity 的实现类 DecompressingEntity
            // 注: HttpClient 不经可以作为结果, 也可以作为请求参数实体, 有很多的实现
            HttpEntity entity = response.getEntity();
            // 对 HttpEntity 操作的工具类
            String entityStr = EntityUtils.toString(entity, StandardCharsets.UTF_8);
            System.out.println(entityStr);
            // 确保流关闭
            EntityUtils.consume(entity);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (closeableHttpClient != null) {
                try {
                    closeableHttpClient.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (response != null) {
                try {
                    response.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
````



## 3.2 请求头的使用 - 伪装成浏览器, 防盗链

````java
// 解决 HttpClient 被认为不是真人行为
httpGet.addHeader("User-Agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.127 Safari/537.36");
// 防盗链, value: 发生防盗链的网站的url
httpGet.addHeader("Referer", "https://www.baidu.com/");
````



## 3.3 有参

````java
// urlencode: 如果是浏览器的话, 浏览器会帮我们自动给做了
// HttpClient 需要自己处理
String passwordParam = "123+abc 456|789"
passwordParam = URLEncoder.encode(passwordParam, StandardCharsets.UTF_8.name());
````



## 3.4 获取响应头以及相应的 Content-Type

````java
if (HttpStatus.SC_OK == response.getStatusLine().getStatusCode()) {
    Header[] headers = response.getAllHeaders();
    for (Header header : headers) {
        System.out.println("响应头" + header.getName() + "的值是" + header.getValue());
    }
    HttpEntity entity = response.getEntity();
    // ContentType 也可以通过 HttpEntity 快速获取
    Header contentType = entity.getContentType();
    System.out.println("ContentType的值是" + contentType);
    // 对 HttpEntity 操作的工具类
    String entityStr = EntityUtils.toString(entity, StandardCharsets.UTF_8);
    System.out.println(entityStr);
    // 确保流关闭
    EntityUtils.consume(entity);
}
````



## 3.5 保存网络图片保存到本地





## 3.6 设置访问代理





## 3.7 连接超时和读取超时





# 四, 发送 Post 请求

## 4.1 MIME type

+ 含义 : Multipurpose Internet Mail Extensions 即多用途互联网邮件扩展类型

+ 组成

  | 信息头                    | 含义         | 举例                                                         |
  | ------------------------- | ------------ | ------------------------------------------------------------ |
  | MIME-Version              | MIME版本     | 1.0                                                          |
  | Content-Type              | 内容类型     | application/x-www-form-urlencoded, application/json          |
  | Content-Transfer-Encoding | 编码格式     | 8bit, binary                                                 |
  | Content-Disposition       | 内容排列方式 | 上传文件时:<br/>Content-Disposition: form-data; name="fileName"; filename="C\Users\lenovo\Desktop\a.html"<br/>下载文件时需要设置:<br/>Content-Disposition: attachment; filename=URLEncoder.encode("xx.zip", "UTF-8") |

  + 网页form表单enctype可用的MIME类型 (Content-Type类型)
    1. application/x-www-form-urlencoded
    2. multipart/form-data
    3. text/plain



## 4.2 发送application/x-www-form-urlencoded类型的请求





## 4.3 发送application/json类型的请求





## 4.4 发送multipart/form-data类型上传文件的请求







# 五, 请求https连接







# 六, 使用HttpClient连接池







# 七, 封装通用的HttpClientUtil









