# spring-cloud-vue
---

## 项目简介
* cloud-vue是一套基于springcloud + mybatis + vue全家桶（Vue2.x + Vue-router2.x + Vuex）的前后端分离框架.
* 使用Maven对项目进行模块化管理，提高项目的易开发性、扩展性。
* 系统包括分布式配置、eureka注册中心、服务中心、zipkin分布式跟踪等。
* 每个模块服务多系统部署，注册到同一个eureka集群服务注册中心，实现集群部署。

## 主要功能
* 登录、退出登录
* 修改密码、记住密码
* 菜单管理
* 系统参数
* 权限节点
* 岗位管理
* 部门管理
* 用户组管理
* 用户管理

## 依赖
### java后端依赖环境
* Maven 3
* Java 8
* MySQL 5.7
* Docker 1.13.1 (不是必须的)

### vue2前端依赖环境
* node >= 6.9.0
* npm  >= 3.0.0
* vue 				<https://vuefe.cn/v2/guide/>
* element-ui@1.1.3  <http://element.eleme.io/1.1/#/zh-CN/component/installation>
* axios  			<https://github.com/mzabriskie/axios>
* fontawesome 		<http://fontawesome.io/icons/>
* js-cookie  		<https://github.com/js-cookie/js-cookie>
* lockr  			<https://github.com/tsironis/lockr>
* lodash  			<http://lodashjs.com/docs/>
* moment  			<http://momentjs.cn/>

## 工程说明
* cloud-config-server：配置中心。
* cloud-eureka-server：注册中心。
* cloud-simple-service：自定义的微服务。
* cloud-zipkin-ui：分布式链路调用监控系统，聚合各业务系统调用延迟数据，达到链路调用监控跟踪。
* cloud-vue : vue（Vue2.x + Vue-router2.x + Vuex)的前端项目

## 部署说明
 * 导入cloud-simple-service的cloud-vue.sql到mysql数据库。
 * 修改cloud-config-repo与cloud-zipkin-ui中的数据库配置文件
 * 打包命令 mvn package -DskipDockerBuild
 * 依次启动cloud-eureka-server-1.0.0.jar、cloud-config-server-1.0.0.jar、cloud-zipkin-ui-1.0.0.jar、cloud-simple-service-1.0.0.jar。
 * 端口：配置中心端口(1111)、注册中心(8888)、rest服务(80)、zipkin服务(9012)、UI前端(8080),如果端口冲突请自行修改。

## 效果图
![登录](./doc/登录.png)

![部门管理](./doc/部门管理.png)

![部门管理](./doc/部门管理.png)

![菜单管理](./doc/菜单管理.png)

![岗位管理](./doc/岗位管理.png)

![权限规则管理](./doc/权限规则管理.png)

![用户组管理](./doc/用户组管理.png)

![注册中心](./doc/注册中心.png)

![swagger](./doc/swagger.png)

![zipkin](./doc/zipkin.png)

## License
cloud-vue 基于apache2.0 <http://www.apache.org/licenses/LICENSE-2.0>

如果项目对您有用，请作者喝杯咖啡吧！

