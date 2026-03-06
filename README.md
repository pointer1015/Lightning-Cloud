# ⚡ 闪电云盘 (Lightning-Cloud)

> 一个基于 Spring Boot 的个人网盘系统，支持文件上传下载、文件夹管理、文件分享、QQ 第三方登录等功能。

<p align="center">
  <img src="https://img.shields.io/badge/version-2.8-blue" />
  <img src="https://img.shields.io/badge/Spring Boot-2.2.4-brightgreen" />
  <img src="https://img.shields.io/badge/Java-8-orange" />
  <img src="https://img.shields.io/badge/license-MIT-green" />
</p>


---

## 📖 项目介绍

**闪电云盘** 是一个功能完整的个人网盘 Web 应用，用户注册后即可获得专属云存储空间，支持上传、下载、分类管理各种类型的文件，并可通过二维码或分享链接将文件分享给他人。系统提供管理员后台，可对用户权限和存储配额进行管理。

---

## ✨ 功能特性

### 用户系统

- **邮箱注册**：发送邮件验证码完成注册
- **邮箱/密码登录**
- **QQ 第三方登录**（基于 QQ Connect OAuth2）
- 用户头像展示（支持 QQ 头像同步）

### 文件管理

- **文件上传**：支持单文件及大文件上传（最大 1024MB），文件直传 FTP 服务器
- **文件下载**：从 FTP 服务器拉取文件流，支持中文文件名
- **文件删除**：同步删除 FTP 服务器文件及数据库记录
- **文件重命名**：同步更新 FTP 服务器文件名及数据库
- **文件类型分类**：图片、文档、视频、音乐、其他，分类浏览

### 文件夹管理

- 创建/删除/重命名文件夹
- 多级目录结构（树形嵌套）
- 面包屑导航，快速定位当前路径

### 文件分享

- **二维码分享**：为文件生成带 Logo 的分享二维码（基于 ZXing）
- **链接分享**：生成带校验参数的分享链接，无需登录即可下载
- **临时文件分享**：无需注册，上传即获得分享链接/二维码，文件自动清理

### 存储空间管理

- 每用户独立存储仓库，实时统计已用空间
- 由管理员设置用户最大存储配额

### 管理员后台

- 用户列表分页查看
- 修改用户仓库权限（可上传下载 / 仅下载 / 禁止上传下载）
- 修改用户存储配额
- 查看系统文件统计信息

---

## 🛠️ 技术栈

| 层次       | 技术                            |
| ---------- | ------------------------------- |
| 基础框架   | Spring Boot 2.2.4               |
| 持久层     | MyBatis 2.1.1 + MyBatis-EhCache |
| 数据库     | MySQL 8.x                       |
| 数据源     | Alibaba Druid 1.1.20            |
| 分页       | PageHelper 5.1.10               |
| 缓存       | EhCache                         |
| 文件存储   | FTP（Apache Commons-Net 3.6）   |
| 对象存储   | 阿里云 OSS SDK 2.4.0            |
| 模板引擎   | Thymeleaf                       |
| 前端库     | jQuery 3.4.1                    |
| 邮件服务   | Spring Mail（QQ SMTP/SSL）      |
| 第三方登录 | QQ Connect SDK (Sdk4J 2.0)      |
| 二维码     | Google ZXing 3.3.3              |
| 工具库     | Lombok、FastJSON、Jackson       |
| 构建工具   | Maven                           |
| JDK        | Java 8                          |

---

## 📁 项目结构

```
lightning-cloud/
├── src/
│   └── main/
│       ├── java/com/cloud/
│       │   ├── LightningCloudApplication.java   # 启动类
│       │   ├── component/
│       │   │   └── LoginHandlerInterceptor.java # 登录拦截器
│       │   ├── config/
│       │   │   ├── DruidConfig.java             # Druid 数据源配置
│       │   │   └── MvcConfig.java               # MVC 拦截器注册
│       │   ├── controller/
│       │   │   ├── BaseController.java          # Controller 公共基类
│       │   │   ├── LoginController.java         # 登录/注册/QQ登录
│       │   │   ├── FileStoreController.java     # 文件上传/下载/分享
│       │   │   ├── SystemController.java        # 网盘主页/文件列表
│       │   │   └── AdminController.java         # 管理员后台
│       │   ├── entity/                          # 实体类
│       │   │   ├── User.java
│       │   │   ├── FileStore.java
│       │   │   ├── MyFile.java
│       │   │   ├── FileFolder.java
│       │   │   ├── TempFile.java
│       │   │   └── FileStoreStatistics.java
│       │   ├── mapper/                          # MyBatis Mapper 接口
│       │   ├── service/                         # Service 接口及实现
│       │   └── utils/
│       │       ├── FtpUtil.java                 # FTP 工具类
│       │       ├── MailUtils.java               # 邮件工具类
│       │       ├── QRCodeUtil.java              # 二维码生成工具
│       │       └── LogUtils.java                # 日志工具类
│       └── resources/
│           ├── application.yml                  # 应用配置
│           ├── ehcache.xml                      # EhCache 配置
│           ├── qqConnectConfig.properties       # QQ 登录配置
│           ├── mybatis/                         # MyBatis 配置及 Mapper XML
│           ├── static/                          # 静态资源 (css/js/img)
│           └── templates/                       # Thymeleaf 模板页面
├── pom.xml
└── README.md
```

