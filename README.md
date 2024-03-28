> **免责声明：**

>本仓库的所有内容仅供学习和参考之用，禁止用于商业用途。任何人或组织不得将本仓库的内容用于非法用途或侵犯他人合法权益。本仓库所涉及的爬虫技术仅用于学习和研究，不得用于对其他平台进行大规模爬虫或其他非法行为。对于因使用本仓库内容而引起的任何法律责任，本仓库不承担任何责任。使用本仓库的内容即表示您同意本免责声明的所有条款和条件。

# 仓库描述

**小红书爬虫**，**抖音爬虫**， **快手爬虫**， **B站爬虫**， **微博爬虫**...。  
目前能抓取小红书、抖音、快手、B站、微博的视频、图片、评论、点赞、转发等信息。

原理：利用[playwright](https://playwright.dev/)搭桥，保留登录成功后的上下文浏览器环境，通过执行JS表达式获取一些加密参数
通过使用此方式，免去了复现核心加密JS代码，逆向难度大大降低  

爬虫技术交流群：[949715256](http://qm.qq.com/cgi-bin/qm/qr?_wv=1027&k=NFz-oY7Pek3gpG5zbLJFHARlB8lKL94f&authKey=FlxIQK99Uu90wddNV5W%2FBga6T6lXU5BRqyTTc26f2P2ZK5OW%2BDhHp7MwviX%2BbrPa&noverify=0&group_code=949715256)，同时欢迎大家贡献代码提交PR

视频配置教程：[MediaCrawler视频入门教程](https://space.bilibili.com/434377496/channel/series)

## 感谢下列Sponsors对本仓库赞助
<a href="https://dashboard.ipcola.com/register?referral_code=vkybwyucyuidpne">全球ip代理超新星</a>
<a href="https://dashboard.ipcola.com/register?referral_code=vkybwyucyuidpne" target="_blank"><img src="https://s2.loli.net/2024/03/18/LKJaWcIHQl92ip5.jpg" alt="IPCola,  全球ip代理超新星-官网图"></a><br>
<br>
<a href="https://monica.im/invitation?c=4HCSQRYS">你也可以通过注册这款免费的ChatGPT产品，帮我获取额外的GPT-4额度作为支持，也是我每天都在用的一款chrome效率插件，推荐给你，你也能获得免费额度。</a>
<br>
<br>
<a href="https://github.com/NanmiCoder/MediaCrawler/issues/180">整数智能《高级爬虫工程师》招聘</a>

成为赞助者，展示你的产品在这里，联系作者：relakkes@gmail.com

## 功能列表
| 平台  | Cookie 登录 | 二维码登录 | 指定创作者主页 | 关键词搜索 | 指定视频/帖子 ID 爬取 | 登录状态缓存 | 数据保存 | IP 代理池 | 滑块验证码 |
|:---:|:---------:|:-----:|:-------:|:-----:|:-------------:|:------:|:----:|:------:|:-----:|
| 小红书 |     ✅     |   ✅   |    ✅    |   ✅   |       ✅       |   ✅    |  ✅   |   ✅    |   ✕   |
| 抖音  |     ✅     |   ✅   |    ✕     |   ✅   |       ✅       |   ✅    |  ✅   |   ✅    |   ✅   |
| 快手  |     ✅     |   ✅   |    ✕    |   ✅   |       ✅       |   ✅    |  ✅   |   ✅    |    ✕   |
| B 站 |     ✅     |   ✅   |    ✕    |   ✅   |       ✅       |   ✅    |  ✅   |   ✅    |   ✕   |
| 微博  |     ✅      |   ✅    |    ✕    |   ✅    |       ✅        |    ✅    |   ✅   |    ✅    |   ✕   |


## 使用方法

### 创建并激活 python 虚拟环境
   ```shell   
   # 进入项目根目录
   cd MediaCrawler
   
   # 创建虚拟环境
   python -m venv venv
   
   # macos & linux 激活虚拟环境
   source venv/bin/activate

   # windows 激活虚拟环境
   venv\Scripts\activate

   ```

### 安装依赖库

   ```shell
   pip3 install -r requirements.txt
   ```

### 安装 playwright浏览器驱动

   ```shell
   playwright install
   ```

### 运行爬虫程序

   ```shell
   # 默认没有开启评论爬取模式，有需要请到配置文件中指定
   # 从配置文件中读取关键词搜索相关的帖子并爬去帖子信息与评论
   python main.py --platform xhs --lt qrcode --type search
   
   # 从配置文件中读取指定的帖子ID列表获取指定帖子的信息与评论信息
   python main.py --platform xhs --lt qrcode --type detail
  
   # 打开对应APP扫二维码登录
     
   # 其他平台爬虫使用示例, 执行下面的命令查看
   python main.py --help    
   ```


### 数据保存
- 支持保存到关系型数据库（Mysql、PgSQL等）
- 支持保存到csv中（data/目录下）
- 支持保存到json中（data/目录下）


### 数据保存至MySQL
- 检查是否安装MySQL，如果没则下载安装
   ```shell
   ps aux | grep mysqld
   ```
- 用root连接MySQL
   ```shell
   mysql -u root -p
   ```
- 创建media_crawler账户
   ```shell
   CREATE USER 'media_crawler'@'localhost' IDENTIFIED BY '123456';
   ```
- 创建media_crawler数据库，并授权给media_crawler账户
   ```shell
   CREATE DATABASE media_crawler;
   GRANT ALL PRIVILEGES ON media_crawler.* TO 'media_crawler'@'localhost';
   FLUSH PRIVILEGES;
   exit;
   ```
- 在media_crawler数据库中建表

   ```shell
   mysql -u media_crawler -p
   USE media_crawler;

   # 创建小红书笔记表
   CREATE TABLE xhs_note (
       id INT AUTO_INCREMENT COMMENT '自增ID',
       user_id VARCHAR(64) COMMENT '用户ID',
       nickname VARCHAR(64) COMMENT '用户昵称',
       avatar VARCHAR(255) COMMENT '用户头像地址',
       ip_location VARCHAR(255) COMMENT '评论时的IP地址',
       add_ts BIGINT COMMENT '记录添加时间戳',
       last_modify_ts BIGINT COMMENT '记录最后修改时间戳',
       note_id VARCHAR(64) COMMENT '笔记ID',
       type VARCHAR(16) COMMENT '笔记类型(normal | video)',
       title VARCHAR(255) COMMENT '笔记标题',
       `desc` TEXT COMMENT '笔记描述',
       video_url TEXT COMMENT '视频地址',
       time BIGINT COMMENT '笔记发布时间戳',
       last_update_time BIGINT COMMENT '笔记最后更新时间戳',
       liked_count VARCHAR(16) COMMENT '笔记点赞数',
       collected_count VARCHAR(16) COMMENT '笔记收藏数',
       comment_count VARCHAR(16) COMMENT '笔记评论数',
       share_count VARCHAR(16) COMMENT '笔记分享数',
       image_list TEXT COMMENT '笔记封面图片列表',
       tag_list TEXT COMMENT '标签列表',
       note_url VARCHAR(255) COMMENT '笔记详情页的URL',
       PRIMARY KEY (id)
   ) COMMENT='小红书笔记';
   
   # 创建小红书笔记评论表
   CREATE TABLE xhs_note_comment (
       id INT AUTO_INCREMENT COMMENT '自增ID',
       user_id VARCHAR(64) COMMENT '用户ID',
       nickname VARCHAR(64) COMMENT '用户昵称',
       avatar VARCHAR(255) COMMENT '用户头像地址',
       ip_location VARCHAR(255) COMMENT '评论时的IP地址',
       add_ts BIGINT COMMENT '记录添加时间戳',
       last_modify_ts BIGINT COMMENT '记录最后修改时间戳',
       comment_id VARCHAR(64) COMMENT '评论ID',
       create_time BIGINT COMMENT '评论时间戳',
       note_id VARCHAR(64) COMMENT '笔记ID',
       content TEXT COMMENT '评论内容',
       sub_comment_count INT COMMENT '子评论数量',
       pictures VARCHAR(512) COMMENT '评论的图片集合',
       PRIMARY KEY (id)
   ) COMMENT='小红书笔记评论';
   
   # 创建小红书博主表
   CREATE TABLE xhs_creator (
       id INT AUTO_INCREMENT COMMENT '自增ID',
       user_id VARCHAR(64) COMMENT '用户ID',
       nickname VARCHAR(64) COMMENT '用户昵称',
       avatar VARCHAR(255) COMMENT '用户头像地址',
       ip_location VARCHAR(255) COMMENT '评论时的IP地址',
       add_ts BIGINT COMMENT '记录添加时间戳',
       last_modify_ts BIGINT COMMENT '记录最后修改时间戳',
       `desc` TEXT COMMENT '用户描述',
       gender VARCHAR(1) COMMENT '性别',
       follows VARCHAR(16) COMMENT '关注数',
       fans VARCHAR(16) COMMENT '粉丝数',
       interaction VARCHAR(16) COMMENT '获赞和收藏数',
       tag_list TEXT COMMENT '标签列表',
       PRIMARY KEY (id)
   ) COMMENT='小红书博主';
   ```

- 修改config/base_config.py
   ```shell
   SAVE_DATA_OPTION = "db"
   ```
- 修改config/db_config.py
   ```shell
   RELATION_DB_PWD = os.getenv("media_crawler", "123456")  # your relation db password
   RELATION_DB_URL = f"mysql://media_crawler:{RELATION_DB_PWD}@localhost:3306/media_crawler"
   ```


## 打赏

如果觉得项目不错的话可以打赏哦。您的支持就是我最大的动力！

打赏时您可以备注名称，我会将您添加至打赏列表中。
<p>
  <img alt="打赏-微信" src="static/images/wechat_pay.jpeg" style="width: 200px;margin-right: 140px;" />
  <img alt="打赏-支付宝" src="static/images/zfb_pay.jpeg" style="width: 200px" />
</p>

## 捐赠信息

PS：如果打赏时请备注捐赠者，如有遗漏请联系我添加（有时候消息多可能会漏掉，十分抱歉）

| 捐赠者         | 捐赠金额  | 捐赠日期       |
|-------------|-------|------------|
| Nate Yang   | 20 元 | 2024-03-19 |
| Tsen Ming   | 100 元 | 2024-03-18 |
| *皓          | 50 元  | 2024-03-18 |
| *刚          | 50 元  | 2024-03-18 |
| *乐          | 20 元  | 2024-03-17 |
| *木          | 20 元  | 2024-03-17 |
| *诚          | 20 元  | 2024-03-17 |
| Strem Gamer | 20 元  | 2024-03-16 |
| *鑫          | 20 元  | 2024-03-14 |
| Yuzu        | 20 元  | 2024-03-07 |
| **宁         | 100 元 | 2024-03-03 |
| **媛         | 20 元  | 2024-03-03 |
| Scarlett    | 20 元  | 2024-02-16 |
| Asun        | 20 元  | 2024-01-30 |
| 何*          | 100 元 | 2024-01-21 |
| allen       | 20 元  | 2024-01-10 |
| llllll      | 20 元  | 2024-01-07 |
| 邝*元         | 20 元  | 2023-12-29 |
| 50chen      | 50 元  | 2023-12-22 |
| xiongot     | 20 元  | 2023-12-17 |
| atom.hu     | 20 元  | 2023-12-16 |
| 一呆          | 20 元  | 2023-12-01 |
| 坠落          | 50 元  | 2023-11-08 |

## 运行报错常见问题Q&A
> 遇到问题先自行搜索解决下，现在AI很火，用ChatGPT大多情况下能解决你的问题  <a href="https://monica.im/invitation?c=4HCSQRYS">免费的ChatGPT推荐</a>

➡️➡️➡️ [常见问题](docs/常见问题.md)


## 项目代码结构
➡️➡️➡️ [项目代码结构说明](docs/项目代码结构.md)

## 手机号登录说明
➡️➡️➡️ [手机号登录说明](docs/手机号登录说明.md)



## star 趋势图
- 如果该项目对你有帮助，star一下 ❤️❤️❤️

[![Star History Chart](https://api.star-history.com/svg?repos=NanmiCoder/MediaCrawler&type=Date)](https://star-history.com/#NanmiCoder/MediaCrawler&Date)




## 参考

- xhs客户端 [ReaJason的xhs仓库](https://github.com/ReaJason/xhs)
- 短信转发 [参考仓库](https://github.com/pppscn/SmsForwarder)
- 内网穿透工具 [ngrok](https://ngrok.com/docs/)

