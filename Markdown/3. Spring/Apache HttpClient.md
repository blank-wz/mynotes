# 一, HttpClient 简介

+ 官网: http://hc.apache.org/	使用版本: 4.5.13

+ 使用场景

  + 爬虫
  + 多系统之间接口交互

+ Maven

  ````xml
  <!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
  <dependency>
      <groupId>org.apache.httpcomponents</groupId>
      <artifactId>httpclient</artifactId>
      <version>4.5.13</version>
  </dependency>
  ````

  



# 二, JDK 原生 API 发送请求

+ HttpURLConnection

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

