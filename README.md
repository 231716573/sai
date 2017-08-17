# composer、laravel 安装教程

### composer 安装过程

首先，先安装xampp或者其他的
#### 安装页面：
```javascript
https://getcomposer.org/download/
```
点击：`Composer-Setup.exe`

#### 如出现：`Failed to decode zlib stream`
原因：外网墙的原因<br>
解决：
* 1 确保xampp里面的php目录里面存在php.exe，右键php.exe->常规->位置，复制php.exe的路径
* 2 点击我的电脑->右键点击属性->系统属性->高级->环境变量->系统变量->Path，双击Path在后面加上php.exe的路径。
* 3 WIN键 + R键，输入cmd，输入php -v，确保能使用php命令
* 4 打开命令行并依次执行下列命令安装最新版本的 Composer：
  ```javascript
  * php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"   下载安装脚本 － composer-setup.php － 到当前目录。
  ```
  ```javascript
  * php composer-setup.php    执行安装过程。
  ```
  ```javascript
  * php -r "unlink('composer-setup.php');"    删除安装脚本。
  ```
8. 全局安装
* 1 找到并进入 PHP 的安装目录（和你在命令行中执行的 php 指令应该是同一套 PHP）。
* 2 将 composer.phar 复制到 PHP 的安装目录下面，也就是和 php.exe 在同一级目录。
* 3 在 PHP 安装目录下新建一个 composer.bat 文件，并将下列代码保存到此文件中。
9. 最后重新打开一个命令行窗口试一试执行 composer --version 看看是否正确输出版本号
10. 提示：不要忘了经常执行 composer selfupdate 以保持 Composer 一直是最新版本哦！
11. 在命令里输入以下代码切换到中国源：
	```javascript
	composer config -g repo.packagist composer https://packagist.phpcomposer.com 
	```
12. 进入一个目录/文件夹，安装laravel(composer create-project --prefer-dist laravel/laravel 项目名)：
	```javascript
	composer create-project --prefer-dist laravel/laravel blog
	```
	
### 常见问题
#### XAMPP 出现 Access forbidden! 错误
权限<Directory>权限配置的问题
解决：
```javascript
DocumentRoot  "F:\laravel"
\<Directory />
    Options +Indexes +FollowSymLinks +ExecCGI
    AllowOverride All
    Order allow,deny
    Allow from all
    Require all granted
\</Directory>
```