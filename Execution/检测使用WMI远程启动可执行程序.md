# T1047-检测WMI远程启动可执行程序

## T1047在ATT&CK中的描述
Windows管理规范（WMI）是Windows管理功能，它为本地和远程访问Windows系统组件提供了统一的环境。它依靠WMI服务进行本地和远程访问，并依靠服务器消息块（SMB）和远程过程调用服务（RPCS）进行远程访问。RPCS通过端口135运行。
攻击者可以使用WMI与本地和远程系统进行交互，并将其用作许多战术功能的手段，例如收集信息以进行发现和远程执行文件（作为横向移动的一部分）。

## WMI简介
WMI是用于在基于Windows的操作系统上管理数据和进行操作的基础结构。相关人员可以编写WMI脚本或应用程序来自动化远程计算机上的管理任务。

## 测试过程复现
wmic /node:远程计算机IP /user:用户名 /password:密码 process all create "进程名"


## 流量检测规则/思路
```yml
title: WMI远程启动可执行程序
status: Establish
description: 该规则主要检测利用wmi远程启动可执行程序。
tags:
    - attack.T1047
references:
    - https://attack.mitre.org/techniques/T1047/
author: blueteamer
date: 2019/12/09
logsource:
    category: rpc
detection:
    selection:
        Endpoint:
            - 'IRemUnknown2'  #wireshark中可能会将该字符串解析成protocol
        Src-port:
            - >= 49152  #源端口号大于49152
        Dst-port:
            - >= 49152  #目的端口号大于49152
    condition: selection
falsepositives:
    - Unknown
level: medium
```

## 参考
[ATT&CK T1047](https://attack.mitre.org/techniques/T1047)

