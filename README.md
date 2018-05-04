### 饮水思源

---  

原项目：https://github.com/Lovnx/micro-service  
Ribbon负载均衡策略配置：https://blog.csdn.net/rickiyeat/article/details/64918756
  

### 一、搭建一个配置中心的测试环境  

---  

#### 搭建架构图
![avata](http://p7240jy2w.bkt.clouddn.com/spring-cloud-demo%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83%E6%9E%B6%E6%9E%84%E5%9B%BE.png)

#### 搭建系统步骤
> eureka-sever  
> config-server  
> service-one  

#### 启动步骤
eureka-sever -> config-server -> service-one

#### 配置中心的测试方法  
访问service-one服务：http://localhost:7074/from   
访问service-one服务：http://localhost:7074/addition?a=1&b=2

#### 什么时候去取配置信息的呢？实时去取吗？  
通过修改文件demo-dev.properties得知，并不是实时取的。  
修改配置后，重启项目配置才生效。改配置还需要重启项目吗？

### 二、搭建负载均衡的环境  

---  

#### 搭建架构图
![avata](http://p7240jy2w.bkt.clouddn.com/spring-cloud-ribbon%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E6%9E%B6%E6%9E%84.png)
#### 搭建系统步骤
> service-two  
> service-two1  
> ribbon  

#### 启动流程
依赖环境：redis、mysql  
eureka-sever -> ribbon -> service-two -> service-two1  

#### 负载均衡测试
访问ribbon服务器：http://localhost:7071/addition?a=1&b=1  

### 三、搭建路由链路的环境 
#### 搭建系统步骤
> zuul  

#### Zuul组件的链路图
![avata](http://p7240jy2w.bkt.clouddn.com/zuul%E7%BB%84%E4%BB%B6%E7%9A%84%E9%93%BE%E8%B7%AF%E5%9B%BE.png)

#### 搭建架构图
![avata](http://p7240jy2w.bkt.clouddn.com/spring-cloud-zuul%E6%9E%B6%E6%9E%84.png)

#### 启动流程
eureka-sever -> config-server -> service-one -> service-two -> service-two1 -> zuul  

#### 路由链路的测试
访问service-one服务器：http://localhost:7073/api-one/addition?a=1&b=1
访问service-two服务器（会随机分发请求到2台服务器）：http://localhost:7073/api-two/addition?a=1&b=1

问题：如何把zuul和ribbon结合起来呢？  


### 四、搭建spring boot admin监控台的环境  
#### 搭建系统步骤
> service-admin  

#### 启动流程
eureka-sever -> service-admin -> service-two  

#### spring boot admin监控台的测试
访问admin系统：http://localhost:7088


### 五、熔断环境搭建
#### 搭建系统步骤
> feign

#### 启动流程  
eureka-sever -> service-two -> ribbon -> feign

#### 熔断测试
访问熔断配置的接口：http://localhost:7078/addition?a=1&b=1


### 项目描述  
#### 端口
```
eureka-sever      7070  
config-server     7072  
service-one       7074  
service-two       7075  
service-two1      7067  
ribbon            7071  
zuul              7073 
service-admin     7088
feign             7078
```