简单、易用、灵活的网络类，spider/network for PHP , too simple .

spider-utils-for-php:

##原则：

简单、易用、灵活、任性任性任性就是任性！


##特色：

- php 界内最简单易用的 http-utils，自动识别支持 curl、socket、file_get_contents 三种方式。
- http 请求支持 gzip，加速请求，节约请求成本。
- 跟踪 301、302 跳转（可设置最大跳转数量）。
- 支持统一转码为 utf-8，不再需要关心页面是否是 gbk、big5、utf8 等编码。
- 字符串支持通配符、正则表达式、DOM表达式（已弃用）三种方式匹配。
- url支持匹配后自动相对路径转绝对路径。
- ToBe Continue.

##什么？转换相对路径到绝对路径
```php
	// return http://baidu.com/bac/index.html
	$result = spider::abs_url('http://baidu.com/abc/', '../bac/index.html');
```

##什么？html2txt?
```php
	// return 123
	$result = spider::html2txt('<p><a href="">1</a>23<p>');
```

##什么？字符串截取？
```php
	// return 23abcde
	$result = spider::cut_str('123abcdef', '1', 'f');
```

##什么？通配符匹配？
```php
	// return abc
	$result = spider::mask_match('123abc123', '123(*)123');
	// return abc
	$result = spider::mask_match('abc123', '(*)123');
	// return 123
	$result = spider::mask_match('123abcabc', '(*)abc');
	// return 123abc
	$result = spider::mask_match('123abcdef', '(*)abc', true);
```


##What？发送http GET请求？ 
```php
    // 所有的 http 请求都会自动识别并统一转码成 utf-8
    $result =  spider::GET('http://www.baidu.com/');
    // process your $result
    echo $result;
    // or get response status code
    echo spider::$last_response_code;
    // or get spider fetch latest url(include 302)
    echo spider::$url;
    // or process your last_header
    print_r(spider::$last_header);
```

##What？发送http POST请求？

```php
    // bind post varialbe
    $post = "wd=".urlencode("你的网址"); 
    // 数组也一样
    // $post = array("wd" => urlencode("你的网址"));
    $result = spider::POST('http://www.baidu.com/s?', $post);
```


##What？POST File？

```php
    // add @ to post file
    $post = array("wd" => "http://", "file" => "@c:/1.txt");
    $result = spider::POST('http://www.baidu.com/s?', $post);
```

##What？要带 UserAgent 和 Cookie? 

```php
	// 一切 headers 都可以传入
	$headers = array(
                // set cookie
		'Cookie' => 'uid=1; my_name_is=mzphp',
                // set UserAgent
		'UserAgent' => 'userAgentForIphone',
                // set Referer
		'Referer' => 'http://baidu.com/',
		// set ip(multi ip support), 设置外网 ip (需要您有多个外网 IP 支持)
		'ip' => '127.0.0.1',
		// set proxy, 设置代理
		'proxy' => array(
			'type' => 'HTTP', //HTTP or SOCKET
			'host' => '127.0.0.1:8888',
                        // BASIC or NTLM
			//'auth' => 'BASIC:user:pass',
		),
	);
    // build post
    $post = array("wd" => "http://", "test" => "123");
    $result = spider::POST('http://www.baidu.com/s?', $post, $headers);
```


##What？这些操作如何漂亮的“在一起”？


```php
	// 首先你需要一个女朋友
	$key = "女朋友";
	$url = 'http://www.sogou.com/web?query='.urlencode($key).'&ie=utf8';
	$html = spider::GET($url, array('Referer'=>'http://www.sogou.com/'));
	// 对你的女朋友进行分析
	$keywordlist = spider::match($html, array('list'=>array(
                // 先是让 spider 去取得(*)对应的结果
		'cut' => '相关搜索</caption>(*)</tr></table>',
                // 然后让 spider 用正则去匹配对应的列表，美其名曰：表白
		'pattern' => array(
                        '#id="sogou_hehehe">(?<key>[^>]*?)</a>#is',
	                // 什么第一次表白没有成功？好那么我们来第二种表白方式
                        '#id="sogou_\d+_\d+">(?<key>[^>]*?)</a>#is',
	        )
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
