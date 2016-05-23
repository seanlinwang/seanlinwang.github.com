---
layout: post
title: 同步git项目到consul key/value 
tags: git consul
---

{{ page.title }}
================

<p class="meta">23 May 2016 - HangZhou</p>

背景
------------------------------------------------
公司consul key/value已经使用了一段时间，本身没有修改记录和备份容灾，后者是git的强项。
所以本文采用把项目的配置提交到指定的repo，用程序自动同步到consul的结构。

备份consul
-------------------------------------------------
先安装[fsconsul](https://github.com/Cimpress-MCP/fsconsul)，编写备份配置文件fsconsul_config.json:

```
{
    "consul" : {
    	"addr": "10.0.40.111:8500", // 别写http: 
        "dc": "dc1" 
    } ,
    "runOnce" : true, // 只需要备份一次
    "mappings" : [{
        "prefix": "/service", // 要备份的consul目录
        "path": "/Users/sean/workspace/study/fsconsul_back" // 本地备份目录
    }]
}
```

执行同步命令:

	fsconsul -configFile fsconsul_config.json

注意fsconsul不会同步空文件夹。

创建repo
-------------------------
把path提交到git上作为同步的repo即可。


同步git
----------------------------------------
找一台机器能pull上述的repo，安装nodejs和npm，再安装[git2consul](https://github.com/Cimpress-MCP/git2consul)。

编写配置:

```
{
    "version":"1.0",
    "local_store": "/data/git2consul", // 本地repo cache，默认是/tmp
    "logger":{
        "name":"git2consul",
        "streams":[
            {
                "level":"trace", // 线上可以用debug
                "type":"rotating-file",
                "path":"/var/log/git2consul/git2consul.log"
            }
        ]
    },
    "repos":[
        {
            "name":"service",
            "url":"git@your_git_lab_host:consul_config/keyvalue.git",
            "include_branch_name" : false,
            "branches":[
                "master"
            ],
            "hooks":[  
                {  // gitlab上配置webhook
                      "type" : "gitlab",
                      "port" : "5252",
                      "url" : "/gitpoke"
                },
                { // 轮询拉取也要配置，防止webhoook不成功
                  "type" : "polling",
                  "interval" : "1" // 最小1分钟
                }
            ]
        }
    ]
}
```

启动git2consul:

	node /usr/bin/git2consul --endpoint 10.0.40.111 --port 8500 --config-file git2consul_config.json

未完成
------------------------
git2consul在和git、consul交互的时候发生问题，会删除本地的repo，重新pull并提交consul。
但开发、测试环境为了方便程序员，往往希望能直接修改consul，只要git上相同文件未被修改，
就不会覆盖consul。 目前只能修改git2consul，参见后续博文。

References
-----------------------------
[fsconsul](https://github.com/Cimpress-MCP/fsconsul)
[git2consul](https://github.com/Cimpress-MCP/git2consul)