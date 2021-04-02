# 05-4-2 機器人-連Postgres

### CLI命令:

```
執行前題, 先申請:
(1) line developers帳號
(2) heroku developers帳號
(3) github帳號



1. 在[D槽]建立一個<bot>資料夾.
------------------------------------------------ 
d:
cd\
md bot
cd bot


2. 建立一個本地端的git
   (假設已安裝git, 網址: https://git-scm.com/downloads)
------------------------------------------------ 
git init


3. 產生package.json
   (假設已安裝node.js, 網址: https://nodejs.org/en/download)
------------------------------------------------ 
npm init


4. 登入heroku
   (假設已安裝heroku, 命令: npm install heroku -g)
------------------------------------------------ 
heroku login


5. 登入github
------------------------------------------------ 
https://github.com/


6. **建立一個heroku應用程式(本步驟也可以在heroku平台上建立)
   如果已在heroku平台上建立, 可跳過本步驟
------------------------------------------------ 
heroku create [heroku應用程式名稱]
              +-----------------+ (請自訂名稱)
              
              
7. 設定目前操作的heroku應用程式
------------------------------------------------ 
heroku git:remote -a [heroku應用程式名稱]
                     +-----------------+ (自己的heroku應用程式名稱)             


8. 修改package.json(程式如下)
------------------------------------------------ 


9. 安裝模組
------------------------------------------------ 
npm install


10. 增加index.js(程式如下)
------------------------------------------------ 


11. 上傳本地端git至heroku應用程式的git空間
------------------------------------------------ 
git add .
git commit -am "myApp"
git push heroku master


12. 開啟heroku應用程式
    (在瀏覽器中顯示, heroku應用程式網址如 https://****.herokuapp.com/)
------------------------------------------------
heroku open


13. **設定line developers的頻道內容
    (1) 如已設定, 可跳過本步驟
    (2) 先登入line developers
    (3) 先在line developers中建立provider, 也在其中建立了頻道
    (4) 在頻道中增加設定以下:
------------------------------------------------ 
Use webhooks -> Enabled
Webhook URL  -> 步驟12顯示的heroku應用程式網址


14. 查看Heroku的控制台畫面
------------------------------------------------ 
heroku logs --tail


15. 在Line中測試
```


### (1) 增加index.js, 修改package.json

```
 <myApp>
   |___ <.git>
   |___ <node_modules>
   |
   |___ index.js
   |___ package.json  
```


### (2) package.json

```json
{
    "name": "myApp",
    "version": "1.0.0",
    "description": "my Linebot application",
    "main": "index.js",
    "scripts": {
        "start": "node ."
    },
    "author": "tomlin",
    "license": "ISC",
    "dependencies": {        
        "express": "^4.16.3",
        "linebot": "^1.4.1",
        "pg": "^7.4.3"
    }
}
```



### (3) index.js

```javascript
//--------------------------------
// 載入必要的模組
//--------------------------------
var linebot = require('linebot');
var express = require('express');
const { Client } = require('pg');


//--------------------------------
// 填入自己在linebot的channel值
//--------------------------------
var bot = linebot({
    channelId: '(填入自己的資料)',
    channelSecret: '(填入自己的資料)',
    channelAccessToken: '(填入自己的資料)'
});


//-----------------------------------------------------
// 自己的資料庫連結位址
//-----------------------------------------------------
var pgConn = 'postgres://(填入自己的資料庫URI)';


//--------------------------------
// 機器人接受訊息的處理
//--------------------------------
bot.on('message', function(event) {    
    event.source.profile().then(
        function (profile) {	
            //取得使用者資料及傳回文字
            var userName = profile.displayName;
            var userId = profile.userId;
            var no = event.message.text;		

            //建立資料庫連線           
            var client = new Client({
                connectionString: pgConn,
                ssl: true,
            })
			
            client.connect();
			
            //查詢資料
            //(資料庫欄位名稱不使用駝峰命名, 否則可能出錯)
            client.query("select * from student where stuno = $1", [no], (err, results) => {    
                console.log(results);
				
                //回覆查詢結果
                if (err || results.rows.length==0){
                    event.reply('找不到資料');
                }else{						
                    var stuname=results.rows[0].stuname;
                    event.reply(stuname);
                }

                //關閉連線
                client.end();
            });  
        }
    );
});


//--------------------------------
// 建立一個網站應用程式app
// 如果連接根目錄, 交給機器人處理
//--------------------------------
const app = express();
const linebotParser = bot.parser();
app.post('/', linebotParser);


//--------------------------------
// 可直接取用檔案的資料夾
//--------------------------------
app.use(express.static('public'));


//--------------------------------
// 監聽3000埠號, 
// 或是監聽Heroku設定的埠號
//--------------------------------
var server = app.listen(process.env.PORT || 3000, function() {
    var port = server.address().port;
    console.log("正在監聽埠號:", port);
});
```
