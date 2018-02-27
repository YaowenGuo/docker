## panda项目介绍
1. doc 文档目录
 - 存放部分需求记录， 历史sql语句

### 前台
1. 代码位置: app/Home(内部人员使用前台)

### 后台
1. 代码位置: app/Admin (编辑课程使用)

### api
1. 代码位置v5版本:
```
api
```

### 项目依赖
1. >=php7.1
2. 安装ffmpeg, 用于上传音频格式转换
3. redis
4. 打分程序
```
php artisan score:server --daemon
```
5. 启动queue, 放置耗时任务，提高性能
```
php artisan queue:work 定时任务中添加
```
6. 更新geo库， 准确定位用户地址
```
php artisan geoip:update
```

7. 创建软连接, 用于文件可被访问
```
php artisan storage:link
```

## 部署步骤
1. sudo mkdir /web (创建部署根路径)
2. sudo chown -R moma:moma .  (修改权限, 线上为ubuntu用户)
3. git clone git@192.168.8.240:Group-Panda/docker_for_panda.git(下载docker环境, 线上需要推送上去)
4. cd docker_for_panda/app (进入代码目录)
5. git clone http://192.168.8.240:8240/Group-Panda/web_panda_site_new.git (下载后台和api代码)
6. git clone http://192.168.8.240:8240/Group-Panda/web_python_api.git  (部署Python项目，[参考](http://192.168.8.240:8240/Group-Panda/web_python_api))
7. 下载ssl证书放入ssl目录(本地不需要ssl支持， 需要重命名nginx的配置文件， 否则启动报错)
8. cd /web/docker_for_panda/app/web_panda_site_new/ (进入项目) 
9. cp .env.example .env (修改配置文件)
10. php artisan key:generate  # 获取密码，自动保存到 .env
10. 同时在该目录下安装依赖: composer install
11. cd /web/
11. sudo docker-compose up # 启动docker容器(如果已有数据， 需要将数据导入到mysql容器中)
12. 打分程序需要根据打分程序的位置进行调整， 开发时用不到的可以先不用部署
    - 文件位置
    ```
    app/Console/Commands/Score.php
    ```
    - 检查打分程序运行的用户, 需要和启动打分服务的用户相同
    - 检查项目日志文件是否对打分程序有写入权限
    - 确保vendor/workerman/ 下面的日志文件让打分程序有写入权限
    - 检查软连接, 需要将项目上传音频的地址， 链接到打分程序的执行路径
    ```
    如果不存在软连接, 执行
    cd Program_files/kaldi_gop/egs/online_demo/online-data/
    ln -s /web/docker-laravel-ubuntu16/app/web_panda_site_new/storage/app/audio/ .
    ```
    - 启动打分程序， 监听:9999
    ```
    启动命令
    cd /web/docker-laravel-ubuntu16/app/web_panda_site_new
    php artisan score:server start   --daemonize
    ```

12. 添加系统定时任务
    ```
    ## laravel cron
    * * * * * sudo docker exec  web_phpffmpeg php web_panda_site_new/artisan schedule:run >> /dev/null 2>&1
    ```
