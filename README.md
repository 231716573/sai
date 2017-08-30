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
#### 全局安装
* 1 找到并进入 PHP 的安装目录（和你在命令行中执行的 php 指令应该是同一套 PHP）。
* 2 将 composer.phar 复制到 PHP 的安装目录下面，也就是和 php.exe 在同一级目录。
* 3 在 PHP 安装目录下新建一个 composer.bat 文件，并将下列代码保存到此文件中，也就是和 php.exe 在同一级目录。
  ```javascript
  @php "%~dp0composer.phar" %*
  ```
  最后重新打开一个命令行窗口试一试执行 composer --version 看看是否正确输出版本号
#### 提示：不要忘了经常执行 composer selfupdate 以保持 Composer 一直是最新版本哦！
#### 在命令里输入以下代码切换到中国源：

	composer config -g repo.packagist composer https://packagist.phpcomposer.com 

#### 进入一个目录/文件夹，安装laravel(composer create-project --prefer-dist laravel/laravel 项目名)：

	composer create-project --prefer-dist laravel/laravel blog

	
### 常见问题
#### 1、XAMPP 出现 Access forbidden! 错误
权限\<Directory>权限配置的问题<br>
解决：
  找到httpd.conf文件(路径如：F:\xampp\apache\conf\httpd.conf)，找到AllowOverride none这行<br>
  修改为：
  ```javascript
  DocumentRoot  "F:\laravel"
  <Directory />
      Options +Indexes +FollowSymLinks +ExecCGI
      AllowOverride All
      Order allow,deny
      Allow from all
      Require all granted
  </Directory>
  ```

#### 2、```javascriptphp artisan migrate``
数据库配置失败的问题<br>
解决：
  在根目录找到.env文件，配置好数据库，例如：
  ```javascript
  DB_CONNECTION=mysql
  DB_HOST=127.0.0.1
  DB_PORT=3306
  DB_DATABASE=test
  DB_USERNAME=root
  DB_PASSWORD=null
  ```
  为了正常显示，在config目录找到database.php文件，设置好mysql的配置内容，如：
  ```javascript
  'mysql' => [
    'driver' => 'mysql',
    'host' => env('DB_HOST', 'localhost'),
    'port' => env('DB_PORT', '3306'),
    'database' => env('DB_DATABASE', 'test'),
    'username' => env('DB_USERNAME', 'root'),
    'password' => env('DB_PASSWORD', ''),
    'charset' => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix' => '',
    'strict' => false,
    'engine' => null,
  ],
  ```


### laravel 小实例

#### 第一步：创建路由
路径在：......\laravel\app\Http
```javascript
Route::get('/', 'WeblcomeController@index');
```


#### 第二步：创建模型
为控制器引入数据<br>
在数据库中创建一个数据库<br>
然后打开 .env 配置完成<br>

然后创建模型：
```javascript
php artisan make:model Test
```

再输PHP artisan migrate  自动创建tests数据表
```javascript
php artisan make:migration create_test_table --create=test
```
往数据库添加表字段：<br>
![images/20150828205525619.jpg](https://github.com/231716573/sai.github.io/blob/master/images/20150828205525619.jpg)

再输入```javascriptphp artisan migrate``` 就会发现数据库中多了两个字段(name 和 password)

打开数据库随便添加一条数据
#### 第三步：创建控制器

由于该控制器是下载Laravel就自带有的，所以不需要创建。若要创建则输入
```javascript
php artisan make:controller xxxxController
```

打开......\laravel\app\Http\Controllers\WelcomeController.php  ,先在
```javascript
<?php 
  namespace App\Http\Controllers;
```
下面输入<br> 
```javascript
  use App\Test;
  use DB;
``` 

来引入模型,再修改index为：
```javascript
public function index(){

  $tests = DB::table('tests')->get();

  return view('welcome', ['tests' => $tests]);
}
```
这样，就可以把tests表的内容，用$tests去获取<br>
视图文件路径：......\laravel\resources\views\welcome.blade.php <br>
如：
```javascript
<div class="panel-body">
    @foreach($tests as $test)
        {{ $test->name }}
        {{ $test->password }}
    @endforeach
</div>
```