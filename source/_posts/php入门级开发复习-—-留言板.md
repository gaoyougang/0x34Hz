---
title: php入门级开发复习 — 留言板
date: 2020-05-20 22:38:48
author:
img:
top:
cover:
coverImg:
password:
toc:
mathjax:
summary:
categories: 学习
tags: 
- php
- mysql
reprintPolicy:
---
# php入门级开发复习 — 留言板

## HTML基础(框架结构)

1. html标签：要么成双成对(<p></p>)，要么形单影只。(<img src="" alt="">)

2. 嵌套：成双成对的才可能有第三者或多者，你个单身狗凑什么热闹。

3. 属性：不管你是成双成对，还是形单影只，你总该有自己的名字、年龄、身高、体重、职位吧！

   <标签 属性=‘值’ >，使相同的标签，有不同的效果。你和他人总有共性和差异性。

4. 标签：

   | 布局容器 | 链接 | 图片 | 表格          | 列表   | 标题  | 换行 |
   | -------- | ---- | ---- | ------------- | ------ | ----- | ---- |
   | div      | a    | img  | table，tr，td | ul，ol | h1-h6 | br   |

   | 表单类 | 多功能标签 | 多行文本输入框 | 下拉表单 |
   | ------ | ---------- | -------------- | -------- |
   | form   | input      | textarea       | select   |

5. 基础框架：index.html

   ```html
   <!DOCTYPE html> <!--声明文本类型 -->
   <html lang="en"> 
    <head>          <!--头部信息 -->
       <meta charset="UTF-8">     <!-- 编码方式 -->
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>网页显示标题</title>
   </head>
   <body>            <!-- 网页显示的内容信息 -->
       <div>        <!-- 容器：可用于布局，区域样式规划 -->
           
       </div>
   </body>
   </html>
   ```

## CSS(框架布局美化)

1. 作用：美化网页和提供更好的管理方法

2. 写入方式;

   | 1                           | 2                             | 3（常用）                                  |
   | --------------------------- | ----------------------------- | ------------------------------------------ |
   | 通过style属性（位于标签内） | 通过style标签（一般在head内） | 通过link标签外链(标签在head，内容在文件外) |

3. 样式：选择器{样式}，常用布局样式：

   | 宽高          | 内外间距        | 浮动  | 清除浮动 | 处理溢出 |
   | ------------- | --------------- | ----- | -------- | -------- |
   | width，height | margin，padding | float | clear    | overflow |

   效果样式：

   | 文字颜色 | 背景       | 文字大小  | ….   |
   | -------- | ---------- | --------- | ---- |
   | color    | background | font-size | 等等 |

4. 选择器：众里寻他千百度，蓦然回首，那人却在，灯火阑珊处：就是选择你想要装饰的模块美化。常用选择器如下：style.css

   ```css
   /* id的选择器*/
   #a1{color:blue;}
   /* class的选择器 */
   .a1{color: red;}
   /* 标签选择器 */
   div{font-size: medium;}
   /* 父子选择器，也叫层级选择器 */
   #a1 .ch{color: aliceblue;}
   .a1 .ch{color: aqua;}
   ```

## 构建项目Mysql

创建数据库：

```mysql
create database mesphp charset=utf8;
```

创建项目需要的表：

| 名      | 类型    | 长度 | 允许空值 | 主键        |
| ------- | ------- | ---- | -------- | ----------- |
| id      | int     | 10   | F        | primary key |
| content | varchar | 255  | F        |             |
| user    | varchar | 20   | F        |             |
| intime  | int     | 10   | F        |             |

```mysql
create table msg2(id int(10) primary key auto_increment,
                 content varchar(255) not null,
                 user varchar(20) not null,
                 intime int(10) not null
                );
#查看字段结构
mysql> desc msg2;
+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra          |
+---------+--------------+------+-----+---------+----------------+
| id      | int(10)      | NO   | PRI | NULL    | auto_increment |
| content | varchar(255) | NO   |     | NULL    |                |
| user    | varchar(20)  | NO   |     | NULL    |                |
| intime  | int(10)      | NO   |     | NULL    |                |
+---------+--------------+------+-----+---------+----------------+
4 rows in set (0.00 sec)

#插入数据
insert into msg2 values(1,'hello world!','kali',20200520); 
insert into msg2 (content,user,intime) values('I will hack you.','gt',20200520);

#输出如下：
mysql> select * from msg2;
+----+------------------+------+----------+
| id | content          | user | intime   |
+----+------------------+------+----------+
|  1 | hello world!     | kali | 20200520 |
|  2 | I will hack you. | gt   | 20200520 |
+----+------------------+------+----------+
2 rows in set (0.00 sec)
```

## php基础了解(实现和数据库互动)

1. 第一个php程序：

   ```php
   <?php
       echo 'hello world!';
   ?>
   ```

