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
$ ack  --with-filename 'func ' app.go

app.go
41:func (r *Renderer) Render(w io.Writer, name string, data interface{}, c echo.Context) error {
45:func init() {
96:func getUser(userID int64) (*User, error) {
107:func addMessage(channelID, userID int64, content string) (int64, error) {
125:func queryMessages(chanID, lastID int64) ([]Message, error) {
132:func sessUserID(c echo.Context) int64 {
141:func sessSetUserID(c echo.Context, id int64) {
151:func ensureLogin(c echo.Context) (*User, error) {
179:func randomString(n int) string {
189:func register(name, password string) (int64, error) {
205:func getInitialize(c echo.Context) error {
214:func getIndex(c echo.Context) error {
233:func getChannel(c echo.Context) error {
263:func getRegister(c echo.Context) error {
271:func postRegister(c echo.Context) error {
290:func getLogin(c echo.Context) error {
298:func postLogin(c echo.Context) error {
321:func getLogout(c echo.Context) error {
328:func postMessage(c echo.Context) error {
353:func jsonifyMessage(m Message) (map[string]interface{}, error) {
369:func getMessage(c echo.Context) error {
412:func queryChannels() ([]int64, error) {
418:func queryHaveRead(userID, chID int64) (int64, error) {
439:func fetchUnread(c echo.Context) error {
482:func getHistory(c echo.Context) error {
551:func getProfile(c echo.Context) error {
582:func getAddChannel(c echo.Context) error {
601:func postAddChannel(c echo.Context) error {
624:func postProfile(c echo.Context) error {
685:func getIcon(c echo.Context) error {
711:func tAdd(a, b int64) int64 {
715:func tRange(a, b int64) []int64 {
723:func main() {
```


### TODO add example
