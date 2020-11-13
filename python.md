# python学习笔记
## python自动化

  ### python自动化遇到的问题

  1.window下安装第三方包遇到的问题，显示 **'pip 不是内部或外部命令，也不是可运行的程序或批处理文件'**。

  **`pip install openpyxl`**

  **解决方法：**找到pip3在 **C:\Users\z1760\AppData\Local\Programs\Python\Python38-32\Scripts** 中的路径，然后配置环境变量
    

 ## python爬虫

### python爬虫学习路线

1. 掌握python的一些基础爬虫模块

2. 深入掌握一款合适的表达式

3. 深入掌握抓包分析技术

4. 精通一款爬虫框架

5. 掌握常见的反爬虫策略与反爬虫处理策略

   常见的反爬策略主要有：

   - IP限制
   - UA限制
   - Cookie限制
   - 资源随机化存储
   - 动态加载技术

    常见的反爬处理手段主要有：

   - IP代理池技术
   - 用户代理池技术
   - Cookie保存与处理
   - 自动触发技术
   - 抓包分析技术与自动触发技术

6. 掌握PhantomJS、Selenium等工具的使用

7. 掌握分布式爬虫技术与数据去重技术

### python爬虫的架构组成

- URL管理器：管理待爬取的url集合和已爬取的url集合，传送待爬取的url给网页下载器
- 网页下载器：爬取url对应的网页，存储成字符串，传送给网页解析器
- 网页解析器：解析出有价值的数据，存储下来，同时补充url到URL管理器

### python爬虫工作原理

Python爬虫通过URL管理器，判断是否有待爬URL，如果有待爬URL，通过调度器进行传递给下载器，下载URL内容，并通过调度器传送给解析器，解析URL内容，并将价值数据和新URL列表通过调度器传递给应用程序，并输出价值信息的过程。

### python爬虫常用框架

- Scrapy

  Scrapy是一个为了爬取网站数据，提取结构性数据而编写的应用框架。 可以应用在包括数据挖掘，信息处理或存储历史数据等一系列的程序中。。用这个框架可以轻松爬下来如亚马逊商品信息之类的数据。

  [项目地址](https://scrapy.org/)

- PySpider

  pyspider 是一个用python实现的功能强大的网络爬虫系统，能在浏览器界面上进行脚本的编写，功能的调度和爬取结果的实时查看，后端使用常用的数据库进行爬取结果的存储，还能定时设置任务与任务优先级等。

  [项目地址]([https://github.com/binux/pysp...](https://github.com/binux/pyspider))

- Crawley

  Crawley可以高速爬取对应网站的内容，支持关系和非关系数据库，数据可以导出为JSON、XML等。

  [项目地址]([http://project.crawley-cloud....](http://project.crawley-cloud.com/))

- Portia

  Portia是一个开源可视化爬虫工具，可让您在不需要任何编程知识的情况下爬取网站！简单地注释您感兴趣的页面，Portia将创建一个蜘蛛来从类似的页面提取数据。

  [项目地址]([https://github.com/scrapinghu...](https://github.com/scrapinghub/portia))

- Newspaper

  Newspaper可以用来提取新闻、文章和内容分析。使用多线程，支持10多种语言等。

  [项目地址]([https://github.com/codelucas/...](https://github.com/codelucas/newspaper))

- Beautiful Soup

  Beautiful Soup 是一个可以从HTML或XML文件中提取数据的Python库.它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式.Beautiful Soup会帮你节省数小时甚至数天的工作时间。

  [项目地址]([https://www.crummy.com/softwa...](https://www.crummy.com/software/BeautifulSoup/bs4/doc/))

- Grab

  Grab是一个用于构建Web刮板的Python框架。借助Grab，您可以构建各种复杂的网页抓取工具，从简单的5行脚本到处理数百万个网页的复杂异步网站抓取工具。Grab提供一个API用于执行网络请求和处理接收到的内容，例如与HTML文档的DOM树进行交互。

  [项目地址]([http://docs.grablib.org/en/la...](http://docs.grablib.org/en/latest/#grab-spider-user-manual.))

- Cola

  Cola是一个分布式的爬虫框架，对于用户来说，只需编写几个特定的函数，而无需关注分布式运行的细节。任务会自动分配到多台机器上，整个过程对用户是透明的。

  [项目地址]([https://github.com/chineking/...](https://github.com/chineking/cola))

#### Scrapy

主要包含以下组件：

- 引擎（Scrapy）
- 调度器（Scheduler）
- 下载器（Downloader）
- 爬虫（Spiders）
- 项目管道（Pipeline）
- 下载器中间件（Downloader Middlewares）
- 爬虫中间价（Spider Middlewares）
- 调度中间件（Scheduler Middewares）

Scrapy运行流程大概如下：

1. 引擎从调度器中取出一个链接（URL）用于接下来的抓取
2. 引擎把URL封装成一个请求（Request）传给下载器
3. 下载器把资源下载下来，并封装成应答包（Response）
4. 爬虫解析Response
5. 解析出实体（Item），则交给实体管道进行进一步的处理
6. 解析出的是链接（URL），则把URL交给调度器等待抓取