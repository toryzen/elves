# cmdbproxy #
## 1.简介 ##
心跳检测组件，用于检测Agent的在线状态与向其他组件提供Agent在线状态。

## 2.组件服务 ##

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
