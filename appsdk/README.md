# Elves SDK #
## 简介 ##
Elves目前提供Csharp与Python版本SDK，支持使用以上两种语言进行Elves-APP的开发，SDK分为Worker/Processor,Worker工作在Agent端执行相应的业务逻辑，Processor独立运行，接收来自Worker的执行结果进行后续的处理工作。

## Elves Python SDK ##
### Worker ###

在开发Worker时，文件名称与类名称需要与APP名称一致，类内部的方法可以进行自定义，但每一个方法只可以有一个参数Param,且为字典形式,其中Flag标识本次执行的业务状态，用于队列任务的依赖使用。

文件示例如下（apptest App）：

	#!/usr/bin/python
	# coding=utf-8  
	# Author: toryzen  
	#  
	# Create: 2016/06/22
	#
	#   app worker 示例 [文件名需要与类名一致]
	
	class apptest():
	    def mod1(self,param):
	        try:
	            result = param["a"]
	        except Exception,e:
	            result = e
	        flag = "false"
	        return flag,result
	
	if __name__ == '__main__':
	    pass

本示例 app:apptest / func:mod1 /param:{"a":"123"} ,执行结束后将返回false,123

### Processor ###

Processor为接收到Worker传入信息的后续处理，用户只需要将逻辑写入service->service.py->run位置即可

文件结构：

	│  app.py
	│  setting.ini
	│
	├─logs
	├─service
	│      service.py
	│
	└─util
	    │  log4py.py
	    │
	    └─thriftutil

处理逻辑如下(service.py):

	#!/usr/bin/python
	# coding=utf-8  
	# Author: toryzen  
	#  
	# Create: 2016/06/22
	#
	# app processor 主逻辑
	import traceback
	
	import sys,time
	sys.path.append('../')
	import threading
	from util.log4py import log4py
	
	log=log4py("service.py")
	
	class processorThread(threading.Thread):
	    def __init__(self, agentip, flag, costtime, message):
	        threading.Thread.__init__(self)
	        self.agentip = agentip
	        self.flag = flag
	        self.costtime = costtime
	        self.message = message
	    
	    def run(self):
	    	log.info("processor start !")
	        print self
	        log.info("processor end !")
	
	
	if __name__ == "__main__":
	    pThread = processorThread("false",10,"1q2w3e4r")
	    pThread.setDaemon(True)
	    pThread.start()

Processor可以接收到的内容：
	
	agentip,执行相应Worker的AgentIP地址
	flag,Worker端进行的判断flag
    costtime,worker端的执行时长
    message,worker端的执行返回结构


配置文件(setting.ini)

	[app]
	appName = apptest
	appIp = 192.168.4.116
	appPort = 20110
	
	[zookeeper]
	zkList = 192.168.4.116:2181
	zkRoot = /Elves
	zkTimeout = 86400

## Elves C# SDK ##
### Worker ###
SDK打开后会有一个工程项目，开发前需要将工程内项目的名称改为APP的名称，且最终打的DLL也需要与APP名称保持一致,命名空间必须叫ElvesApp,且类名必须叫Worker，类内部的方法可以进行自定义，但每一个方法只可以有一个参数Param,且为字典形式,其中Flag标识本次执行的业务状态，用于队列任务的依赖使用。

Worker逻辑示例（apptest->Worker.cs）：

	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	
	namespace ElvesApp
	{
	    public class Worker
	    {
	        public string flag = "true";
	
	        public string mod1(Dictionary<string,string> param)
	        {
	            string result = "";
	            flag = "true";
	            try
	            {
	                result = param["a"];
	            }
	            catch (Exception e) {
	                result = e.ToString();
	            }
	            return result;
	        }
	
	    }
	}

本示例 app:apptest / func:mod1 /param:{"a":"123"} ,执行结束后将返回false,123

### Processor ###

Processor为接收到Worker传入信息的后续处理，用户只需要将逻辑写入processor->service->Service.cs->Exec位置即可

文件结构：

	└─processor
	    ├─bin
	    ├─obj
	    ├─Properties
	    ├─service
	    └─util
	        └─thrift

处理逻辑如下（Service.cs）：

	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	using System.Threading;
	
	namespace ElvesApp
	{
	    public class Processor 
	    {
	        public log4net.ILog loginfo = log4net.LogManager.GetLogger("loginfo");
	        public  void Exec(string agentip, int flag, int costtime,string message) {
	            loginfo.Info("Processor Start!");
	            string result = "";
	            //Thread.Sleep(10000);
	            try
	            {
	                result = message + " | Processor Reviced!(" + DateTime.Now.ToString() + ")";
	                /**
	
	                    在此编写业务代码
	
	                **/
	            }
	            catch (Exception e) {
	                result = e.ToString();
	            }
	            loginfo.Info("Processor Finish!");
	        }
	
	    }
	}

Processor可以接收到的内容：
	
	agentip,执行相应Worker的AgentIP地址
	flag,Worker端进行的判断flag
    costtime,worker端的执行时长
    message,worker端的执行返回结构


配置文件(setting.ini)

	<?xml version="1.0" encoding="utf-8" ?>
	<configuration>
	  <appSettings>
	    
	    <!-- 模块信息-->
	    <add key="appName" value="apptest" />
	    <add key="appIp" value="192.168.8.194" />
	    <add key="appPort" value="10010" />
	
	    <!-- Zookeeper配置-->
	    <add key="zkList" value="192.168.6.116:2181" />
	    <add key="zkRoot" value="/Elves" />
	    <add key="zkTimeout" value="86400" />
	    
	  </appSettings>
	</configuration>

## Elves App上线 ##
1. 将Worker的一级目录打包为ZIP，可以将不同的SDK实现的Worker打入同一个ZIP包，默认Windows会选择C#版本执行，Linux会选择Python版本执行，并将ZIP包放入至指定为未知
2. 修改Processor的配置文件信息，并以Deamon形式将其运行起来