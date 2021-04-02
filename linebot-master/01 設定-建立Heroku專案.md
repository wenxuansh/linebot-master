# 01 設定-建立Heroku專案



### (1) 在Heroku平台上建立一個帳戶

#### https://devcenter.heroku.com/
![GitHub Logo](/imgs/1-1.jpg)


### (2) 安裝node.js

#### https://nodejs.org/en/
![GitHub Logo](/imgs/1-2.jpg)


### (3) 進入命令提示字元畫面

#### 附屬應用程式 -> 命令提示字元
![GitHub Logo](/imgs/1-3.jpg)


### (4) 在[D槽]建立一個名稱為app的Express框架
```
d:
cd\
express app -ejs
cd app
npm install
```


### (5) 登入heroku
```
heroku login
(依照說明依序輸入帳號及密碼)
```


### (6) 檢查目前的使用者帳號
```
heroku whoami
```


### (7) **如果要登出heroku
```
heroku logout
```


### (8) 如果已登出, 請先重新登入


### (9) **建立一個heroku應用程式(也可以在heroku developers平台中建立)
```
heroku create [heroku應用程式名稱(如:test12345)]
```


### (10) 顯示自己在heroku帳號中所有的應用程式
```
heroku apps
```


### (11) 設定目前操作的heroku應用程式
```
heroku git:remote -a [heroku應用程式名稱]
```


### (12) 顯示目前操作的heroku應用程式(應該會與步驟11設定的名稱相同)
```
heroku apps:info
```


### (13) 列出遠端git儲存庫資訊
```
git remote -v
```


### (14) 將本地端資料上傳至遠端heroku應用程式的git儲存庫
```
git add .
git commit -am "myApp"
git push heroku master
```


### (15) 開啟目前操作的heroku應用程式
```
heroku open
```


#### 應該看到以下畫面
![GitHub Logo](/imgs/1-4.jpg)


### (16) 查看目前操作heroku的控制台畫面
```
heroku logs --tail
```


### (17) 將遠端git內容下載至另一個資料夾中(假設下載至app2, 需先登入heroku)
```
cd\
md app2
cd app2
heroku git:clone -a [自己原有heroku的應用程式名稱]
```
