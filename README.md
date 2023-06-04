### 项目结构
```
├─src
│  ├─main
│  │  ├─java
│  │  │  └─com
│  │  │      └─hmdp
│  │  │          │  HmDianPingApplication.java
│  │  │          │
│  │  │          ├─config
│  │  │          │      MvcConfig.java
│  │  │          │      MybatisConfig.java
│  │  │          │      RedissonConfig.java
│  │  │          │      WebExceptionAdvice.java
│  │  │          │      WebMvcConfig.java
│  │  │          │
│  │  │          ├─controller
│  │  │          │      BlogCommentsController.java
│  │  │          │      BlogController.java
│  │  │          │      FollowController.java
│  │  │          │      ShopController.java
│  │  │          │      ShopTypeController.java
│  │  │          │      UploadController.java
│  │  │          │      UserController.java
│  │  │          │      VoucherController.java
│  │  │          │      VoucherOrderController.java
│  │  │          │
│  │  │          ├─dto
│  │  │          │      LoginFormDTO.java
│  │  │          │      Result.java
│  │  │          │      ScrollResult.java
│  │  │          │      UserDTO.java
│  │  │          │
│  │  │          ├─entity
│  │  │          │      Blog.java
│  │  │          │      BlogComments.java
│  │  │          │      Follow.java
│  │  │          │      SeckillVoucher.java
│  │  │          │      Shop.java
│  │  │          │      ShopType.java
│  │  │          │      User.java
│  │  │          │      UserInfo.java
│  │  │          │      Voucher.java
│  │  │          │      VoucherOrder.java
│  │  │          │
│  │  │          ├─mapper
│  │  │          │      BlogCommentsMapper.java
│  │  │          │      BlogMapper.java
│  │  │          │      FollowMapper.java
│  │  │          │      SeckillVoucherMapper.java
│  │  │          │      ShopMapper.java
│  │  │          │      ShopTypeMapper.java
│  │  │          │      UserInfoMapper.java
│  │  │          │      UserMapper.java
│  │  │          │      VoucherMapper.java
│  │  │          │      VoucherOrderMapper.java
│  │  │          │
│  │  │          ├─service
│  │  │          │  │  IBlogCommentsService.java
│  │  │          │  │  IBlogService.java
│  │  │          │  │  IFollowService.java
│  │  │          │  │  ISeckillVoucherService.java
│  │  │          │  │  IShopService.java
│  │  │          │  │  IShopTypeService.java
│  │  │          │  │  IUserInfoService.java
│  │  │          │  │  IUserService.java
│  │  │          │  │  IVoucherOrderService.java
│  │  │          │  │  IVoucherService.java
│  │  │          │  │
│  │  │          │  └─impl
│  │  │          │          BlogCommentsServiceImpl.java
│  │  │          │          BlogServiceImpl.java
│  │  │          │          FollowServiceImpl.java
│  │  │          │          SeckillVoucherServiceImpl.java
│  │  │          │          ShopServiceImpl.java
│  │  │          │          ShopTypeServiceImpl.java
│  │  │          │          UserInfoServiceImpl.java
│  │  │          │          UserServiceImpl.java
│  │  │          │          VoucherOrderServiceImpl.java
│  │  │          │          VoucherServiceImpl.java
│  │  │          │
│  │  │          └─utils
│  │  │                  CacheClient.java
│  │  │                  ILock.java
│  │  │                  LoginInterceptor.java
│  │  │                  PasswordEncoder.java
│  │  │                  RedisConstants.java
│  │  │                  RedisData.java
│  │  │                  RedisIdWorker.java
│  │  │                  RefreshTokenInterceptor.java
│  │  │                  RegexPatterns.java
│  │  │                  RegexUtils.java
│  │  │                  SimpleRedisLock.java
│  │  │                  SystemConstants.java
│  │  │                  UserHolder.java
│  │  │
│  │  └─resources
│  │      │  application-prod.yaml
│  │      │  application.yaml
│  │      │  seckill.lua
│  │      │  unlock.lua
│  │      │
│  │      ├─db
│  │      │      hmdp.sql
│  │      │
│  │      └─mapper
│  │              VoucherMapper.xml
│  │
│  └─test
│      └─java
│          └─com
│              └─hmdp
│                      HmDianPingApplicationTests.java
│                      NormalTest.java
│                      RedissonTest.java
│
└─target
```




## 遇到的问题：
### 1.GEOADD命令无法使用
![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230601092430.png)
原因是redis版本的问题，GEO功能是在redis3.2出来的
> 登录客户端后使用命令"info server" 打印redis服务端版本
![img.png](img.png)

### 2.gitignore失效
项目的前端中图片文件夹无需上传，配置gitignore后发现并为生效，一顿摸索时候发现：
项目初始化后 .gitignore需要先commit一次，后面的才能扫描文件夹
 
![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230601093108.png)

### 3. git commit后，撤销commit失败
正常来说，普通的撤销 使用命令：
```java
git reset --soft HEAD^
```
HEAD^ 表示上一个版本，即上一次的commit，也可以写成HEAD~1
如果进行两次的commit，想要都撤回，可以使用HEAD~2
不删除工作空间的改动代码 ，撤销commit，不撤销add，使用--soft
删除工作空间的改动代码，撤销commit且撤销add，使用--hard

另外一点，如果commit注释写错了，先要改一下注释，有其他方法也能实现，如：
```agsl
git commit --amend
```
但是❗❗❗，如果第一次commit，用上述方法是无法撤销的，可以使用
```java
git update-ref -d HEAD
```
命令来实现想要的效果。尝试过后，发现commit被成功撤销，仍然保留了add后的结果。

测试了好多次commit才解决 😭

![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230601093915.png)

### 4.Redis线上数据同步失败
❗❗❗ 宝塔面板自带的redis下dump.rdb不在默认路径下，将本地数据同步到线上的时候要注意（沉痛教训😭，redis托管容易埋坑）
![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230603101312.png)

另外在使用redis消息队列时，线上别忘记也要先建好消息队列
```JAVA
XGROUP CREATE stream.orders g1 0 MKSTREAM
```

### 5.拦截器错误
Springboot框架中通过实现WebMvcConfigurer接口中的addInterceptors来实现拦截器的效果，
有正向拦截(addPathPatterns)和反向拦截(excludePathPatterns),实际使用来反向拦截很有可能翻车，
因为配置的是"不拦截"的，很有可能大部分正常的页面会被拦截，
❗❗❗ 另外,即使配置了返回的是401"无权限",但会被浏览器识别成跨域（还不知道导致的）
最后还是通过正向拦截解决了orz。
```java
public class MvcConfig implements WebMvcConfigurer {

    @Resource
    private StringRedisTemplate stringRedisTemplate;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 登录拦截器
//        registry.addInterceptor(new LoginInterceptor())
//                .excludePathPatterns(
//                        "/shop/**",
//                        "/voucher/**",
//                        "/shop-type/**",
//                        "/upload/**",
//                        "/blog/hot",
//                        "/user/code",
//                        "/user/login"
//                ).order(1);
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns(
                        "/follow/**/**", 
                        "/voucher-order/**"
                ).order(1);
        // token刷新的拦截器
        registry.addInterceptor(new RefreshTokenInterceptor(stringRedisTemplate)).addPathPatterns("/**").order(0);
    }
}
```