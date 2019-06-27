nginx命令：

start nginx

nginx -s reload

nginx -s stop



查看端口是否被占用语句

netstat -aon|findstr "8005"

查看进程

netstat -aon|findstr "8005"

杀死程序

netstat -aon|findstr "8005"





vue niginx部署

ip  10.202.181.101   

用户名 appdeploy

密码 sf123456

 nginx.conf所在位置： cd /app/nginx/conf

查看文件 vim nginx.conf  (tab键可联想)

```
upstream scs-oms2-inmsg {
        server scs-oms2-inmsg.intsit.sfdc.com.cn:1080; //后台地址
    }

server {
    listen       8080;
    server_name  localhost;

    access_log  logs/access.log  main;   
```

```
    location / {
        root  /nfsc/SCS_OMS2_CORE_MSG/bg; //前端地址路径
    }
```

```
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }
}
```

部署包所在位置： cd /nfsc/SCS_OMS2_CORE_MSG 

1、上传部署包

命令： wget http://10.118.66.26/dist.zip   将zip上传到hfs上，就有http地址了

2、删除部署包

命令：rm -rf bg

3、解压部署包到bg目录下

命令：unzip dist.zip -d bg

4、重启应用

到目录下： cd /app/nginx

启动：./startNginx.sh

查看进程：ps -ef | grep  nginx



修改目录文件夹名字：mv dist bg

提取目录到上一层：mv dist/ ../