---

## 🚀 快速开始

### 环境要求

- JDK 8+
- Maven 3.6+
- MySQL 5.7+ / 8.x
- FTP 服务器（如 vsftpd）

### 1. 克隆项目

```bash
git clone https://github.com/your-username/lightning-cloud.git
cd lightning-cloud
```

### 2. 初始化数据库

创建数据库并执行建表 SQL（请在 `docs/` 目录或项目 Wiki 中查找建表语句），默认数据库名为 `lightning-cloud`。

```sql
CREATE DATABASE `lightning-cloud` DEFAULT CHARACTER SET utf8mb4;
```

数据库涉及的主要表：

| 表名          | 说明                           |
| ------------- | ------------------------------ |
| `user`        | 用户信息                       |
| `file_store`  | 用户文件仓库（存储配额、权限） |
| `my_file`     | 用户上传的文件记录             |
| `file_folder` | 文件夹信息                     |
| `temp_file`   | 临时文件记录                   |

### 3. 修改配置文件

编辑 `src/main/resources/application.yml`，按实际环境填写：

```yaml
spring:
  datasource:
    username: your_db_user
    password: your_db_password
    url: jdbc:mysql://127.0.0.1:3306/lightning-cloud?serverTimezone=Hongkong&useAffectedRows=true

  mail:
    username: your_email@qq.com
    password: your_smtp_auth_code   # QQ 邮箱授权码（非登录密码）
    host: smtp.qq.com
    port: 465
```

编辑 `src/main/resources/qqConnectConfig.properties`，填入 QQ 互联平台申请的应用信息：

```properties
app_ID = your_qq_app_id
app_KEY = your_qq_app_key
redirect_URI = http://your-domain/lightning-cloud/connection
```

### 4. 配置 FTP 服务器

在 `FtpUtil.java` 中修改 FTP 服务器地址、端口、用户名和密码：

```java
private static String FTP_HOST = "your_ftp_host";
private static int FTP_PORT = 21;
private static String FTP_USERNAME = "your_ftp_user";
private static String FTP_PASSWORD = "your_ftp_password";
```

### 5. 编译打包

```bash
mvn clean package -DskipTests
```

### 6. 运行

```bash
java -jar target/lightning-cloud-2.8.jar
```

访问地址：[http://localhost:8080/lightning-cloud](http://localhost:8080/lightning-cloud)

---

## 📸 页面预览

| 页面             | 说明                             |
| ---------------- | -------------------------------- |
| `/`              | 登录 / 注册首页                  |
| `/index`         | 用户主页（仓库概览）             |
| `/files`         | 我的网盘（文件/文件夹列表）      |
| `/temp-file`     | 临时文件分享                     |
| `/manages-users` | 管理员用户管理页（需管理员权限） |

---

## 🔒 权限说明

| 角色     | role 值 | 说明                         |
| -------- | ------- | ---------------------------- |
| 管理员   | `0`     | 可访问管理后台，管理所有用户 |
| 普通用户 | `1`     | 正常使用网盘功能             |

**仓库权限（permission）：**

| 值   | 说明                   |
| ---- | ---------------------- |
| `0`  | 正常（可上传 & 下载）  |
| `1`  | 只读（仅允许下载）     |
| `2`  | 封禁（禁止上传和下载） |

---

## 📦 主要依赖

```xml
<!-- Spring Boot Web + Thymeleaf -->
<!-- MyBatis + PageHelper + EhCache -->
<!-- Druid 数据源 -->
<!-- MySQL 驱动 -->
<!-- Apache Commons-Net (FTP) -->
<!-- 阿里云 OSS SDK -->
<!-- QQ Connect SDK (Sdk4J) -->
<!-- Google ZXing (二维码) -->
<!-- Spring Mail -->
<!-- Lombok + FastJSON -->
```

完整依赖见 [pom.xml](pom.xml)。

---

## ⚙️ 配置项说明

| 配置项       | 位置                         | 说明              |
| ------------ | ---------------------------- | ----------------- |
| 数据库连接   | `application.yml`            | MySQL 连接信息    |
| 邮件 SMTP    | `application.yml`            | QQ 邮箱 SMTP 配置 |
| Session 超时 | `application.yml`            | 默认 60 分钟      |
| 最大上传文件 | `application.yml`            | 默认 1024MB       |
| FTP 服务器   | `FtpUtil.java`               | FTP 连接信息      |
| QQ 登录      | `qqConnectConfig.properties` | QQ 互联应用信息   |
| 缓存策略     | `ehcache.xml`                | EhCache 缓存配置  |

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建你的 Feature 分支：`git checkout -b feature/your-feature`
3. 提交更改：`git commit -m 'feat: add some feature'`
4. 推送分支：`git push origin feature/your-feature`
5. 提交 Pull Request

---

## 📄 许可证

本项目基于 [MIT License](LICENSE) 开源，欢迎自由使用和修改。

---

## 📬 联系方式

如有问题，欢迎通过 GitHub Issues 联系。
