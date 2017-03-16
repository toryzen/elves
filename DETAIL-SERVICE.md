# Elves Service #

##ShedulerService SD服务##
###sendSync 发起同步任务###
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
	
###sendAsync 发起异步任务###
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

##HeartBeatService HB服务##
###getStatByIp 根据IP获取Agent在线状态###
	接收
	
	{
		"mqt":"call",
		"mqs":"heartbeat",
		"mqf":"getstatbyip",
		"agentip":""

	}

	回复

    {
		"mqt":"back",
		"mqs":"heartbeat",
		"mqf":"getstatbyip",
		"status":""

	}

###getInfoByIp 根据IP获取Agent在线状态###
	接收
	
	{
		"mqt":"call",
		"mqs":"heartbeat",
		"mqf":"getstatbyip",
		"agentip":""

	}

	回复

    {
		"mqt":"back",
		"mqs":"heartbeat",
		"mqf":"getstatbyip",
		"agent_starttime":"",
		"hw_config":""
		"cronlist":["",""]

	}


###getStatByIpList 批量根据IP获取Agent在线状态###
	接收
	
	{
		"mqt":"call",
		"mqs":"heartbeat",
		"mqf":"getstatbyiplist",
		"iplist":["","",""]

	}

	回复

    {
		"mqt":"back",
		"mqs":"heartbeat",
		"mqf":"getstatbyiplist",
		"status":["","",""]
	}

###getOnlineStat 获取当前在线统计数据###
	接收
	
	{
		"mqt":"call",
		"mqs":"heartbeat",
		"mqf":"getonlinestat",

	}

	回复

    {
		"mqt":"back",
		"mqs":"heartbeat",
		"mqf":"getonlinestat",
		"stat":{
			"totalonline":"",
			"linuxonline":"",
			"winonline":""
			...
		}
	}

##SupervisorService SV服务##
###checkIpAuthByKey 检查此Key是否有此IP权限### 
	接收
	
	{
		"mqt":"call",
		"mqs":"supervisor",
		"mqf":"checkipauthbykey",
		"agentip":"",
		"token":"",
		"key":""		

	}

	回复

    {
		"mqt":"back",
		"mqs":"heartbeat",
		"mqf":"getstatbysn",
		"status":""
	}
 

	 
###getAppCallbackInfo 获取APP回调信息###
	接收
	
	{
		"mqt":"call",
		"mqs":"supervisor",
		"mqf":"checkipauthbykey",
		"appname":"",	

	}

	回复

    {
		"mqt":"back",
		"mqs":"heartbeat",
		"mqf":"getstatbysn",
		"appip":"",
		"appport":""
	}
###getAuthByKey 根据Key获取权限###


##CmdbProxySerice CMDB服务##
###getCmdbInfoByIp 根据IP获取Cmdb数据###
	接收
	
	{
		"mqt":"call",
		"mqs":"cmdbproxy",
		"mqf":"checkipauthbykey",
		"agentip":"",	

	}

	回复

    {
		"mqt":"back",
		"mqs":"heartbeat",
		"mqf":"getstatbysn",
		"sn":"",
		"iplist":"",
		"idc":"",
		"main_cat":"",
		"sub_cat":"",
		"app_info":"",
		"manager":""
	}
	
###getCmdbInfoByIpList 批量根据IP获取Cmdb数据###
	接收
	
	{
		"mqt":"call",
		"mqs":"cmdbproxy",
		"mqf":"checkipauthbykey",
		"agentiplist":["","",""],	

	}

	回复

    {
		["mqt":"back","mqs":"heartbeat","mqf":"getstatbysn","sn":"","iplist":"","idc":"","main_cat":"","sub_cat":"","app_info":"","manager":""],
		[...],
		...
	}



##CronService Cron服务##
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
	
##PublicService 公共服务##
###appProcessorResult 处理APPProcessor信息 ###
	接收
	
	{
		"mqt":"call",
		"mqs":"tasklog",
		"mqf":"appProcessorLog",
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
		"mqs":"tasklog",
		"mqf":"agentAsyncLog",
		"info":{
			"agent_callback_time":,			#Agent回调时间
			"worker_flag":,					#Agent执行状态
			"worker_result":,				#Agent执行结果
			"worker_costtime":				#Agent执行耗时
		}
	}
	
	
	
###getTaskByIds 根据ID获取任务###
	接收
	
    {
		"mqt":"call",
		"mqs":"tasklog",
		"mqf":"agentAsyncLog",
		"id":["",""]
	}
	
	发送	
    {
		"mqt":"back",
		"mqs":"tasklog",
		"mqf":"agentAsyncLog",
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
