### AWK 整理总结
1. 如何取某一行数据中的第N列数据
	* 从前往后数的话，每一行按分割符分割，`$1` 代表第一列，然后依次为，`$2` `$3 ...`
	* 从后往前数的话，每一行按分割符分割，`$NF`代表最后一列，然后向前依次为，`$(NF-1)` `$(NF-2) ...`
	* 下面举一个示例，获取 `/etc/passwd` 文件中的第 2 列，倒数第 1、倒数第3列
	```bash
	[root@ali-c-bpc-backup02 ~]# cat /etc/passwd
	root:x:0:0:root:/root:/bin/bash
	bin:x:1:1:bin:/bin:/sbin/nologin
	daemon:x:2:2:daemon:/sbin:/sbin/nologin
	[root@ali-c-bpc-backup02 ~]# awk -F ':' '{print $2,$(NF),$(NF-3)}' /etc/passwd
	x /bin/bash 0
	x /sbin/nologin 1
	x /sbin/nologin 2
	```
