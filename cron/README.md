# cron #
## 1.简介 ##
定时任务组件，用于设置并管理Agent上的定时任务

## 2.组件服务 ##

###getCronListByIp 根据IP获取计划任务###

接收
	
	{
		"mqt":"call",
		"mqs":"cron",
		"mqf":"setcronbyip",
		"agentip":""	
	}
	
	回复
	{
		"mqt":"call",
		"mqs":"cron",
		"mqf":"setcronbyip",
		"cron_list":[
            "cron_id"   :"A49B1E8FA1B43BC5",
            "agent_ip"  : "127.0.0.1",
            "cron_type" : "self",
            "ins":{
				"mode":"",
				"app":"",
				"func":"",
				"param":"",
				"timeout":"",
				"proxy":""
			}
            "param"     : "",
            "rule"      : "0/10 * * * * ?",
            "create_time": "2016-06-06 17:28:08"
        ],[..],[..]

	}
            
            

###setCronByIp 根据IP设置计划任务###
	接收
	
	{
		"mqt":"call",
		"mqs":"cron",
		"mqf":"setcronbyip",
		"agentip":"",	
		"cron_type":"",
		"rule":"",
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
		"mqt":"call",
		"mqs":"cron",
		"mqf":"setcronbyip",
		"flag":"",	
		"error_info":""

	}
	
###delCronByIpId 根据IP删除计划任务###
	接收
	
	{
		"mqt":"call",
		"mqs":"cron",
		"mqf":"setcronbyip",
		"agentip":"",	
		"id":"",
	}
	
	回复
	{
		"mqt":"call",
		"mqs":"cron",
		"mqf":"setcronbyip",
		"flag":"",	
		"error_info":""

	}