2. 注释：// 或# 单行注释，/\*多行注释\*/

3. 变量：必须以“\$”开头，简单成为(杯子)

   ```php
   <?php
   	#声明变量
       $a = 'hello Kali';
   	echo $a;	//只会输出a的值
   	var_dump($a); //不仅会输出a的值，还有其数据类型;多值输出，用“,”分开
   ?>
   ```

4. 表达式：任何有值的东西，就叫表达式

5. 数据类型：

   | 整 型   | 浮点型    | 布尔型     | 字符串        | 字符串（识别变量) | 数组                               |
   | ------- | --------- | ---------- | ------------- | ----------------- | ---------------------------------- |
   | \$a=1 ; | \$a2=1.2; | \$a3=true; | \$a4=‘hello’; | \$a5=“hello”;     | \$6=[1,2,3];或者 \$6=array(1,2,3); |

   | 对象                 | 无          |
   | -------------------- | ----------- |
   | \$a7 = new stdclass; | \$a8 = null |

6. 运算符号：

   | 算数运算符              | 赋值运算  | 比较运算    | 逻辑运算           | 其他       |
   | ----------------------- | --------- | ----------- | ------------------ | ---------- |
   | +  -  \*  /  %  ++  \-- | =  .=  += | > < ==  === | && and    \|\|  or | .   =>  -> |

7. 数组：有序的映射，简称：带编号杯子的集合

   ```php
   <?php
       $arr1 = [1,2,3];  //索引数组
   	$arr2 = arry(1,2,3);
   	$arr3 = [1,[1,2],3];  //二维数组
   	var_dump($arr1,$arr2,$arr3);  //输出的是键值对（下标：值）
   
   	$arr4 = [		//关联数组
           'a' => 1,
           'b' => 3,
           'c' => 5,
       ];
   
   	#增加
   	$arr4['d'] = 7;
   	#删除
   	unset($arr4['c']);
   	#修改
       $arr4['b'] = 2;
   	#查询
   	echo $arr4['b'];
   
   	var_dump($arr4);
   ?>
   ```

8. 流程控制

   | 条件        | 循环                               |
   | ----------- | ---------------------------------- |
   | if 和switch | for   foreach  while   (do  while) |

   ```php
   <?php
       //条件if
       $a = 2;
   	if($a == 1){
           echo "是1";
       }elseif($a == 2){
           echo "是2";
       }else{
           echo "不认识";
       }
       echo "--------------------"
   //switch
   	switch($a){
           case 1:
               echo "是1";
               break;			#break跳出循环，continue跳过本次循环
           case 2:
               echo "是2";
               break;
           default:
               echo "不认识";
       }	
   ?>
   ```

   ```php
   <?php
       //循环for
   	for($i=0;$i<10;$i++){
           echo $i;
       }
   //第二种方式
   	$b = 1;
   	for(::){
           if($b > 10){
               break;
           }
           echo $b;
           $b++;
       }
   //foreach使用对象数组
   	$C = [
           'a' => 1,
           'b' => 2;
           'c' => 3;
       ];
   	foreach($c as $key => $value){
           echo $value;
       }
   ?>
   ```

9. 函数：实现代码复用，减少重复造轮子

   ```php
   <?php
       #函数自定义
       function name(){
       	echo 'Kali'; 
   	}
   	#函数调用
   	name();
   ?>
   ```

10. 局部变量和全局变量

    ```php
    <?php
        $a = 'math'; #全局变量a,只能用于全家，不能用于函数内部
    	class($a); #通过传参转化
    	function class($m){
            var_dump($m);	#局部变量只能用于函数，不能用于函数外
        }
    ?>
    ```

11. 类与对象：类是是图纸，对象是参照图纸构建的事物; 实现大规模合作开发

    ```php
    <?php
        #声明类
        class man{
        	public $name = 'kali'; #定义公有类属性
        	public $age = 18;     
        	public function walk(){  #定义公有类方法
                echo "Walking....";
            }
        	public function say($content){
                $this -> walk();	#伪变量，实现同类中方法的调用
                echo "He says $content";
            }
    	}
    	#声明对象,根据类实例化对象
    	$kali = new man();
    	#调用对象的属性
    	echo $kali -> name;
    	#调用对象的方法
    	$kali -> say("good");
    ?>
    ```

12. 魔术方法：

    ```php
    <?php
    	class man{
        	public $name = 'kali';
        #魔术方法，在声明对象的时候就会执行，不需要调用，以两个下划线开头
        	public function __hello($n){ 
                $this -> name = $n;
            }
    		public function say(){
                echo $this -> name;
            }
    	}
    	$gt = new man('gt');
    ?>
    ```

