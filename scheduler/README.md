# scheduler #

## 简介 ##
任务调度组件,发送任务指令，回收任务结果并做后续处理。发起任务的指令来源于OpenApi组件与Cron组件，在任务执行后，Sheduler需要根据任务类型不同选择是否通知App-Processor。若需要通知，则需要向Supervisor请求App-processor信息，以便快速调用。

## 2.组件服务 ##

### schedlerService ###

####sendSync 发起同步任务####
接收
	
	{
		"mqt":"call",
		"mqs":"sheduler",
		"mqf":"sendsync",
		"agentip":"",
		"ins":{
			"mode":"",
			"app":"",
			"func":"",
			"param":"",
			"timeout":"",
			"proxy":""
		}
	}

	回复

    {
		"mqt":"back",
		"mqs":"sheduler",
		"mqf":"sendsync",
		"info":{
			"agent_sync_flag":"",			#任务发送状态
			"agent_sync_error":"",			#任务错误信息
			"agent_sync_costtime:"",		#任务执行时间
			"worker_flag":,					#Agent执行状态
			"worker_result":,				#Agent执行结果
			"worker_costtime":				#Agent执行耗时
		}
	}
	
####sendAsync 发起异步任务####
接收
	
	{
		"mqt":"call",
		"mqs":"sheduler",
		"mqf":"sendsync",
		"agentip":"",
		"ins":{
			"mode":"",
			"app":"",
			"func":"",
			"param":"",
			"timeout":"",
			"proxy":""
		}
	}
	
	回复
	
    {
		"mqt":"back",
		"mqs":"sheduler",
		"mqf":"sendsync",
		"info":{
			"agent_sync_flag":"",			#Agent任务发送状态(true/false)
			"agent_sync_error":"",			#Agent任务错误信息
			"agent_sync_costtime:"",		#Agent任务执行耗时
		}
	}

 
#### 3.Thrift服务 ####

##### 结构体 #####
	//命令构体
	struct Instruct{
	    1 : string id,	//指令ID
	    2 : string ip,	//AgentIP
	    3 : string type, //类型(queue/cron.rt)
	    4 : string mode, //模式(sap,sanp,aap,aanp,ssp,ssnp)
	    5 : string app,	 //Elves App
	    6 : string func, //Elves App 方法
	    7 : string param, //参数JSON
	    8 : i32 timeout,  //超时时间
	    9 : string proxy  //代理器
	}
	
	//命令结果结构体
	struct Reinstruct{
	    1 : Instruct ins,
    	3 : i32 flag,
    	4 : i32 costtime,
    	5 : string result
	}

##### 服务 #####

	service SchedulerService{
	    //异步返回结果处理器
	    string dataTransport(1:Reinstruct reins)
	    //指令中转器[同步]
	    Reinstruct instructionTransit(1:Instruct ins)
	}



#### 4.设计思想 ####
1. Master注册到Zookeeper上，进行健康监测
1. 多个scheduler依靠Zookeeeper通过选举方式产生Leader，Leader负责计划任务的调用执行与队列任务的调用执行，所有的scheduler均会进行Agent的数据处理
1. 指令中转器可由用户通过WEB界面或其他系统调用，并且只可以采用同步模式，异步模式将采用队列方式提交
1. scheduler提供队列功能，并且队列之间可以设置依赖关系，在构造完队列后提交至scheduler，由scheduler触发完成后续操作，队列功能全部采用异步模式进行
1. scheduler提供计划任务功能，并且提供对外接口，可以设置某IP上的队列，支持设置由scheduler触发或Agent触发模式
1. scheduler与核心数据库进行数据交互
1. scheduler采用jdk1.7, Spring , Mybatis构建JAVA客户端项目