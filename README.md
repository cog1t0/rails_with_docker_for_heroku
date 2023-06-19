# Build Rails development environment with docker

## Run this command to start developing rails
**You need to install dip gem if you didn't**

### Create new project
```
dip rails new .
docker compose build app --no-cache	
```

### Only for MySQL users
#### update your config/database.yml
```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password
  host: db
```

### Create your database
```
dip rails db:create
```

### Run your server
```
dip rails server
```
