# 04-2 網頁-查詢postgres


### 呼叫方式:

```
https://自己的Heroku專案名稱.herokuapp.com/student/120001   
```


### (1) 修改了index.js, 增加db.js

```
 <myApp>
   |___app.js  (修改)
   |
   |___<routes>
          |___ index.js     (修改)
          |___ <lib>
                 |___ db.js (增加)
    
```


### (1) app.js

```javascript
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');

//-----------------------------
var routes = require('./routes/index');
//-----------------------------

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

//-----------------------------
app.use('/student', routes);
//-----------------------------

// catch 404 and forward to error handler
app.use(function(req, res, next) {
    var err = new Error('Not Found');
    err.status = 404;
    next(err);
});

// error handlers

// development error handler
// will print stacktrace
if (app.get('env') === 'development') {
    app.use(function(err, req, res, next) {
        res.status(err.status || 500);
        res.render('error', {
            message: err.message,
            error: err
        });
    });
}

// production error handler
// no stacktraces leaked to user
app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
        message: err.message,
        error: {}
    });
});


module.exports = app;
```



### (2) db.js

```javascript
const { Client } = require('pg');

module.exports = function(){
    return new Client({
        connectionString: 'postgres://自己在heroku中的database URI',
        ssl: true,
    })
};
```



### (3) index.js

```javascript
var express = require('express');
var router = express.Router();

var Client = require('./lib/db.js');

//---------------------
// 查詢
//---------------------
router.get('/query/:no', function(req, res, next) {
    var no = req.params.no;
	
    client = Client();
    client.connect();

    client.query('select * from student where stuno=$1', [no], (err, results) => {
        if (err) throw err;
	  
        if (results.rows.length==0){
            res.render('index', { title: '找不到資料' });
        }else{	      
            var stuname=results.rows[0].stuname;
            res.render('index', { title: stuname });
        }
        client.end();
    });
});

module.exports = router;
```
