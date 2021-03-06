
# go-mysql-diff
Compare two mysql database triggers, functions, tables, columns and indexing tools

### 增加自动生成ALTER TABLE ADD COLUMN AFTER语句
```
[Info]2018/05/04 16:53:45 ALTER TABLE `oracle_server` ADD COLUMN `threshold_archivelog` int(11) NULL DEFAULT '10' AFTER `alarm_archivelog`;

[Info]2018/05/04 16:53:45 ALTER TABLE `oracle_server` ADD COLUMN `alarm_flash_recovery_area_usage` int(11) NULL DEFAULT NULL AFTER `threshold_archivelog`;

[Info]2018/05/04 16:53:45 ALTER TABLE `oracle_server` ADD COLUMN `threshold_flash_recovery_area_usage` int(11) NULL DEFAULT '90' AFTER `alarm_flash_recovery_area_usage`;

[Info]2018/05/04 16:53:45 ALTER TABLE `oracle_server` ADD COLUMN `alarm_rman_backupset_job` int(11) NULL DEFAULT '10' AFTER `threshold_flash_recovery_area_usage`;
```

### 配置文件利用[toml](https://github.com/toml-lang/toml)
格式如下：

```
# schema with the same name servers 
schemaname1 = "ch1"

schemaname2 = "ch2"

[servers]
  # You can indent as you please. Tabs or spaces. TOML don't care.
  [servers.1]
  host = "127.0.0.1"
  port = "3306"
  user = "ch"
  password = "123456"
  name = "ch1"

  [servers.2]
  host = "127.0.0.1"
  port = "3306"
  user = "ch"
  password = "123456"
  name = "ch2"
```

填写正确的数据库连接, schemaname 名称与 serers name 名称对应一致。

go run main.go

### log 日志输出到文件中，配置方法

```
// 定义一个日志
logFile, err := os.Create("diff.log")
defer logFile.Close()
if err != nil {
	log.Fatalln("open file error !")
}
// 创建一个日志对象
dLog = log.New(logFile, "[Info]", log.LstdFlags|log.Lshortfile)
//配置一个日志格式的前缀
dLog.SetPrefix("[Info]")
//配置log的Flag参数
dLog.SetFlags(dLog.Flags() | log.LstdFlags)
```

### 交叉编译适合不同系统

Mac:`GOOS=darwin GOARCH=amd64 go build -ldflags="-w -s" -o diff_mac`

Linux:`GOOS=linux GOARCH=amd64 go build -ldflags="-w -s" -o diff_linux`

Windows:`GOOS=windows GOARCH=amd64 go build -ldflags="-w -s" -o diff_win`
