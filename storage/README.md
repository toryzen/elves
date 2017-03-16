# storage #
## 1.简介 #
查询与日志组件，监听所有组件间数据交互，并提供数据各项查询数据

## 2.组件服务 ##

###getTaskLogByIds 根据ID获取队列任务###
	接收
	
    {
		"mqt":"call",
		"mqs":"storage",
		"mqf":"gettasklogbyids",
		"id":["",""]
	}
	
	发送	
    {
		"mqt":"back",
		"mqs":"storage",
		"mqf":"gettasklogbyids",
		"info":{[
			"instruct": "" ,
            "param": "" ,
            "starttime": "" ,
            "agent_sync_flag": "" ,
            "agent_sync_error": "" ,
            "agent_sync_costtime": "" ,
            "agent_callback_time": "" ,
            "worker_message": "" ,
            "module_sync_flag": "" ,
            "module_sync_error": "" ,
            "module_sync_costtime": "" ,
            "module_callback_time": "" ,
            "processor_message": "" ,
            "processor_costtime": "" ,
            "endtime": "",
            "result_flag": ""
            ],[...]
        }
	}