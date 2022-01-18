# Linxu Crontab 定时任务写入 概述
* `/etc/crontab` 该文件负责安排由系统管理员制定的维护系统以及其他任务的crontab。
* 研究目的是为了使用`crontab`写入反弹shell
* 命令介绍：
```
root@23a891613b7e:/etc/cron.d# crontab -h
crontab: invalid option -- 'h'
crontab: usage error: unrecognized option
usage:	crontab [-u user] file
	crontab [ -u user ] [ -i ] { -e | -l | -r }
		(default operation is replace, per 1003.2)
	-e	(edit user's crontab)
	-l	(list user's crontab)
	-r	(delete user's crontab)
	-i	(prompt before deleting user's crontab)
root@23a891613b7e:/etc/cron.d# 
```
* 举例：
```
每天的六点输出信息
0 6 * * * echo "Good morning." >> /tmp/test.txt
每晚的21:30重启smb
30 21 * * * /etc/init.d/smb restart
```