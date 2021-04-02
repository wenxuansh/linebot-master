# 04-1 網頁-連結postgres


### (1) 修改了index.js, 增加db.js

```
 <myApp>
   |___app.js 
   |
   |___<routes>
          |___ index.js     (修改)
          |___ <lib>
                 |___ db.js (增加)
    
```


### (1) db.js

```javascript
const { Client } = require('pg');

module.exports = function(){
    return new Client({
        connectionString: 'postgres://自己在heroku中的database URI',
        ssl: true,
    })
};
```



### (2) index.js

```javascript
var express = require('express');
var router = express.Router();
var Client = require('./lib/db.js');

/* GET home page. */
router.get('/', function(req, res, next) {
    client = Client();
    client.connect();

    client.query("select * from student where stuno='120010'", (err, results) => {
        if (err) throw err;
	  
        if (results.rows.length==0){
            res.render('index', '找不到資料');
        }else{	      
            console.log(results.rows[0].stuname);
            var stuname=results.rows[0].stuname;
            res.render('index', { title: stuname });
        }
        client.end();
    });
});

module.exports = router;
```