13. 包含文件：

    ```php
    <?php
    	include('path/file.php');	#加载同一文件多次会报错
    	include_once('path/file.php');#不管加载同一文件多次，也只会加载一次，不会报错
    	require('path/file.php');
    	require_once('path/file.php'); #include和require几乎一样，只是处理失败的方式不同
    ?>
    ```

14. **数据发送与接收**：方法(get和post)，其中预定义变量(\$\_POST和\$\_GET)是数组类型

## 项目源码

1. message.php：留言板主页码

   ```php
   <?php
   //链接数据库
   $host = '127.0.0.1';
   $dbuser = 'root';
   $pwd = '123456';
   $dbname = 'mesphp';
   $db = new mysqli($host,$dbuser,$pwd,$dbname);
   //判断链接是否成功
   if($db->connect_errno <> 0){
       die('链接数据库失败！');
   }
   $db->query("SET NAMES UTF8");
   $sql = "select * from msg2 order by id desc";
   $mysqli_result = $db->query($sql);
   if($mysqli_result == false){
       die('SQL错误');
   }
   //从数据库读取数据并保留到数组rows中
   $rows = [];
   while($row = $mysqli_result->fetch_array()){
       $rows[] = $row;
   }
   ?>
   
   <!DOCTYPE html>
   <html lang="zh"> 
    <head>          
       <meta charset="UTF-8">     
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>留言板</title>
       <link rel="stylesheet" href="style.css">
   </head>
   <body class="body"> 
       <div class="content">
           <!-- 发表留言 -->
           <div class="voice"> 
               <form action="save.php" method="post">
                   <textarea class="message" name="message" id="" cols="50" rows="5"></textarea>
                   <br>
                   <input class="user" name="user" type="text">
                   <input class="btn" type="submit" value="发表留言">
               </form>       
           </div>
           <?php
           foreach($rows as $row){
           ?>
           <!-- 查看留言 -->
           <div class="msgs">
               <div class="info">
                   <span class="user"><?php echo $row['user'];?></span>
                   <span class="time"><?php echo date("Y-m-d H:i:s",$row['intime']);?></span>
               </div>
               <div class="message">
               <?php echo $row['content'];?>
               </div>
           </div>
           <?php
           }
           ?>
       </div>    
   </body>
   </html>
   ```

   

2. style.css：留言板CSS样式（没有话太多时间美化）

   ```css
   /* 背景设置 */
   .body{width: 99%;height: 100%;background: url(./12.jpg) no-repeat fixed;background-size: cover;}
   /* 发表留言区域 */
   .content{width: 800px;margin: 0px auto;}
   .voice{overflow: hidden;}
   .voice .message{width: 798px;margin: 0;padding: 0;}
   .voice .user{float: left;}
   .voice .btn{float: right;}
   
   /* 查看留言区域 */
   .msgs{margin: 20px 0px;padding: 5px;}
   .msgs .info{overflow: hidden;}
   .msgs .user{float: left;color:gold;}
   .msgs .time{float: right;color: #999;}
   .msgs .message{width: 100%;}
   ```

3. input.php：检查关键字输入类

   ```php
   <?php
       class input{
           function post($message){
               if($message == ''){  #禁止为空
                   return false;
               }
               #禁止使用关键字判断
               $key = ['','',''];
               foreach($key as $values){
                   if($message == $values){
                       return false;
                   }
               }
               return true;
           }
       }
   ?>
   ```

4. connect.php：用于链接数据库

   ```php
   <?php
       //链接数据库
       $host = '127.0.0.1';
       $dbuser = 'root';
       $pwd = '123456';
       $dbname = 'mesphp';
       $db = new mysqli($host,$dbuser,$pwd,$dbname);
       //判断链接是否成功
       if($db->connect_errno <> 0){
           die('链接数据库失败！');
       }
       //设置链接数据库编码
       $db->query("SET NAMES UTF8");
   ?>
   ```

5. save.php：返回错误错误信息，插入数据的作用

   ```php
   <?php
       include_once('./input.php');
       include_once('./connect.php');
   
       $message = $_POST['message'];
       $user = $_POST['user'];
   
       $input = new input();
   
       $is = $input->post($message);
       if($is == false){
           die('留言内容的数据不正确');
       }
       $is = $input->post($user);
       if($is == false){
           die('留言人的数据不正确');
       }
   
       $time = time();
       $sql = "insert into msg2 (content,user,intime) values ('{$message}','{$user}','{$time}')";
       $db = $db->query($sql);
       header("location:message.php");
   ?>
   ```

## 项目效果截图
![留言板](https://gitee.com/gaoyougang/myblog/raw/master/2020/05/20/php%E5%85%A5%E9%97%A8%E7%BA%A7%E5%BC%80%E5%8F%91%E5%A4%8D%E4%B9%A0-%E2%80%94-%E7%95%99%E8%A8%80%E6%9D%BF/msg.png)
