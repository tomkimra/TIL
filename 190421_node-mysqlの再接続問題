# 概要
hubotでnode-mysqlモジュールを使ってDB接続をしているが，しばらくすると以下のようなエラーが出てqueryが働かなくなる問題．
```
Error: Cannot enqueue
Query after fatal error.
```

## 原因
mySQLのセキュリティ的な仕様上，一定時間で接続が切れるようになっているとのこと

[node-mysqlで接続が切れる点を改善 - a2 Tech blog](http://ninna2.hatenablog.com/entry/2017/02/22/node-mysql%E3%81%A7%E6%8E%A5%E7%B6%9A%E3%81%8C%E5%88%87%E3%82%8C%E3%82%8B%E7%82%B9%E3%82%92%E6%94%B9%E5%96%84)

## 解決策
connection時に`PROTOCOL_CONNECTION_LOST`エラーを検出してもいいが，Poolを使うとシンプルになる．

[Node.jsでMySQLを使うメモ - Qiita](https://qiita.com/PianoScoreJP/items/7ed172cd0e7846641e13#%E3%82%B3%E3%83%8D%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%83%97%E3%83%BC%E3%83%ABpooling-connections)

## コード
```coffeescript
mysql = require('mysql')
db_config = {
  host: 'foo',
  user: 'foo',
  password: 'foo',
  database: 'foo'
}
pool = mysql.createPool(db_config)

module.exports = (robot) ->
  robot.hear /sqltest/i, (msg) ->
    pool.getConnection (e,conn) ->
      conn.query "select * from sometable", (ee,res,fields) ->
        if ee
          msg.send "Error: #{ee}"
          throw ee
        msg.send "OK"
        conn.release
```
