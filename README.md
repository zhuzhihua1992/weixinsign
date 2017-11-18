# weixinsign
weixinsin
#获取签名的刚刚

首先要在配置文件里面配置AppId和secret
把上面的包引入然后：
//	    //1、获取AccessToken  
		AccessToken accessToken = new AccessToken();
		accessToken.request();
		String access = accessToken.getToken();
//	    System.err.println(accessToken);
	    //2、获取Ticket  
	    String jsapi_ticket  = TokenProxy.jsApiTicket();

如果你使用的SpringMVC架构可以使用如下代码获取签名：

>@RequestMapping(value = "/sign", produces = "text/html;charset=UTF-8")
	public @ResponseBody String Add(HttpServletRequest request, HttpServletResponse response) { 
		response.setHeader("Access-Control-Allow-Origin","*");
//	    //1、获取AccessToken  
		AccessToken accessToken = new AccessToken();
		accessToken.request();
		String access = accessToken.getToken();
//	    System.err.println(accessToken);
	    //2、获取Ticket  
	    String jsapi_ticket  = TokenProxy.jsApiTicket();
	    //3、时间戳和随机字符串  
	    String noncestr = UUID.randomUUID().toString().replace("-", "").substring(0, 16);//随机字符串  
	    String timestamp = String.valueOf(System.currentTimeMillis() / 1000);//时间戳  
	    System.out.println("accessToken:"+accessToken+"\njsapi_ticket:"+jsapi_ticket+"\n时间戳："+timestamp+"\n随机字符串："+noncestr);  
	    //4、获取url  
	    String url=request.getParameter("url");
	    /*根据JSSDK上面的规则进行计算，这里比较简单，我就手动写啦 
	    String[] ArrTmp = {"jsapi_ticket","timestamp","nonce","url"}; 
	    Arrays.sort(ArrTmp); 
	    StringBuffer sf = new StringBuffer(); 
	    for(int i=0;i<ArrTmp.length;i++){ 
	        sf.append(ArrTmp[i]); 
	    } 
	    */  
	    //5、将参数排序并拼接字符串 
	    //"jsapi_ticket=" + jsapi_ticket + "&noncestr=" + noncestr + "&timestamp=" + timestamp + "&url=" + url;
	    String str = "jsapi_ticket="+jsapi_ticket+"&noncestr="+noncestr+"&timestamp="+timestamp+"&url="+url;   
	    //6、将字符串进行sha1加密  
	    String signature =SHA1(str);  
	    Map<String, String> paramesMap=new HashMap<String, String>();
	    paramesMap.put("timestamp", timestamp);
	    paramesMap.put("nonceStr", noncestr);
	    paramesMap.put("signature", signature);
	    /**
	     * 发送给朋友: "menuItem:share:appMessage"
分享到朋友圈: "menuItem:share:timeline"
分享到QQ: "menuItem:share:qq"
分享到Weibo: "menuItem:share:weiboApp"
	     */
	    System.out.println("参数："+str+"\n签名："+signature);
		return JSONUtil.getJsonStr(0000, "成功", paramesMap);  
	}  
	
