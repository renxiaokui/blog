---
title: SpringCloud
date: 2020-04-10 16:20:18
tags: 
    - springcloud
categories:
    - springcloud
---

# springcloud 学习

## springcloud版本查看和选择

1. [springcloud官网](https://spring.io/projects/spring-cloud)

2. 找到你想要的springcloud的版本信息 GA对应的是正式版 选择这个即可

   {% asset_img springcloud_version1.png springcloud版本 %}

3. 点击[ Reference Doc.](https://cloud.spring.io/spring-cloud-static/Hoxton.SR3/reference/html/spring-cloud.html)可以看到springcloud版本和对应的springboot版本

   {% asset_img springcloud_version2.png springcloud版本 %}



## springcloud pom工程搭建





## springcloud oauth2 教程

[github代码示例](https://github.com/renxiaokui/springcloud-oauth2-demo)

密码模式：

{% asset_img oauth1.jpg 密码模式 %}



客户端模式：

{% asset_img oauth2.jpg 客户端模式 %}



授权码模式：

获取授权码连接：

http://localhost:8001/oauth/authorize?client_id=client_3&redirect_uri=http://baidu.com&response_type=code&scope=read

{% asset_img oauth3.jpg 授权码模式 %}



简介：

认证和授权的框架；也可以做单点登录功能；第三方授权，获取用户信息，例如微信 微博 qq授权登录

核心配置继承**AuthorizationServerConfigurerAdapter**和**WebSecurityConfigurerAdapter**

AuthorizationServerConfigurerAdapter 类中3个不同的configure方法分别

我们授权别人，那么就需要一个客户端凭证，凭证通过了给他一个token，所以需要以下配置

- configure(ClientDetailsServiceConfigurer clients) 用来配置客户端（ClientDetailsService）

  这里可以写死固定的客户端，也可以在数据库动态做配置

- configure(AuthorizationServerEndpointsConfigurer endpoints) 

  用来配置授权（authorization）以及令牌（token）的访问端点和令牌服务(token services)，还有token的存储方式(tokenStore)；

- configure(AuthorizationServerSecurityConfigurer security) 

  用来配置令牌端点(Token Endpoint)的安全约束；

WebSecurityConfigurerAdapter

- configure(HttpSecurity http) httpSecurity中配置所有请求的安全验证
- 注入Bean UserDetailsService
- 注入Bean AuthenticationManager 用来做验证
- 注入Bean PasswordEncoder

### 使用jwt token

- ####  对称加解密算法

  ```
  #授权服务器
  @Bean
      public JwtAccessTokenConverter jwtAccessTokenConverter(){
          JwtAccessTokenConverter converter = new JwtAccessTokenConverter();
          converter.setSigningKey("my-sign-key"); //资源服务器需要配置此选项方能解密jwt的token
          return converter;
      }
  #资源服务
  @Bean
      public JwtAccessTokenConverter jwtAccessTokenConverter(){
          JwtAccessTokenConverter converter = new JwtAccessTokenConverter();
          converter.setSigningKey("my-sign-key"); //与授权服务器相同的signingKey
          return converter;
      }
  ```

- #### 对称加解密算法

  授权服务器oauth配置

  ```
  #使用java自带工具keytool生成jks (Java Key Store) 密钥
  keytool -genkeypair -alias projectkey -validity 3650 -keyalg RSA -keypass projectpassword -keystore projectkey.jks -storepass projectpassword
  
  - genkeypair：生成公私钥对条目，私钥不可见，公钥会以证书格式保存在keystore中。
  - alias: 指定别名，区分不同条目(默认mykey)
  - keysize: 密钥长度(默认1024)
  - keyalg: 公私钥算法
  - validity: 证书过期时间(默认90天)
  - keystore: 指定密钥库的名称
  - storetype: 密钥库类型  JKS PKCS等
  - storepass 指定密钥库的密码(获取keystore信息所需的密码)
  - keypass 指定别名条目的密码(私钥的密码)
  ```

  ```
  #yml文件配置
  encrypt:
    key-store:
      location: classpath:/changgou66.jks
      secret: projectpassword
      alias: projectkey
      password: projectpassword
  
    //读取密钥的配置
      @Bean("keyProp")
      public KeyProperties keyProperties(){
          return new KeyProperties();
      }
  
      @Resource(name = "keyProp")
      private KeyProperties keyProperties;
      
   @Bean
      public JwtAccessTokenConverter jwtAccessTokenConverter(CustomUserAuthenticationConverter customUserAuthenticationConverter) {
          JwtAccessTokenConverter converter = new JwtAccessTokenConverter();
          KeyPair keyPair = new KeyStoreKeyFactory(
                  keyProperties.getKeyStore().getLocation(),         //证书路径 
                  keyProperties.getKeyStore().getSecret().toCharArray()) //证书秘钥
                  .getKeyPair(
                     keyProperties.getKeyStore().getAlias(), //证书别名 changgou
                     keyProperties.getKeyStore().getPassword().toCharArray()); //证书密码 
          converter.setKeyPair(keyPair);
          return converter;
      }
  ```

  资源服务器配置

  [openssl工具下载](http://slproweb.com/products/Win32OpenSSL.html)

  安装该工具并配置环境变量

  ```
  #用私钥生成公钥
  keytool -list -rfc --keystore projectkey.jks | openssl x509 -inform pem -pubkey
  ```

  {% asset_img jks1.jpg 公钥 %}

  ```
    //公钥
      private static final String PUBLIC_KEY = "public.key";
  /**
       * 获取非对称加密公钥 Key
       * @return 公钥 Key
       */
      private String getPubKey() {
          Resource resource = new ClassPathResource(PUBLIC_KEY);
          try (InputStreamReader inputStreamReader = new   			InputStreamReader(resource.getInputStream());
               BufferedReader br = new BufferedReader(inputStreamReader)) {
              return br.lines().collect(Collectors.joining("\n"));
          } catch (Exception ioe) {
              throw new RuntimeException("公钥未找到");
          }
      }
     @Bean
      public JwtAccessTokenConverter jwtAccessTokenConverter() {
          JwtAccessTokenConverter converter = new JwtAccessTokenConverter();
  	    String publicKey = getPubKey();
  	    converter.setVerifierKey(publicKey);
  	    //不设置这个会出现 Cannot convert access token to JSON
  	    converter.setVerifier(new RsaVerifier(publicKey));
          return converter;
      }
  ```

  

