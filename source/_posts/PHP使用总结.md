---
title: php使用总结
comment: true
date: 2017-04-26 16:36:21
tags: PHP
categories: 后端学习笔记
---
php使用总结。

<!-- more -->

# 配置文件代码示例

```php
<?php
//对每个文件进行编码
header("content-type:text/html; charset=utf-8");

//数据库服务器连接，@符表示屏蔽错误信息
$link=@mysql_connect("localhost","root","775166") or die ("连接服务器失败失败");

//选择数据库
mysql_select_db("212db",$link) or die ("连接数据库失败");

//数据库编码
mysql_query("set names 'utf8'"); 

//次函数表示转义我们在文本域中使用的换行和空格
function htmtocode($content) {
    $content = str_replace("\n", "<br>", str_replace(" ", "&nbsp;", $content));
    return $content;
}

自定义常量：
/** 数据库的名称 */
define('DB_NAME', 'myblog');

/** MySQL数据库用户名 */
define('DB_USER', 'root');

/** MySQL数据库密码 */
define('DB_PASSWORD', '775166');

/** MySQL主机 */
define('DB_HOST', 'localhost');

/** 创建数据表时默认的文字编码 */
define('DB_CHARSET', 'utf8');

?>
```

# 注册用户逻辑

```php
//加载配置文件
include ("config.php");

//判断是否填写数据，如果有则执行
if ($_POST['submit']){
    $username = $_POST['username'];
    $password = $_POST['password'];
    $email = $_POST['email'];
    $sql="select * from user where username='$_POST[username]'";
    $check=mysql_query($sql);
    $row=mysql_fetch_array($check);

    //继续判断是否有相似用户名，如果有则执行
    if($row){
        echo "<script>alert('脫脙禄搂脙没脰脴赂麓拢隆');window.location='reg.php'</script>";
        return false;
    }

    //加密密码，并插入数据库
    $password = MD5($password);
    $sql="insert into user(id,username,password,email,regtime) values ('','$username','$password','$email',now())";
    $query=mysql_query($sql);
    echo "<script>alert('脳垄虏谩鲁脡鹿娄,脟毛碌脟脗陆拢隆');window.location='login.php'</script>";
}
```

# 登录逻辑

```php
include ("config.php");
if ($_SESSION['username']) {
    header("Location: index.php");
}

//保留登录信息
session_start();

//判断是否填写用户信息
if(isset($_POST['submit'])){
    $username = $_POST['username'];
    $password = MD5($_POST['password']);

    //到数据库校验是否匹配
    $sql="select * from user where username='$username'and password='$password'";
    $query=mysql_query($sql);
    $row=mysql_fetch_array($query);

    //如果匹配则保留登录信息，否则重新填写
    if($row){
            $_SESSION['username']=$username;
            }
    else {
            echo "<script>alert('ÓÃ»§Ãû»òÃÜÂë´íÎó');location.href='login.php';</script>";
        }
}
```


```
$sql="select * from  artical order by id desc limit 6";
$query=mysql_query($sql);

<ul>
<?php while($rs=mysql_fetch_array($query)){?>
<li><a href="artical.php?id=<?php echo $rs['id']?>"><?php echo $rs['title'] ?></a></li>
<?php 
}
?>
</ul>
```

# sql语句

```bash
# 命令行中输入并加分号。

# 选择数据库
use RUNOOB

# 设置使用的字符集
set names utf8;

# 读取数据表的信息
SELECT * FROM Websites
```

## SELECT
```bash
SELECT * FROM Websites;

SELECT name,country FROM Websites;

# 从 "Websites" 表的 "country" 列中选取唯一不同的值，也就是去掉 "country" 列重复值
SELECT DISTINCT country FROM Websites;

# 使用where获取满足情况的数据
SELECT * FROM Websites WHERE country='CN';

# 加入or和and
SELECT * FROM Websites WHERE country='CN' AND alexa > 50;
SELECT * FROM Websites WHERE country='USA' OR country='CN';
SELECT * FROM Websites WHERE alexa > 15 AND (country='CN' OR country='USA');

# 使用order，其中alexa是表中的一个字段
SELECT * FROM Websites ORDER BY alexa;
SELECT * FROM Websites ORDER BY alexa DESC;
SELECT * FROM Websites ORDER BY country,alexa;
```
where子句中的运算符：

