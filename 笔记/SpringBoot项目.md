# 目的

- Spring Boot
- Spring MVC
- MyBatis
- MySQL/H2
- FlyWay
- Heroku
- Git/[Github](https://www.bootcss.com/)
- Maven
- Restful

# 明确需求

![](D:\JAVA基础\Git\community1\模板.PNG)

# BootStrap

- https://www.bootcss.com/

## 登录功能

- 接入GitHub的登录功能
- https://developer.github.com/apps/
- 1.Creating a GitHub App

### 登录

- ### 1. Request a user's GitHub identity

  ```
  GET https://github.com/login/oauth/authorize
  ```

- 登录按钮绑定一个地址->点击这个地址可以跳转到```GET https://github.com/login/oauth/authorize```

```html
<li><a href="https://github.com/login/oauth/authorize?cilent_id=724959b413e6359b2b49&redirect_uri=http://localhost:8088/callback&scope=user&">登录</a></li>
```

#### Parameters

|      Name      |   Type   |                         Description                          |
| :------------: | :------: | :----------------------------------------------------------: |
|  `client_id`   | `string` | **Required**. The client ID you received from GitHub when you [registered](https://github.com/settings/applications/new). |
| `redirect_uri` | `string` | The URL in your application where users will be sent after authorization. See details below about [redirect urls](https://developer.github.com/apps/building-oauth-apps/authorizing-oauth-apps/#redirect-urls). |
|    `login`     | `string` | Suggests a specific account to use for signing in and authorizing the app. |
|    `scope`     | `string` | A space-delimited list of [scopes](https://developer.github.com/apps/building-oauth-apps/understanding-scopes-for-oauth-apps/). If not provided, `scope` defaults to an empty list for users that have not authorized any scopes for the application. For users who have authorized scopes for the application, the user won't be shown the OAuth authorization page with the list of scopes. Instead, this step of the flow will automatically complete with the set of scopes the user has authorized for the application. For example, if a user has already performed the web flow twice and has authorized one token with `user` scope and another token with `repo` scope, a third web flow that does not provide a `scope` will receive a token with `user` and `repo` scope. |
|    `state`     | `string` | An unguessable random string. It is used to protect against cross-site request forgery attacks. |
| `allow_signup` | `string` | Whether or not unauthenticated users will be offered an option to sign up for GitHub during the OAuth flow. The default is `true`. Use `false` when a policy prohibits signups. |

## 2.Users are redirected back to your site by GitHub

- AuthorizeController
  - 作用：callbackController+callbackAccessToken等等
- @Controller注解：把当前的类作为整个路由的承载者

- @Component注解：把当前的类初始到Spring容器的上下文(调用的时候不需要实例化对象)
- 通过@Compnent注解已经把其注解对象放入了Spring容器通过@Autowired自动加载到当前的上下文

#### Parameters

|      Name       |   Type   |                         Description                          |
| :-------------: | :------: | :----------------------------------------------------------: |
|   `client_id`   | `string` | **Required.** The client ID you received from GitHub for your GitHub App. |
| `client_secret` | `string` | **Required.** The client secret you received from GitHub for your GitHub App. |
|     `code`      | `string` | **Required.** The code you received as a response to Step 1. |
| `redirect_uri`  | `string` | The URL in your application where users are sent after authorization. |
|     `state`     | `string` |    The unguessable random string you provided in Step 1.     |

- 参数超过两个封装成对象去做

- ```java
  package cn.cqu.ccc1z.community.dto;
  
  /**
   * Created by Ccc1Z on 2019/03/22
   *
   * Some data transfer objects used to accesstoken
   */
  public class AccessTokenDTO {
      private String client_id;
      private String client_secret;
      private String code;
      private String redirect_uri;
      private String state;
  
      public String getClient_id() {
          return client_id;
      }
  
      public void setClient_id(String client_id) {
          this.client_id = client_id;
      }
  
      public String getClient_secret() {
          return client_secret;
      }
  
      public void setClient_secret(String client_secret) {
          this.client_secret = client_secret;
      }
  
      public String getCode() {
          return code;
      }
  
      public void setCode(String code) {
          this.code = code;
      }
  
      public String getRedirect_uri() {
          return redirect_uri;
      }
  
      public void setRedirect_uri(String redirect_uri) {
          this.redirect_uri = redirect_uri;
      }
  
      public String getState() {
          return state;
      }
  
      public void setState(String state) {
          this.state = state;
      }
  }
  ```

- Exchange this `code` for an access token:

  ```
  POST https://github.com/login/oauth/access_token
  ```

- 使用OkHttp进行POST

- ## Post to a Server[¶](https://square.github.io/okhttp/#post-to-a-server)

  This program posts data to a service.

  ```java
  public static final MediaType JSON
      = MediaType.get("application/json; charset=utf-8");
  
  OkHttpClient client = new OkHttpClient();
  
  String post(String url, String json) throws IOException {
    RequestBody body = RequestBody.create(json, JSON);
    Request request = new Request.Builder()
        .url(url)
        .post(body)
        .build();
    try (Response response = client.newCall(request).execute()) {
      return response.body().string();
    }
  }
  ```

- 使用maven导入jar包

```xml
<dependency>  
  <groupId>com.squareup.okhttp3</groupId>
  <artifactId>okhttp</artifactId>
  <version>4.4.0</version>
</dependency>
```

## 3.Use the access token to access the API

- The access token allows you to make requests to the API on a behalf of a user.

```
Authorization: token OAUTH-TOKEN
GET https://api.github.com/user
```

- For example, in curl you can set the Authorization header like this:

```
curl -H "Authorization: token OAUTH-TOKEN" https://api.github.com/user
```



## 配置Application properties

- @Value注解：

- ```java
  @Value("${github.client.id}")
  private String clientId;
  ```

- 会到配置文件中去找{}中的内容，类似于@Autowired



## 细说Session和Cookies

- 目的：登录成功后保证登录态

- Session就是你的银行账户：开户后银行会留有你的消息
- Cookie就是你的银行卡，有了银行卡后台才能拿到你的账户操作你的信息
- 浏览器就相当于用户，服务器就相当于银行
- **http本身是无状态的，该怎么让http(服务器)记住你的状态->携带银行卡**

![](C:\Users\44766\Desktop\Cookie.PNG)



- session是在 ```HttpServletRequest requset```中拿到的

## 保持持久登录态(数据库)

- 在maven仓库找到h2,引入h2依赖

```sql
CREATE TABLE PUBLIC.user
(
   id int auto_increment,
   account_id VARCHAR(100),
   name VARCHAR(50),
   token CHAR(36),
   gmt_create BIGINT,
   gmt_modified BIGINT,
   constraint user_pk
      primary key (id)
);
```

- 创建一张user表
- gmt_create代表格林威治时间的事件创建时间戳
- gmt_modified代表格林威治时间的事件修改时间戳
- token代表cookie
- BIGINT对应java的long类型

### 集成MyBatis并实现插入功能

http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/



- 类和类之间传输用DTO
- 数据库中是model

```java
/**
 * Created by Ccc1Z on 2020/03/23
 */
@Mapper
public interface UserMapper {
    @Insert("INSERT INTO user (name,account_id,token,gmt_create,gmt_modified) values (#{name})")
    public void insert(User user);
}
```

- 当执行这条语句时,MyBaties会把user属性自动替换掉```#{name}```

```java
if(githubUser != null){
    User user = new User();
    user.setToken(UUID.randomUUID().toString());
    user.setName(githubUser.getName());
    user.setAccount_id(String.valueOf(githubUser.getId()));
    user.setGmt_create(System.currentTimeMillis());
    user.setGmt_modified(user.getGmt_create();
    userMapper.insert(user);
```



```
spring.datasource.url=jdbc:mysql:~/community1
```



## 集成Flyway Migration

- 解决多人开发数据库版本不统一的问题











## 一些IDEA快捷键

- ctrl+alt+v->快速新建对象

- ```java
  githubProvider.getAccessToken(new AccessTokenDTO());
  ```

- 对new AccessTokenDto()使用快捷键会得到下面的代码：

- ```java
  AccessTokenDTO accessTokenDTO = new AccessTokenDTO();
  githubProvider.getAccessToken(accessTokenDTO);
  ```

- 