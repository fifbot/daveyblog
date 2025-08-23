# 利用CloudFlare+Gmail實現自有域名臨時郵箱服務，無需伺服器


## 前言

之前本博客有介紹過幾種搭建自有域名的郵局功能《[分享自定義郵箱當臨時郵箱的幾種方法](https://mailberry.com.cn/2023/02/temp-mail/)》，免費提供服務的QQ郵箱的域名郵箱，阿里雲的企業郵箱，但現在都不免費了，也介紹過forsaken-mail自搭建的方式，但要求有伺服器和服務商要開放25端口，門檻還是有點高，今天介紹一種人人都能玩的方法。

## 原理

主要利用的是CloudFlare域名提供的電子郵件路由功能，配合Gmail實現收發，從面達到無限賬號的域名郵局服務，可以當成臨時郵箱使用。

要求

1，CloudFlare有一域名

2，gmail郵箱

## 過程

主要分兩個過程，一個接收，一個發送。

### 接收

第一步，在CloudFlare上進入對應的域名

![image-20230829101405612](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829101405612.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

點擊我的之前註冊的link免費域名，進入，如何把域名添加到CloudFlare，可能參考之前的文章《[保姆級教程：免費域名link註冊過程](https://mailberry.com.cn/2023/07/free-domain-by-dynadot/)》

![image-20230829101538754](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829101538754.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

點擊電子郵件——電子郵件路由——開始使用

![image-20230829101651320](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829101651320.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

添加自定義地址和目標位置—創建並繼續，要多少自己創建多少

第二步，驗證郵箱

登錄你的谷歌郵箱，會有一封驗證郵件，證明你目標郵箱是你自己的

![image-20230829102525303](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829102525303.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

點擊Verify email address，完成驗證

第三步，添加DNS記錄

回到CloudFlare後臺

![image-20230829102645511](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829102645511.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

可以看到已經準備好了，我們只要點擊“添加記錄並啓用”，就能自動完成DNS記錄的操作了

![image-20230829103302310](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829103302310.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

可以看到我的一個域名郵箱已經完成了，**你需要多少賬號，就在自定義地址添加便可**，會統一轉發到目標 地址，即是可以N對1。

第四步，測試

![image-20230829103830628](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829103830628.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

可以成功接收到郵件。

**至此：域名郵箱的接收功能已完成**

### 發送

如果你只是接收郵件，這一步可以不需要理會了，如果你也有發送郵件的需求，那接着下接的步驟。

利用的是Gmail SMTP Server，提示免費的發送電子郵件服務，不限制域名，每天可使用500封發送Email服務

第一步，添加其他電子郵件地址

![image-20230829104953251](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829104953251.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

設定——賬號和導入——添加其他電子郵件地址

![image-20230829105110353](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829105110353.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

名稱：就是你發郵件給別人顯示的名稱

電子郵件地址：就是你希望對方接收到你郵件時顯示的地址，我使用上面的sosel@corlalcloud.link

第二步，設定STMP

![image-20230829112527170](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829112527170.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

SMTP伺服器：smtp.gmail.com

用戶名：你谷歌郵箱地址

密碼：你谷歌密碼（套用專用密碼）

TLS：是

我這裏不成功，以下錯誤提示

> **身份驗證失敗。請檢查您的用戶名/密碼。**
> **伺服器返回錯誤: "535-5.7.8 Username and Password not accepted. Learn more at 535 5.7.8 https://support.google.com/mail/?p=BadCredentials em9-20020a17090b014900b002612150d958sm9709788pjb.16 - gsmtp , code: 535"**

解決辦法

添加兩步認證，使用專用密碼登錄

![image-20230829112745686](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829112745686.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

生成專用密碼

![image-20230829114502964](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829114502964.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

設定——賬號和導入——更改賬號設定——“更改密碼”爲專用密碼

![image-20230829114539113](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829114539113.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

這裏把郵箱的密碼改成了專用密碼

第三步，驗證郵箱地址

![image-20230829145232060](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829145232060.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

驗證郵件，這時候由於我們接收那一步已經設定了郵件轉了到gmail.所以直接在gmail收件箱就能檢視到sosel@coralcloud.link的驗證郵件或者驗證碼。

![image-20230829145542837](https://oss.mailberry.com.cn/picgo/2023/03/image-20230829145542837.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

第四步，測試

測試域名郵箱地址發送

![1693294356323](https://oss.mailberry.com.cn/picgo/2023/03/1693294356323.png?x-oss-process=image/watermark,text_TWFpbEJlcnJ5LmNvbS5jbg,type_ZmFuZ3poZW5naGVpdGk,size_18,shadow_50,t_70,g_se,x_10,y_10,color_ffffff)

發送時候選擇域名郵箱作爲發件人，接收端顯示也是域名郵箱的地址

**至此：發件人也用上了域名郵箱地址了**

## 總結

這個方法，一來不需要部署伺服器，二來比臨時郵箱更多了永久能接收郵件；綜合來看，這種方式實現域名郵箱，比部署forsaken-mail更優方案，只要一個域名就能擁有無限個郵箱，非常適合自己需要多郵箱地址的套用場景。比如無限註冊ChatGPT。。
