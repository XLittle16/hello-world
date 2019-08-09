window安装redis

https://www.cnblogs.com/dingguofeng/p/8709476.html

下载地址

https://github.com/microsoftarchive/redis/releases

直接解压使用



1、redis 主从模式，如果主挂了，从需要手工转移，为了自动故障转移，实现了哨兵模式

https://www.liangzl.com/get-article-detail-29695.html





1、sentinel1监听主从心跳不可达，判定位主管下线

2、当主观下线的是mster时，sentinel1发送命令，向其他sentinel发送命令sentinel is-mater-down-by-addr询问对master的判断，达到quorun数量时，sentinel1判定master为客观下线

然后sentinel1再发送sentinel is-master-down-by-addr来提议自己为leader，其他sentels投票，达到半数以上，那么它成为leader



然后，sentinel选举slave2为mater，对新的master执行salveof no one命令让他成为master

然后向其他slaves发送命令，复制新master的，成为新的master的slave

然后对客户端发出通知

然后将原来的maseter更新为slave，当他恢复后，加入到slave




