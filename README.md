# 表情购（EmojiBuy）
## 项目介绍
这是一款基于Spring Boot框架搭建的表情包商店APP，实现了丰富的功能，包括表情包查询、优惠券秒杀、发布点评、点赞关注、Feed流推送等。
用户可以在APP中浏览各种热门表情包，
APP还实现了优惠券秒杀，用户可以在秒杀活动期间获得超值的优惠券，从而享受更加优惠的购物体验。
同时，用户还可以在APP中发布自己的点评，分享自己使用表情包的感受，以及点赞和关注其他用户的点评。
作为一款社交型应用，该APP还实现了Feed流推送的功能，用户可以在APP中浏览其他用户的动态和点评，了解最新的表情包和活动信息，增加用户的互动和社交体验。

## 项目特点
该项目主要利用Redis实现了一系列常见功能，如登录分布式session；热点表情包key缓存；对非法key缓存空对象解决缓存穿透；使用逻辑过期
机制解决热点key缓存击穿问题；zset实现排行榜；set交集实现共同关注；
乐观锁解决了超卖问题、synchronized实现一人一单；
并进行了异步改造，用Redis + Lua脚本实现库存预检，stream消息队列实现异步下单。提高了应用的性能和可靠性。
项目采用了Spring Boot框架，代码高效、简便，同时还实现了前后端分离并符合RESTful风格，使得应用具有更好的扩展性和可维护性。


### 后端技术栈

* 主语言：Java
* 框架：SpringBoot 2.7.0、Mybatis-plus、Spring Cloud
* 数据库：Mysql8.0、Redis



### 页面展示

主页/登录页/类别页


![描述](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606091354.png)![描述](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606091629.png)![描述](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606091700.png)


表情包详情页/优惠券抢购

![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606093006.png)![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606093308.png)![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606093315.png)

表情包发布/主页展示

![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606095454.png)![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606101527.png)![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606101544.png)

共同关注/新消息推送

![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606100923.png)![](https://raw.githubusercontent.com/jackyrwj/picb/master/20230606101445.png)

### 项目结构
```
emojibuy
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
│  │  │          │
│  │  │          ├─entity
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
│  │
│  └─test
│
└─target
```

## Quick Start

1. 配置Redis（需要6.5及以上，保证stream命令可用），运行测试代码提前加载表情包key

2. 修改项目的端口、密码

3. 启动后端服务





