# heartbeat #
## 1.简介 ##
心跳检测组件，用于检测Agent的在线状态与向其他组件提供Agent在线状态。

## 2.组件服务 ##

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

   
## 3.Thrift服务 ##
	service HeartbeatService{
	    //获取Agent状态信息
	    string getAgentInfo(1:hbstruct info)
	}