##测试环境测试订单
	测试地址：http://portal.longzhu.com/test/suningShop?orderItemCode=xxxxx
	
	测试大礼包和正常充值，需要在测试环境管理后台配置redis ：WalletProducts 

####1.正常充值 
	1.无折扣，纯现金支付   
      充值账号：48915293
	  orderItemCode:00138845689101
	  orderCode:33112199408
	  productCode:11402033883
	  支付组合:（总金额=18.00000 龙币数量=1800.00000）：（现金支付：18.0 元）（状态：成功）
	
      订单记录：
	      4281058	12	114	48915293	苏宁旗舰店充值龙币	苏宁旗舰店充值龙币 （总金额=18.00000 龙币数量=1800.00000）：（现金支付：18.0 元）（状态：成功）；   	message={messageId=800f05db713c4285a27e40c51b1eb799 ,source=null,topic=suning_order_virtual_resend,msg={"orderItemCode":"00138845689101","supplierCode":"70880256"},messageType=FROM,user=70880256,appKey=8013159edc318f72b83b4c367b0ee4d8}	orderItemCode=00138845689101	orderCode=33112199408	orderInfo={"sn_responseContent":{"sn_body":{"getVirtual":{"passNoName":"","rechargeType":"直冲","orderCode":"33112199408","productCode":"11402033883","orderType":"1","gameId":"0UVM","sum":"18.00000","roleName":"","orderItemCode":"00138845689101","supplierCode":"70880256","sectionName":"","roleCode":"","customer":"48915293","passNoCode":"","serverName":"","deviceIp":"101.87.7.23","mobilePhone":"","serverCode":"","orderAccount":"13761114885","sectionCode":"","returnFlag":"","orderLineStatus":"10","itemCode":"0000000000000","cardNum":"1.00000"}}}}
		2019-10-02 16:03:59


	2.无折扣，现金、云钻支付   	
      充值账号：123958506
	  orderItemCode:00138818152201
	  orderCode:35112246377
	  productCode:11315525574
	  支付组合：（总金额=1.00000 龙币数量=100.00000）：（现金支付：0.16 元）（状态：成功）； （云钻支付：0.84 元）（状态：成功）；
	
      订单记录：
			4280910	12	114	123958506	苏宁旗舰店充值龙币	苏宁旗舰店充值龙币 （总金额=1.00000 龙币数量=100.00000）：（现金支付：0.16 元）（状态：成功）； （云钻支付：0.84 元）（状态：成功）；  	message={messageId=d80a21bd87a0442ebdeefe1d9dbd8a66 ,source=null,topic=suning_order_virtual_resend,msg={"orderItemCode":"00138818152201","supplierCode":"70880256"},messageType=FROM,user=70880256,appKey=8013159edc318f72b83b4c367b0ee4d8}	orderItemCode=00138818152201	orderCode=35112246377	orderInfo={"sn_responseContent":{"sn_body":{"getVirtual":{"passNoName":"","rechargeType":"直冲","orderCode":"35112246377","productCode":"11315525574","orderType":"1","gameId":"0UVM","sum":"1.00000","roleName":"","orderItemCode":"00138818152201","supplierCode":"70880256","sectionName":"","roleCode":"","customer":"123958506","passNoCode":"","serverName":"","deviceIp":"49.94.69.3","mobilePhone":"","serverCode":"","orderAccount":"15895852171","sectionCode":"","returnFlag":"","orderLineStatus":"10","itemCode":"","cardNum":"1.00000"}}}}
				2019-10-02 11:24:29     

	3.无折扣，现金、云钻、优惠券支付 
      充值账号：155472300
	  orderItemCode:00144160856001
	  orderCode:37113959391
	  productCode:11402033732

	  支付组合：总金额=158.00000 龙币数量=15800.00000）：（现金支付：156.93 元）（状态：成功）； （云钻支付：1.0 元）（状态：成功）； （优惠券支付：0.07 元）（状态：成功）

	  订单记录：
			4303338	12	114	155472300	苏宁旗舰店充值龙币	苏宁旗舰店充值龙币 （总金额=158.00000 龙币数量=15800.00000）：（现金支付：156.93 元）（状态：成功）； （云钻支付：1.0 元）（状态：成功）； （优惠券支付：0.07 元）（状态：成功）； 	message={messageId=c5f39b9481254b9890bbbd60d945739a ,source=null,topic=suning_order_virtual_resend,msg={"orderItemCode":"00144160856001","supplierCode":"70880256"},messageType=FROM,user=70880256,appKey=8013159edc318f72b83b4c367b0ee4d8}	orderItemCode=00144160856001	orderCode=37113959391	orderInfo={"sn_responseContent":{"sn_body":{"getVirtual":{"passNoName":"","rechargeType":"直冲","orderCode":"37113959391","productCode":"11402033732","orderType":"1","gameId":"0UVM","sum":"158.00000","roleName":"","orderItemCode":"00144160856001","supplierCode":"70880256","sectionName":"","roleCode":"","customer":"155472300","passNoCode":"","serverName":"","deviceIp":"117.136.110.168","mobilePhone":"","serverCode":"","orderAccount":"13507067605","sectionCode":"","returnFlag":"","orderLineStatus":"10","itemCode":"0000000000000","cardNum":"1.00000"}}}}
	2019-10-20 20:47:59
 

	4.有折扣，纯现金支付     
	  orderItemCode:00117117860501
	  orderCode:35108998506
	  productCode:11317641469
	  支付组合：    （总金额=297.00000 龙币数量=29700.00000）：（现金支付：297.0 元）（状态：成功）；增发支付）：3.0 元）（状态：成功）；
	  
	  订单记录：
		纯现金支付：
			4230438	12	114	154648474	苏宁旗舰店充值龙币	苏宁旗舰店充值龙币 （总金额=297.00000 龙币数量=29700.00000）：（现金支付：297.0 元）（状态：成功）；   	message={messageId=838ee6de08fe4a89995e94b2948c2417 ,source=null,topic=suning_order_virtual_resend,msg={"orderItemCode":"00117117860501","supplierCode":"70880256"},messageType=FROM,user=70880256,appKey=8013159edc318f72b83b4c367b0ee4d8}	orderItemCode=00117117860501	orderCode=35108998506	orderInfo={"sn_responseContent":{"sn_body":{"getVirtual":{"passNoName":"","rechargeType":"账号直冲","orderCode":"35108998506","productCode":"11317641469","orderType":"1","gameId":"0UVM","sum":"297.00000","roleName":"","orderItemCode":"00117117860501","supplierCode":"70880256","sectionName":"","roleCode":"","customer":"154648474","passNoCode":"","serverName":"","deviceIp":"122.193.225.12","mobilePhone":"","serverCode":"","orderAccount":"13506295595","sectionCode":"","returnFlag":"","orderLineStatus":"10","itemCode":"","cardNum":"1.00000"}}}}
				2019-08-23 20:37:31
       增发支付：
			4230439	12	114	154648474	苏宁旗舰店充值龙币	苏宁旗舰店充值龙币 （增发）（总金额=3.00000 龙币数量=300.00000）：   （增发支付）：3.0 元）（状态：成功）；	message={messageId=838ee6de08fe4a89995e94b2948c2417 ,source=null,topic=suning_order_virtual_resend,msg={"orderItemCode":"00117117860501","supplierCode":"70880256"},messageType=FROM,user=70880256,appKey=8013159edc318f72b83b4c367b0ee4d8}	orderItemCode=00117117860501	orderCode=35108998506	orderInfo={"sn_responseContent":{"sn_body":{"getVirtual":{"passNoName":"","rechargeType":"账号直冲","orderCode":"35108998506","productCode":"11317641469","orderType":"1","gameId":"0UVM","sum":"297.00000","roleName":"","orderItemCode":"00117117860501","supplierCode":"70880256","sectionName":"","roleCode":"","customer":"154648474","passNoCode":"","serverName":"","deviceIp":"122.193.225.12","mobilePhone":"","serverCode":"","orderAccount":"13506295595","sectionCode":"","returnFlag":"","orderLineStatus":"10","itemCode":"","cardNum":"1.00000"}}}}
	2019-08-23 20:37:31

	5.有折扣，现金、云钻支付
      充值账号：140524386
	  orderItemCode:00135146782801
	  orderCode:34129869678
	  productCode:11317038633
	  支付组合：
			4224727	12	114	140524386	苏宁旗舰店充值龙币	苏宁旗舰店充值龙币 （总金额=678.00000 龙币数量=67800.00000）：（现金支付：673.88 元）（状态：成功）； （云钻支付：4.12 元）（状态：成功）；    （增发支付）：10.0 元）（状态：成功）

	  订单记录： 
		现金、云钻支付：
			4224727	12	114	140524386	苏宁旗舰店充值龙币	苏宁旗舰店充值龙币 （总金额=678.00000 龙币数量=67800.00000）：（现金支付：673.88 元）（状态：成功）； （云钻支付：4.12 元）（状态：成功）；  	message={messageId=ab71b11ab4744d34a998da5771f52f23 ,source=null,topic=suning_order_virtual_resend,msg={"orderItemCode":"00135146782801","supplierCode":"70880256"},messageType=FROM,user=70880256,appKey=8013159edc318f72b83b4c367b0ee4d8}	orderItemCode=00135146782801	orderCode=34129869678	orderInfo={"sn_responseContent":{"sn_body":{"getVirtual":{"passNoName":"","rechargeType":"账号直冲","orderCode":"34129869678","productCode":"11317038633","orderType":"1","gameId":"0UVM","sum":"678.00000","roleName":"","orderItemCode":"00135146782801","supplierCode":"70880256","sectionName":"","roleCode":"","customer":"140524386","passNoCode":"","serverName":"","deviceIp":"223.71.41.98","mobilePhone":"","serverCode":"","orderAccount":"18651661096","sectionCode":"","returnFlag":"","orderLineStatus":"10","itemCode":"","cardNum":"1.00000"}}}}
	2019-08-19 12:06:04     
		
		增发支付： 
			4224728	12	114	140524386	苏宁旗舰店充值龙币	苏宁旗舰店充值龙币 （增发）（总金额=10.00000 龙币数量=1000.00000）：   （增发支付）：10.0 元）（状态：成功）；	message={messageId=ab71b11ab4744d34a998da5771f52f23 ,source=null,topic=suning_order_virtual_resend,msg={"orderItemCode":"00135146782801","supplierCode":"70880256"},messageType=FROM,user=70880256,appKey=8013159edc318f72b83b4c367b0ee4d8}	orderItemCode=00135146782801	orderCode=34129869678	orderInfo={"sn_responseContent":{"sn_body":{"getVirtual":{"passNoName":"","rechargeType":"账号直冲","orderCode":"34129869678","productCode":"11317038633","orderType":"1","gameId":"0UVM","sum":"678.00000","roleName":"","orderItemCode":"00135146782801","supplierCode":"70880256","sectionName":"","roleCode":"","customer":"140524386","passNoCode":"","serverName":"","deviceIp":"223.71.41.98","mobilePhone":"","serverCode":"","orderAccount":"18651661096","sectionCode":"","returnFlag":"","orderLineStatus":"10","itemCode":"","cardNum":"1.00000"}}}}
	2019-08-19 12:06:04 


