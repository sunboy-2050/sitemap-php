
What is sitemap-php ?
----------

sitemap-php 是一个轻量级、简单快速生成网站地图的开源项目，由北京米扑科技有限公司([mimvp.com](http://mimvp.com))开发分享。

通过简单的配置定义，一个函数createSitemap()，可自动生成sitemap.xml、sitemap.html等网站地图文件,

自动生成的xml、html文件，支持Google、Bing、Baidu等主流搜索引擎收录。

Fast and lightweight class for generating Google sitemap XML files and index of sitemap files. 

Written on PHP and uses XMLWriter extension (wrapper for libxml xmlWriter API) for creating XML files. 
XMLWriter extension is enabled by default in PHP 5 >= 5.1.2. 

If you having more than 50000 url, it splits items to seperated files. _(In benchmarks, 1.000.000 url was generating in 8 seconds)_


## 示例：

#### sitemap.xml : [http://mimvp.com/sitemap.xml](http://mimvp.com/sitemap.xml)

#### sitemap.html : [http://mimvp.com/sitemap.html](http://mimvp.com/sitemap.html)



How to use
----------

Sitemap 封装了生成sitemap.xml的属性和方法的类，使用非常简单，示例代码：

```php
function testSitemap() {
	$sitemap = new Sitemap("http://mimvp.com");
	
	 $sitemap->addItem('/', '1.0', 'daily', 'Today');
	 $sitemap->addItem('/hr.php', '0.8', 'monthly', time());
	 $sitemap->addItem('/index.php', '1.0', 'daily', 'Jun 25');
	 $sitemap->addItem('/about.php', '0.8', 'monthly', '2017-06-26');
	 
	 $sitemap->addItem('/hr2.php', '1.0', 'daily', time())->addItem('/index2.php', '1.0', 'daily', 'Today')->addItem('/about2.php', '0.8', 'monthly', 'Jun 25');
	 
	 $sitemap->endSitemap();
}
```

1. 初始化类对象

```php
$sitemap->addItem('/index.php', '1.0', 'daily', time());
```

2. 添加Item

```php
$sitemap->addItem('/', '1.0', 'daily', 'Today');
$sitemap->addItem('/hr.php', '0.8', 'monthly', time());
$sitemap->addItem('/index.php', '1.0', 'daily', 'Jun 25');
$sitemap->addItem('/about.php', '0.8', 'monthly', '2017-06-26');
```

或者

```php
$sitemap->addItem('/hr2.php', '1.0', 'daily', time())->addItem('/index2.php', '1.0', 'daily', 'Today')->addItem('/about2.php', '0.8', 'monthly', 'Jun 25');
```

3. 结束文档

```php
$sitemap->endSitemap();
```
	
4. 生成结果 sitemap.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
	<url>
		<loc>http://mimvp.com/</loc>
		<priority>1.0</priority>
		<changefreq>daily</changefreq>
		<lastmod>2017-06-26T00:00:00+08:00</lastmod>
	</url>
	<url>
		<loc>http://mimvp.com/hr.php</loc>
		<priority>0.8</priority>
		<changefreq>monthly</changefreq>
		<lastmod>2017-06-26T20:16:23+08:00</lastmod>
	</url>
	<url>
		<loc>http://mimvp.com/index.php</loc>
		<priority>1.0</priority>
		<changefreq>daily</changefreq>
		<lastmod>2017-06-25T00:00:00+08:00</lastmod>
	</url>
	<url>
		<loc>http://mimvp.com/about.php</loc>
		<priority>0.8</priority>
		<changefreq>monthly</changefreq>
		<lastmod>2017-06-26T00:00:00+08:00</lastmod>
	</url>
</urlset>
```


More Functions
----------

1. 设置根域名

```php
$sitemap = new Sitemap("http://mimvp.com");
```

修改初始化的域名为

```php
$sitemap->setDomain('http://blog.mimvp.com');
```	
	
2. 设置保存路径
sitemap.xml默认保存在当前目录下，也可设置文件夹目录，例如： xmls/sitemap，表示sitemap.xml保存在当前目录下的xmls/目录下，其中xmls目录会自动创建。注：支持多级目录

```php
$sitemap->setXmlFile("xmls/sitemap");
$sitemap->setXmlFile("xmls/mimvp/sitemap");
```
	
3. 设置是否更多头部

```php
$sitemap->setIsChemaMore(true);
```

如果设置为true，则sitemap.xml文件头部会增加一些头部信息：

```xml
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 	
xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" 
```
	
4. 获取当前写入的sitemap文件

```php
$sitemap->getCurrXmlFileFullPath();
```
	

Advanced Functions
----------

1. 指定包含文件，以/开头

```php
$GIncludeArray = array("", "/index.php", "about.php", "hr.php");
```

2. 排除特定文件或目录

```php
$GExcludeArray = array("usercenter/", "sadmin/", "admin/", "sitemap.php");
```

3. 递归扫描指定目录，默认扫描三层（可自己设定）

```php
function scanRootPath($rootPath=".", $dirLevel=1, $MaxDirLevel=3, &$resArray=array())
```

4. 转化 xml + xsl 为 html 

```php
function createXSL2Html($xmlFile, $xslFile, $htmlFile, $isopen_htmlfile=false) 
```


Sitemap Demo
----------

0. 全局变量，G开头

```php
$GCONFIG = array(	"domain"=>"http://mimvp.com",
			"xmlfile"=>"sitemap",
			"htmlfile"=>"sitemap.html",
			"xslfile"=>"sitemap-xml.xsl",
			"isopen_xmlfile"=>true,
			"isopen_htmlfile"=>true,
			"isscanrootpath"=>true,
			"isxsl2html"=>true,
			"isschemamore"=>true);
```

1. 生成sitemap.xml

```php		
createSitemap();
```

生成示例：

![sitemap.xml 示例](https://github.com/mimvp/sitemap-php/blob/master/mimvp-sitemap-xml.png)




2. 生成 sitemap.html

```php
createXSL2Html($xmlFile, $xslFile, $htmlFile, $isopen_htmlfile=false);
```

生成示例：

![sitemap.html 示例](https://github.com/mimvp/sitemap-php/blob/master/mimvp-sitemap-html.png)
	
	
You need to submit sitemap.xml and sitemap.html to Google、 Bing、 Baidu，etc.


**sitemap-php项目，目前支持指定网页、排除网页、扫描根目录等网站地图；<br>
后期完善时，会增加导出数据库、爬取整个网站等功能，<br>
也希望您的加入，继续完善此项目**

	sitemap-php All Rights by mimvp.com


