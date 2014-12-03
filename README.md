spider-utils-for-php:

##原则：

简单、易用、灵活、任性任性任性就是任性！


##特色：

- php 界内最简单易用的 http-utils，自动识别支持 curl、socket、file_get_contents 三种方式。
- http 请求支持 gzip，加速请求，节约请求成本。
- 跟踪 301、302 跳转（可设置最大跳转数量）。
- 支持统一转码为 utf-8，不再需要关心页面是否是 gbk、big5、utf8 等编码。
- 字符串支持通配符、正则表达式、DOM表达式三种方式匹配。
- url支持匹配后自动相对路径转绝对路径。
- ToBe Continue.

##什么？转换相对路径到绝对路径
```php
	// $result = http://baidu.com/bac/index.html
	$result = spider::abs_url('http://baidu.com/abc/', '../bac/index.html');
```

##什么？字符串截取？
```php
	// $result = 23abcde
	$result = spider::cut_str('123abcdef', '1', 'f');
```

##什么？通配符匹配？
```php
	// $result = abc
	$result = spider::mask_match('123abc123', '123(*)123');
	// $result = abc
	$result = spider::mask_match('abc123', '(*)123');
	// $result = 123
	$result = spider::mask_match('123abcabc', '(*)abc');
	// $result = 123abc
	$result = spider::mask_match('123abcdef', '(*)abc', true);
```


##什么？正则匹配？
```php
	// $result = abc
	$result = spider::mask_match('123abc123', '123(*)123');
	// $result = abc
	$result = spider::mask_match('abc123', '(*)123');
	// $result = 123
	$result = spider::mask_match('123abcabc', '(*)abc');
	// return full match mode $result = 123abc
	$result = spider::mask_match('123abcdef', '(*)abc', true);
```


##What？发送http GET请求？ 
```php
    // 自动转码 utf-8, 
    $result =  spider::fetch_url('http://www.baidu.com/');
```

##What？发送http POST请求？

```php
	$post = "wd=".urlencode("你的网址"); 
    // 数组也一样
	// $post = array("wd" => urlencode("你的网址"));
    $result = spider::fetch_url('http://www.baidu.com/s?',$post);
```


##What？POST File？

```php
    $post = array("wd" => "http://", "file" => "@c:/1.txt");
    $result = spider::fetch_url('http://www.baidu.com/s?',$post);
```

##What？要带 UserAgent 和 Cookie? 

```php
	// 一切 headers 都可以带
	$headers = array(
		'Cookie' => 'uid=1; my_name_is=mzphp',
		'UserAgent' => 'userAgentForIphone',
		'Referer' => 'http://baidu.com/',
	);
    $result = spider::fetch_url('http://www.baidu.com/s?',$post, $headers);
```


##What？这些操作如何漂亮的“在一起”？


```php
	// 首先你需要一个女朋友
	$key = "魔爪小说阅读器";
	$url = 'http://www.sogou.com/web?query='.urlencode($key).'&ie=utf8';
	$html = spider::fetch_url($url, '', array('Referer'=>'http://www.sogou.com/'));
	// 对你的女朋友进行分析
	$keywordlist = spider::match($html, array('list'=>array(
		'cut' => '相关搜索</caption>(*)</tr></table>',
		'pattern' => '#id="sogou_\d+_\d+">(?<key>[^>]*?)</a>#is',
	)));
	//
	$newarr = array();
	foreach($keywordlist['list'] as $key=>$val){
		$newarr[$val['key']] = array('key'=>$val['key']);
	}
```

##更多？

好吧，你可以参考一下 mzphp2 项目中的 start_example 里的index_control，on_spider 方法：

[http://git.oschina.net/mz/mzphp2/blob/master/start_example/control/index_control.class.php](http://git.oschina.net/mz/mzphp2/blob/master/start_example/control/index_control.class.php)

##注：

mime_content_type 方法需要 php 开启 finfo