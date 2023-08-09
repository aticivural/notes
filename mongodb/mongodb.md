install mongo db

```shell
brew install mongodb/brew/mongodb-community@4.4
```

start mongo service

```shell
brew services start mongodb-community@4.4
```

stop mongo service

```shell
brew services stop mongodb-community@4.4
```

restart mongo service

```shell
brew services restart mongodb-community@4.4
```

start mongo service manually

```shell
mongod --config /usr/local/etc/mongod.conf
```

connect to mongo server via the mongo shell

```shell
mongo
```

restore mongo dump

```shell
mongorestore --db <db-name> --verbose <path/to/dump/folder>
```

```shell
mongorestore --db paramla --verbose ./paramla
```
