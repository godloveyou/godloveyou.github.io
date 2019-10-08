---
title: 微信小程序获取openid
categories: 微信小程序
tags: 微信小程序
---


```
    // 登录
    wx.login({
      success: res => {
        // 发送 res.code 到后台换取 openId, sessionKey, unionId
        console.log("res: "+JSON.stringify(res))
        var code = res.code;
        wx.request({
          url: 'https://nilaile.easy.echosite.cn/yw-api/api/common/wechat/getOpenId',
          method:'post',
          data:{
            'code': code,
            'clientFlag':'client_company'
          },
          success:function(result){
            console.log("请求结果: " + JSON.stringify(result.data))
          }
        })

      }
    })
```
