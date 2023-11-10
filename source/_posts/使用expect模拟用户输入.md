---
title: 使用expect模拟用户输入
date: 2023-11-09 16:18:11
tags:
categories:
	- "教程"
---

在公司想要批量给多个服务器实现免密登录，但是使用`ssh-copy-id`的时候需要进行输入密码,百度了一下可以使用`expect`来模拟交互操作。

<!-- more -->

## expect的作用

expect是一个免费的编程工具，可以自动化完成交互任务，无需人为的干预。

## 一、安装expect

expect默认是没有安装的，所以需要先进行安装

```bash
# linux下安装命令：
sudo yum install expect

# ubuntu下安装命令：
sudo apt-get install expect
```

## 二、编写exp脚本

1. 创建一个新的文本文件，并命名脚本的名字，expect的脚本后缀是`*.exp`,例如:`script.exp`

2. 添加expect解释器,bash脚本的第一行通常是: `#!/bin/bash`。但因为expect有自己的解释器，所以第一行是：`#!/usr/bin/expect -f`

3. expect常用的命令和关键词：

+ `spawn`：启动一个新的进程，并与之进行交互。
+ `expect`：等待特定的模式出现在输出中，然后执行相应的操作。
+ `send`：向程序发送文本或命令。
+ `interact`：允许用户与程序进行交互，将控制权交还给用户。

4. 样例：
```bash
#!/usr/bin/expect -f

set pwd "123456"
set user "root"
# 可以设置列表
set servers {
	192.168.1.1
	192.168.1.2
	192.168.1.3
}
# for 循环
foreach server $servers {
    spawn ssh-copy-id $user@$server
    expect {
        "Are you sure you want to continue connecting (yes/no)?" {
                send "yes\r"
                exp_continue
        }
        "worker@$server's password:" {
            send "$pwd\r"
            exp_continue
        }
        eof {
            puts "Public key copied to $server"
        }
    }
}
```
5. 执行脚本:
```bash
expect script.exp
```

## 三、将expect脚本嵌入bash脚本

样例：
```bash
#!/bin/bash

expect_tmp() {
	expect -c "
	set pwd "123456"
	set user "root"
	set servers {
		192.168.1.1
		192.168.1.2
		192.168.1.3
	}
	foreach server $servers {
	    spawn ssh-copy-id $user@$server
	    expect {
    	    "Are you sure you want to continue connecting (yes/no)?" {
                	send "yes\r"
                	exp_continue
        	}
        	"worker@$server's password:" {
            	send "$pwd\r"
            	exp_continue
        	}
        	eof {
            	puts "Public key copied to $server"
        	}
 	   }
	}
	"

	echo expect_tmp done!!!
}

expect_tmp

echo Done!!!
exit 0
```
