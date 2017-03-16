# queue #
## 1.简介 ##
队列任务组件，用于设置并管理Agent上的队列任务

## 2.组件服务 ##

###createQueue 新建队列###
	接收
	
	{
		"mqt":"call",
		"mqs":"queue",
		"mqf":"createqueue",
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
		"mqs":"queue",
		"mqf":"subqueue",
		"result":""
	}
	
###subQueue 提交队列 ###

	接收
	
	{
		"mqt":"call",
		"mqs":"queue",
		"mqf":"subqueue",
		"queue_ids":["","",""]
	}
	
	回复
	
	{
		"mqt":"back",
		"mqs":"queue",
		"mqf":"subqueue",
		"result":""
	}
	

###appProcessorResult 处理APPProcessor信息 ###
	接收
	
	{
		"mqt":"call",
		"mqs":"queue",
		"mqf":"appprocessorlog",
		"info":{
			"processor_sync_flag":,			#processor执行状态
			"processor_sync_error":,		#processor错误信息
			"processor_sync_costtime":,		#processor执行耗时
		}
	}
	
###agentAsyncResult 处理异步Agent信息 ###
	接收
	
    {
		"mqt":"call",
		"mqs":"queue",
		"mqf":"agentasynclog",
		"info":{
			"agent_callback_time":,			#Agent回调时间
			"worker_flag":,					#Agent执行状态
			"worker_result":,				#Agent执行结果
			"worker_costtime":				#Agent执行耗时
		}
	}
	

