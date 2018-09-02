# Application

## Objective
Understand application structure and problems. Check followings.

- N+1 in view, serialize.
- Data insert and update.
- Data initialization in for loop.
- large data
- how to serve file
- multiple nest

Request

```
$ ack  --with-filename '\.(GET|POST|PUT|DELETE|PATCH)' app.go

app.go
738:    e.GET("/initialize", getInitialize)
739:    e.GET("/", getIndex)
740:    e.GET("/register", getRegister)
741:    e.POST("/register", postRegister)
742:    e.GET("/login", getLogin)
743:    e.POST("/login", postLogin)
744:    e.GET("/logout", getLogout)
746:    e.GET("/channel/:channel_id", getChannel)
747:    e.GET("/message", getMessage)
748:    e.POST("/message", postMessage)
749:    e.GET("/fetch", fetchUnread)
750:    e.GET("/history/:channel_id", getHistory)
752:    e.GET("/profile/:user_name", getProfile)
753:    e.POST("/profile", postProfile)
755:    e.GET("add_channel", getAddChannel)
756:    e.POST("add_channel", postAddChannel)
757:    e.GET("/icons/:file_name", getIcon)
```


DB
```
$ ack  --with-filename '(SELECT|UPDATE|DELETE|INSERT)' app.go

app.go
98:     if err := db.Get(&u, "SELECT * FROM user WHERE id = ?", userID); err != nil {
109:            "INSERT INTO message (channel_id, user_id, content, created_at) VALUES (?, ?, ?, NOW())",
127:    err := db.Select(&msgs, "SELECT * FROM message WHERE id > ? AND channel_id = ? ORDER BY id DESC LIMIT 100",
194:            "INSERT INTO user (name, salt, password, display_name, avatar_icon, created_at)"+
206:    db.MustExec("DELETE FROM user WHERE id > 1000")
207:    db.MustExec("DELETE FROM image WHERE id > 1001")
208:    db.MustExec("DELETE FROM channel WHERE id > 10")
209:    db.MustExec("DELETE FROM message WHERE id > 10000")
210:    db.MustExec("DELETE FROM haveread")
243:    err = db.Select(&channels, "SELECT * FROM channel ORDER BY id")
306:    err := db.Get(&user, "SELECT * FROM user WHERE name = ?", name)
355:    err := db.Get(&u, "SELECT name, display_name, avatar_icon FROM user WHERE id = ?",
400:            _, err := db.Exec("INSERT INTO haveread (user_id, channel_id, message_id, updated_at, created_at)"+
402:                    " ON DUPLICATE KEY UPDATE message_id = ?, updated_at = NOW()",
414:    err := db.Select(&res, "SELECT id FROM channel")
428:    err := db.Get(&h, "SELECT * FROM haveread WHERE user_id = ? AND channel_id = ?",
463:                            "SELECT COUNT(*) as cnt FROM message WHERE channel_id = ? AND ? < id",
467:                            "SELECT COUNT(*) as cnt FROM message WHERE channel_id = ?",
506:    err = db.Get(&cnt, "SELECT COUNT(*) as cnt FROM message WHERE channel_id = ?", chID)
520:            "SELECT * FROM message WHERE channel_id = ? ORDER BY id DESC LIMIT ? OFFSET ?",
536:    err = db.Select(&channels, "SELECT * FROM channel ORDER BY id")
558:    err = db.Select(&channels, "SELECT * FROM channel ORDER BY id")
565:    err = db.Get(&other, "SELECT * FROM user WHERE name = ?", userName)
589:    err = db.Select(&channels, "SELECT * FROM channel ORDER BY id")
614:            "INSERT INTO channel (name, description, updated_at, created_at) VALUES (?, ?, NOW(), NOW())",
665:            _, err := db.Exec("INSERT INTO image (name, data) VALUES (?, ?)", avatarName, avatarData)
669:            _, err = db.Exec("UPDATE user SET avatar_icon = ? WHERE id = ?", avatarName, self.ID)
676:            _, err := db.Exec("UPDATE user SET display_name = ? WHERE id = ?", name, self.ID)
688:    err := db.QueryRow("SELECT name, data FROM image WHERE name = ?",
```


functions
```
$ ack  --with-filename '(SELECT|UPDATE|DELETE|INSERT)' app.go

app.go
98:     if err := db.Get(&u, "SELECT * FROM user WHERE id = ?", userID); err != nil {
109:            "INSERT INTO message (channel_id, user_id, content, created_at) VALUES (?, ?, ?, NOW())",
127:    err := db.Select(&msgs, "SELECT * FROM message WHERE id > ? AND channel_id = ? ORDER BY id DESC LIMIT 100",
194:            "INSERT INTO user (name, salt, password, display_name, avatar_icon, created_at)"+
206:    db.MustExec("DELETE FROM user WHERE id > 1000")
207:    db.MustExec("DELETE FROM image WHERE id > 1001")
208:    db.MustExec("DELETE FROM channel WHERE id > 10")
209:    db.MustExec("DELETE FROM message WHERE id > 10000")
210:    db.MustExec("DELETE FROM haveread")
243:    err = db.Select(&channels, "SELECT * FROM channel ORDER BY id")
306:    err := db.Get(&user, "SELECT * FROM user WHERE name = ?", name)
355:    err := db.Get(&u, "SELECT name, display_name, avatar_icon FROM user WHERE id = ?",
400:            _, err := db.Exec("INSERT INTO haveread (user_id, channel_id, message_id, updated_at, created_at)"+
402:                    " ON DUPLICATE KEY UPDATE message_id = ?, updated_at = NOW()",
414:    err := db.Select(&res, "SELECT id FROM channel")
428:    err := db.Get(&h, "SELECT * FROM haveread WHERE user_id = ? AND channel_id = ?",
463:                            "SELECT COUNT(*) as cnt FROM message WHERE channel_id = ? AND ? < id",
467:                            "SELECT COUNT(*) as cnt FROM message WHERE channel_id = ?",
506:    err = db.Get(&cnt, "SELECT COUNT(*) as cnt FROM message WHERE channel_id = ?", chID)
520:            "SELECT * FROM message WHERE channel_id = ? ORDER BY id DESC LIMIT ? OFFSET ?",
536:    err = db.Select(&channels, "SELECT * FROM channel ORDER BY id")
558:    err = db.Select(&channels, "SELECT * FROM channel ORDER BY id")
565:    err = db.Get(&other, "SELECT * FROM user WHERE name = ?", userName)
589:    err = db.Select(&channels, "SELECT * FROM channel ORDER BY id")
614:            "INSERT INTO channel (name, description, updated_at, created_at) VALUES (?, ?, NOW(), NOW())",
665:            _, err := db.Exec("INSERT INTO image (name, data) VALUES (?, ?)", avatarName, avatarData)
669:            _, err = db.Exec("UPDATE user SET avatar_icon = ? WHERE id = ?", avatarName, self.ID)
676:            _, err := db.Exec("UPDATE user SET display_name = ? WHERE id = ?", name, self.ID)
688:    err := db.QueryRow("SELECT name, data FROM image WHERE name = ?",
```


### TODO add example