- =   等于
- <>  不等于。在 SQL 的一些版本中，该操作符可被写成 !=
- >   大于
- <   小于
- >=  大于等于
- <=  小于等于
- BETWEEN 在某个范围内
- LIKE    搜索某种模式
- IN  指定针对某个列的多个可能值

## INSERT INTO 

```bash
INSERT INTO Websites (name, url, alexa, country) VALUES ('百度','https://www.baidu.com/','4','CN');

# 指定列插入数据
INSERT INTO Websites (name, url, country) VALUES ('stackoverflow', 'http://stackoverflow.com/', 'IND');
```

## UPDATE 

```bash
UPDATE Websites SET alexa='5000', country='USA' WHERE name='菜鸟教程';
```

## DELETE

```bash
DELETE FROM Websites WHERE name='百度' AND country='CN';
```

# PHP中的Mysql

操作MySQL的函数：

- mysqli_connect($connect)
- mysqli_query($connect,"SQL 语句")
- mysqli_fetch_array()
- mysqli_close()

## 连接数据库
```
$dbhost = 'localhost:3306';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('Could not connect: ' . mysqli_error());
}
echo '数据库连接成功！';

//创建数据库
$sql = 'CREATE DATABASE RUNOOB';
$retval = mysqli_query($conn,$sql );
if(! $retval )
{
    die('创建数据库失败: ' . mysqli_error($conn));
}
echo "数据库 RUNOOB 创建成功\n";

//删除数据库
$sql = 'DROP DATABASE RUNOOB';
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
    die('删除数据库失败: ' . mysqli_error($conn));
}
echo "数据库 RUNOOB 删除成功\n";

//选择数据库
mysqli_select_db($conn, 'RUNOOB' );

//创建数据表
$sql = "CREATE TABLE runoob_tbl( ".
        "runoob_id INT NOT NULL AUTO_INCREMENT, ".
        "runoob_title VARCHAR(100) NOT NULL, ".
        "runoob_author VARCHAR(40) NOT NULL, ".
        "submission_date DATE, ".
        "PRIMARY KEY ( runoob_id ))ENGINE=InnoDB DEFAULT CHARSET=utf8; ";
mysqli_select_db( $conn, 'RUNOOB' );
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
    die('数据表创建失败: ' . mysqli_error($conn));
}
echo "数据表创建成功\n";

//删除数据表
$sql = "DROP TABLE runoob_tbl";
mysqli_select_db( $conn, 'RUNOOB' );
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
  die('数据表删除失败: ' . mysqli_error($conn));
}
echo "数据表删除成功\n";

mysqli_close($conn);
```

# mysql命令

## 登录数据库服务器
```bash
# 从命令行登录MySQL数据库服务器 1、登录使用默认3306端口的MySQL
/usr/local/mysql/bin/mysql -u root -p

# 通过TCP连接管理不同端口的多个MySQL（注意：MySQL4.1以上版本才有此项功能）
/usr/local/mysql/bin/mysql -u root -p --protocol=tcp --host=localhost --port=3307

# 通过socket套接字管理不同端口的多个MySQL
/usr/local/mysql/bin/mysql -u root -p --socket=/tmp/mysql3307.sock

# 通过端口和IP管理不同端口的多个MySQL
/usr/local/mysql/bin/mysql -u root -p -P 3306 -h 127.0.0.1
```

## 增删改查

