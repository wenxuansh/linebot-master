# 02 設定-在Heroku專案增加DB



### (1) 開始前, 應該在Heroku平台有一個帳戶, 而且已經登入

#### 確定已在登入狀態
![GitHub Logo](/imgs/2-0.jpg)



### (2) 除了可以用前面的[01 建立Heroku應用程式]外, 也可以在平台上建立heroku應用程式

#### 在已登入的頁面中, 選擇自己的Dashboard
![GitHub Logo](/imgs/2-1.jpg)



### (3) 查看自己的應用程式, 如要新增, 選擇New項下的[Create new app]

#### 查看應用程式, 新增應用程式.
![GitHub Logo](/imgs/2-2.jpg)



### (4) 建立一個Heroku Postgres資料庫

#### 選擇Data, 在Heroku Postgres選單中按下[Create one].
![GitHub Logo](/imgs/2-3.jpg)



### (5) 安裝Heroku Postgres

#### 按下[Install Heroku Postgres].
![GitHub Logo](/imgs/2-4.jpg)



### (6) 輸入專案名稱, DB將安裝其中

#### 輸入自己的專案名稱.
![GitHub Logo](/imgs/2-5.jpg)



### (7) 確定選擇的項目無誤後, 開始安裝

#### 按下[Provision add on].
![GitHub Logo](/imgs/2-6.jpg)



### (8) 執行後, 檢查專案中的Data內容

#### 選擇Data, 應該有一筆Datastores記錄
![GitHub Logo](/imgs/2-7.jpg)



### (9) 點選某個Datastores記錄(目前只有1個), 查看它的資料庫的設定
#### 選擇某個Datastores
![GitHub Logo](/imgs/2-8-1.jpg)


#### 選擇Settings
![GitHub Logo](/imgs/2-8.jpg)



### (9) 查看資料庫連線認證內容
#### 選擇View Credentials..
![GitHub Logo](/imgs/2-9.jpg)



### (10) 資料庫連線認證內容
#### 其中的主機, 資料庫名稱, 帳號, 密碼, 埠號資訊, 可以用在其他程式與此DB連線.
![GitHub Logo](/imgs/2-10.jpg)
