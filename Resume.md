 <center>
     <h1>蔡箐菁</h1>
     <!-- <img src="assets/photo.jpg" width="120px"> -->
     <div>
         <span>
             <img src="assets/phone-solid.svg" width="18px">
             18222558817
         </span>
         ·
         <span>
             <img src="assets/envelope-solid.svg" width="18px">
             caiqj1203@gmail.com
         </span>
         ·
         <span>
             <img src="assets/github-brands.svg" width="18px">
             <a href="https://github.com/LydiaCai1203">LydiaCai1203</a>
         </span>
         ·
         <span>
             <img src="assets/rss-solid.svg" width="18px">
             <a href="https://lydiacai1203.github.io/blog.html">MyBlog</a>
         </span>
     </div>
 </center>
 ## <img src="assets/info-circle-solid.svg" width="30px"> 个人信息 

 - 女，1995.12.03
 - 求职意向：Python工程师、数据分析师
 - 工作经验：1 年
 - 期望薪资：13k

## <img src="assets/graduation-cap-solid.svg" width="30px"> 教育经历

- 学士，南开大学滨海学院，数字媒体技术专业，2015.9~2019.6
- 学分绩：85.085，专业前 5%
- 通过了 CET6英语等级考试，具有良好的英文文档阅读能力
- 通过了 全国计算机等级考试 二级(C++)
- 获得 第八届蓝桥杯全国软件和信息技术专业人才大赛天津赛区 C/C++程序设计大学B组 二等奖


## <img src="assets/briefcase-solid.svg" width="30px"> 工作经历

- **上海财联社金融科技有限公司（财联社作为金融垂直领域排名第一的APP，为用户提供快速精准的咨询服务），技术部项目一组，Python工程师，2018.8~至今**

## <img src="assets/project-diagram-solid.svg" width="30px"> 项目经历

- **智能信息抓取后端系统**
  - 项目简介：
    - 作为财联社的核心系统，对2万+的数据源头进行爬取, 其中web源达到1.5万+，微博源在2k+，微信源在3k+。每个数据源的更新频率控制在3s左右，每天爬取到10万+的有效网页。在爬虫系统的支撑下，财联社保持金融媒体资讯最快速的APP，行业排名第一。
  - 使用了以下相关的技术：

    - *后端web框架：Django、Flask*

    - *数据库：MySQL、Redis、MongoDB*

    - *任务分发工具以及进程管理工具：Celery、Supervisor、Crontab*
    
    - *部署：Docker、k8s*
  - 主要功能：
    - 爬取用户期望监控的网站列表页的内容 以及 特定微信公众号发布的内容 以及 微博用户发布的内容，将数据整理成统一的格式存进数据库中。提供相应接口，供用户对数据进行基础的增删改查操作，得到快速准确的数据。
  - 我负责的部分：
    - 来源部分接口的设计与开发
    - web爬虫的设计与开发
      该通用爬虫系统满足多种形式的的内容爬取，采用分布式任务队列Celery，每小时的爬取次数达50万+次，保证1.5万+的数据源，每个数据源在一分钟内至少能被抓取1-3次。
    - 微信爬虫的设计与开发
      微信爬虫系统可以同时容纳5个微信爬虫同时进行数据爬取，即能同时监控5000个公众号，实时地获取公众号推送。
    - 正文爬虫的设计与开发以及优化
      - 第一版本借助的是第三方开源库newspaper进行的正文获取，爬取率在百分之五十左右。
      - 第二版本通过结合XPath与JS动态渲染引擎，完成多种形式内容的抓取。支持预定义和自定义的正文标签，有效实现精准正文提取与广告过滤的功能，与开源库newspaper相比准确率提升30%，达到85%。
    - 独立爬虫的开发
      - 独立完成深交所网站、银保监等反爬措施做的较好的网站的爬虫开发。
    - 后续小组人员变动主动接管智能信息抓取后端系统，承担主要开发责任，以及负责项目维护和优化，掌握整个智能信息爬取后端系   统的运作流程，以及技术开发难点。
  
- **智能文章拼接系统**
- 使用了以下相关的技术：
  
  - *后端web框架：Django*
    - *自动化测试工具：Selenium*
    - *数据库：MySQL、MongoDB*
    - *进程管理工具：Supervisor、Crontab*
    
  - 主要功能：
    - 将用户填入的信息、以及携带特定信息请求其它接口获得指定数据整合成一篇文章供用户阅览。
  
  - 我负责的部分：
    - 与产品确认需求。
    - 与前端开发人员以及其他的后端开发人员确认接口的数据格式，以便进行准确、快速项目开发。
    - 负责基础增删改查接口的开发以及文章页面结构的开发和数据处理拼接。
    - 负责接口文档的开发。
    - 负责项目的运维工作。
  
- **原创文章转载监控系统**

    - 使用了以下相关的技术：

      - *后端web框架：Django*

      - *数据库：MySQL*

      - *进程管理工具：Supervisor、Crontab*
    
  - 主要功能：
    - 监控财联社原创文章被转载且被百度收录的情况。
  
  - 我负责的部分：
    - 与产品确认需求。
    - 文章监控显示页面的开发。
    - 原创文章爬虫 以及 监控爬虫的开发。
    - 原创文章转载监控后台系统的开发。
    - 负责项目的运维工作。


## <img src="assets/tools-solid.svg" width="30px"> 技能清单

- ★★★ Python
- ★★☆ C++、Java
- ★★★ MySQL
- ★★☆ Redis
- ★★☆ MongoDB
- ★★☆ Celery、Crontab、Supervisor
- ★☆☆ RabbitMQ、Kafka
- ★☆☆ Docker
- ★☆☆ Git
