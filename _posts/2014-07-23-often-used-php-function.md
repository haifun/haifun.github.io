---
layout: post
title:  "PHP常用函数推荐"
date:   2014-07-23 10:36:52
categories: php
---

PHP函数的使用频率也是很高的，在一个高效率的网站中，不能没有PHP函数，将多个PHP的函数集合在一起，就会形成PHP的类，实例化类以后，以页面调用一个功能就很方便了，这就是PHP的面向对象了。

在学习PHP的面向过程的时候，使用PHP语句很容易的就实现了某个功能，但如果某个网站出现某个功能很频率，比如分页，判断邮箱地址，获取客户端IP等等，这时候使用面向过程就会感觉很吃力，因为每个地方都要重写一篇，这时候我们就要考虑将这些功能进行封装处理，形成PHP的函数，这样就不用再写那么多冗余的代码了。

下面是一些PHP常用函数推荐，这是一些使用频率比较高的函数，有的来自别人的程序……在使用时直接调用函数即可，是不是很方便呢。

###1.产生随机字符串函数

    function random($length) {
        $hash = ”;
        $chars = ‘ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyz’;
        $max = strlen($chars) – 1;
        mt_srand((double)microtime() * 1000000);
        for($i = 0; $i < $length; $i++) {
            $hash .= $chars[mt_rand(0, $max)];
        }
        return $hash;
    }

###2.截取一定长度的字符串

注：该函数对GB2312使用有效

    function wordscut($string, $length ,$sss=0) {
        if(strlen($string) > $length) {
            if($sss){
                $length=$length – 3;
                $addstr=’ …’;
            }
            for($i = 0; $i < $length; $i++) {
                if(ord($string[$i]) > 127) {
                    $wordscut .= $string[$i].$string[$i + 1];
                    $i++;
                } else {
                    $wordscut .= $string[$i];
                }
            }
            return $wordscut.$addstr;
        }
        return $string;
    }

###3.取得客户端IP地址

    function GetIP(){
        if (getenv(“HTTP_CLIENT_IP”) && strcasecmp(getenv(“HTTP_CLIENT_IP”), “unknown”))
            $ip = getenv(“HTTP_CLIENT_IP”);
        else if (getenv(“HTTP_X_FORWARDED_FOR”) && strcasecmp(getenv(“HTTP_X_FORWARDED_FOR”), “unknown”))
            $ip = getenv(“HTTP_X_FORWARDED_FOR”);
        else if (getenv(“REMOTE_ADDR”) && strcasecmp(getenv(“REMOTE_ADDR”), “unknown”))
            $ip = getenv(“REMOTE_ADDR”);
        else if (isset($_SERVER['REMOTE_ADDR']) && $_SERVER['REMOTE_ADDR'] && strcasecmp($_SERVER['REMOTE_ADDR'], “unknown”))
             $ip = $_SERVER['REMOTE_ADDR'];
        else
            $ip = “unknown”;
        return($ip);
    }

###4.创建相应的文件夹

    function createdir($dir=”){
        if (!is_dir($dir)){
            $temp = explode(‘/’,$dir);
            $cur_dir = ”;
            for($i=0;$i<count($temp);$i++){
                $cur_dir .= $temp[$i].’/';
                if (!is_dir($cur_dir)){
                    @mkdir($cur_dir,0777);
                }
            }
        }
    }

###5.判断邮箱地址

    function checkEmail($inAddress){
        return (ereg(“^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(\.[a-zA-Z0-9_-])+”,$inAddress));
    }

###6.跳转

    function gotourl($message=”,$url=”,$title=”){
        $html =”<html><head>”;
        if(!empty($url))
            $html .=”<meta http-equiv=’refresh’ content=\”3;url=’”.$url.”‘\”>”;
            $html .=”<link href=’../templates/style.css’ type=text/css rel=stylesheet>”;
            $html .=”</head><body><br><br><br><br>”;
            $html .=”<table cellspacing=’0′ cellpadding=’0′ border=’1′ width=’450′ align=’center’>”;
            $html .=”<tr><td bgcolor=’#ffffff’>”;
            $html .=”<table border=’1′ cellspacing=’1′ cellpadding=’4′ width=’100%’>”;
            $html .=”<tr class=’m_title’>”;
            $html .=”<td>”.$title.”</td></tr>”;
            $html .=”<tr class=’line_1′><td align=’center’ height=’60′>”;
            $html .=”<br>”.$message.”<br><br>”;
        if (!empty($url))
            $html .=”系统将在3秒后返回<br>如果您的浏览器不能自动返回,请点击[<a href=".$url." target=_self>这里</a>]进入”;
        else
            $html .=”[<a href='#' onclick='history.go(-1)'>返回</a>]“;
            $html .=”</td></tr></table></td></tr></table>”;
            $html .=”</body></html>”;
        echo $html;
        exit;
    }

###7.分页（两个函数配合使用）

    function getpage($sql,$page_size=20){
        global $page,$totalpage,$sums; //out param
        $page = $_GET["page"];
        //$eachpage = $page_size;
        $pagesql = strstr($sql,” from “);
        $pagesql = “select count(*) as ids “.$pagesql;
        $result = mysql_query($pagesql);
        if($rs = mysql_fetch_array($result)) $sums = $rs[0];
        $totalpage = ceil($sums/$page_size);
        if((!$page)||($page<1)) $page=1;
        $startpos = ($page-1)*$page_size;
        $sql .=” limit $startpos,$page_size “;
        return $sql;
    }
    function showbar($string=”"){
        global $page,$totalpage;
        $out=”共<font color=’red’><b>”.$totalpage.”</b></font>页 “;
        $linkNum =4;
        $start = ($page-round($linkNum/2))>0 ? ($page-round($linkNum/2)) : “1″;
        $end = ($page+round($linkNum/2))<$totalpage ? ($page+round($linkNum/2)) : $totalpage;
        $prestart=$start-1;
        $nextend=$end+1;
        if($page<>1)
        $out .= “<a href=’?page=1&&”.$string.”‘title=第一页>第一页</a> “;
        if($start>1)
        $out.=”<a href=’?page=”.$prestart.”‘ title=>..<<</a> “;
        for($t=$start;$t<=$end;$t++){
            $out .= ($page==$t) ? “<font 
            color=’red’><b>[".$t."]</b></font> ” : “<a 
            href=’?page=$t&&”.$string.”‘>$t</a> “;
        }
        if($end<$totalpage)
            $out.=”<a href=’?page=”.$nextend.”&&”.$string.”‘ title=>>>..</a>”;
        if($page<>$totalpage)
            $out .= ” <a href=’?page=”.$totalpage.”&&”.$string.”‘ title=最后页>最后页</a>”;
        return $out;
    }