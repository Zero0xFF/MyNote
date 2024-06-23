# 搭建Sport Store环境

## Download docker image

```
docker pull mysql:5.5.62
```

## Build mysql by docker

```
docker run -p 3306**:**3306 --name my_mysql -e MYSQL_ROOT_PASSWORD=P@ssw0rd -d mysql:mysql:5.5.62
```

## Install Navicat

[Navicat_16.3.9](https://navicat.com/en/download/navicat-premium)

[Navicat Crack](https://github.com/LiJunYi2/navicat-keygen-16V/releases)

## Build database and import .sql script

![image-20240420044252631](./assets/image-20240420044252631.png)

## 导入IDEA

![image-20240420044335372](./assets/image-20240420044335372.png)

## 解决冲突

### 修复tomcat路径

由于我们本地tomcat7，tomcat8路径与项目配置tomcat78路径不一致导致错误。

![image-20240420044643929](./assets/image-20240420044643929.png)

修复部署问题

![image-20240420045758409](./assets/image-20240420045758409.png)

### 修复ApplicationContext

![image-20240420050318942](./assets/image-20240420050318942.png)

![image-20240420060415933](./assets/image-20240420060415933.png)

### 修复tomcat lib路径

由于我们更换了本地tomcat，因此项目tomcat lib路径需要更改。

![image-20240420055226278](./assets/image-20240420055226278.png)

更改后tomcat lib路径如图下图所示

![image-20240420055416653](./assets/image-20240420055416653.png)

### 修改数据库登录信息

![image-20240420060625977](./assets/image-20240420060625977.png)

## 成功登录

![image-20240420060905991](./assets/image-20240420060905991.png)

![image-20240420061056171](./assets/image-20240420061056171.png)