```bash
# 查看已有数据库
SHOW DATABASES;

# 创建名称为rewin的数据库
CREATE DATABASE rewin;

# 删除名称为rewin的数据库
DROP DATABASE rewin;

# 选择rewin数据库
USE rewin;

# 显示当前数据库中存在什么表
SHOW TABLES;

# 创建数据库表zhangyan
CREATE TABLE `zhangyan` ( `id` INT( 5 ) UNSIGNED NOT NULL AUTO_INCREMENT , `username` VARCHAR( 20 ) NOT NULL , `password` CHAR( 32 ) NOT NULL , `time` DATETIME NOT NULL , `number` FLOAT( 10 ) NOT NULL , `content` TEXT NOT NULL , PRIMARY KEY ( `id` ) ) ENGINE = MYISAM ;

# 查看zhangyan表结构
DESCRIBE zhangyan;

# 从表中检索所有记录
SELECT * FROM zhangyan;

# 从zhangyan表中检索特定的行
SELECT * FROM zhangyan WHERE username = abc AND number=1 ORDER BY id DESC;

# 从zhangyan表中检索指定的字段
SELECT username, password FROM zhangyan;

# 从zhangyan表中检索出唯一的不重复记录：
SELECT DISTINCT username FROM zhangyan;

# 插入信息到zhangyan表
INSERT INTO zhangyan (id, username, password, time, number, content) VALUES (, abc, 123456, 2007-08-06 14:32:12, 23.41, hello world);

# 更新zhangyan表中的指定信息
UPDATE zhangyan SET content = hello china WHERE username = abc;

# 删除zhangyan表中的指定信息
DELETE FROM zhangyan WHERE id = 1;

# 清空zhangyan表
DELETE FROM zhangyan;

# 删除zhangyan表
DROP TABLE zhangyan;

# 更改表结构，将zhangyan表username字段的字段类型改为CHAR(25)
ALTER TABLE zhangyan CHANGE username username CHAR(25);

# 将当前目录下的mysql.sql导入数据库
SOURCE ./mysql.sql;
```
## 数据库权限操作SQL语句

```bash
# 创建一个具有root权限，可从任何IP登录的用户sina，密码为zhangyan
GRANT ALL PRIVILEGES ON *.* TO sina@% IDENTIFIED BY zhangyan;

# 创建一个具有"数据操作"、"结构操作"权限，只能从192.168.1.***登录的用户sina，密码为zhangyan
GRANT SELECT , INSERT , UPDATE , DELETE , FILE , CREATE , DROP , INDEX , ALTER , CREATE TEMPORARY TABLES , CREATE VIEW , SHOW VIEW , CREATE ROUTINE, ALTER ROUTINE, EXECUTE ON *.* TO sina@192.168.1.% IDENTIFIED BY zhangyan;

# 创建一个只拥有"数据操作"权限，只能从192.168.1.24登录，只能操作rewin数据库的zhangyan表的用户sina，密码为zhangyan
GRANT SELECT , INSERT , UPDATE , DELETE ON  rewin.zhangyan TO sina@192.168.1.24 IDENTIFIED BY zhangyan;

# 创建一个拥有"数据操作"、"结构操作"权限，可从任何IP登录，只能操作rewin数据库的用户sina，密码为zhangyan 
GRANT SELECT , INSERT , UPDATE , DELETE , CREATE , DROP , INDEX , ALTER , CREATE TEMPORARY TABLES , CREATE VIEW , SHOW VIEW , CREATE ROUTINE, ALTER ROUTINE, EXECUTE ON rewin.* TO sina@% IDENTIFIED BY zhangyan;

# 删除用户
DROP USER sina@%;

# MySQL中将字符串aaa批量替换为bbb的SQL语句
UPDATE 表名 SET 字段名 = REPLACE (字段名, aaa, bbb);

# 修复损坏的表 

# 用root帐号从命令行登录MySQL
mysql -u root -p

# 输入root帐号的密码

# 选定数据库名（本例中的数据库名为student）
use student

# 修复损坏的表（本例中要修复的表为smis_user_student）
repair table smis_user_student;udent
```