####2.大礼包
	1.大礼包支付：
      纯现金
	  充值账号：20882022
	  orderItemCode:00437483784101
      orderCode:32431474320
	  productCode:11315525574
	  支付组合：（总金额=1.00000 龙币数量=100.00000）：（现金支付：1.0 元）（状态：成功）；


	  订单记录：4323349	12	114	20882022	苏宁旗舰店充值龙币	苏宁旗舰店充值龙币 （总金额=1.00000 龙币数量=100.00000）：（现金支付：1.0 元）（状态：成功）；   	message={messageId=821f58320f074a9e8af17612c635507b ,source=null,topic=suning_order_virtual_resend,msg={"orderItemCode":"00437483784101","supplierCode":"70880256"},messageType=FROM,user=70880256,appKey=8013159edc318f72b83b4c367b0ee4d8}	orderItemCode=00437483784101	orderCode=32431474320	orderInfo={"sn_responseContent":{"sn_body":{"getVirtual":{"passNoName":"","rechargeType":"直冲","orderCode":"32431474320","productCode":"11315525574","orderType":"1","gameId":"0UVM","sum":"1.00000","roleName":"","orderItemCode":"00437483784101","supplierCode":"70880256","sectionName":"","roleCode":"","customer":"20882022","passNoCode":"","serverName":"","deviceIp":"114.84.212.27","mobilePhone":"","serverCode":"","orderAccount":"18516631780","sectionCode":"","returnFlag":"","orderLineStatus":"10","itemCode":"","cardNum":"1.00000"}}}}
	2019-11-04 14:32:00

	
	2.大礼包支付：配置产品id:
	  纯现金
      充值账号：1324822
	  orderItemCode:00147645143401
	  orderCode:36117022833
	  productCode:11315525574
	  支付组合：（总金额=1.00000 龙币数量=100.00000）：（现金支付：1.0 元）（状态：成功）；

	  订单记录：
		4318821	12	114	1324822	苏宁旗舰店充值龙币	苏宁旗舰店充值龙币 （总金额=1.00000 龙币数量=100.00000）：（现金支付：1.0 元）（状态：成功）；   	message={messageId=d95ee2c383264622ad2a7e565829ac9a ,source=null,topic=suning_order_virtual_resend,msg={"orderItemCode":"00147645143401","supplierCode":"70880256"},messageType=FROM,user=70880256,appKey=8013159edc318f72b83b4c367b0ee4d8}	orderItemCode=00147645143401	orderCode=36117022833	orderInfo={"sn_responseContent":{"sn_body":{"getVirtual":{"passNoName":"","rechargeType":"直冲","orderCode":"36117022833","productCode":"11315525574","orderType":"1","gameId":"0UVM","sum":"1.00000","roleName":"","orderItemCode":"00147645143401","supplierCode":"70880256","sectionName":"","roleCode":"","customer":"1324822","passNoCode":"","serverName":"","deviceIp":"114.84.212.27","mobilePhone":"","serverCode":"","orderAccount":"18616331322","sectionCode":"","returnFlag":"","orderLineStatus":"10","itemCode":"","cardNum":"1.00000"}}}}
	2019-10-31 17:21:59

	
	


		




