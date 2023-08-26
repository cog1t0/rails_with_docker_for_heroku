# Build Rails development environment with docker

## Run this command to start developing rails
**You need to install dip gem if you didn't**

### Create new project
```
dip rails new . --force --database=postgresql
docker compose build app --no-cache	
```

### Create your database
```
dip rails db:create
```

```database.yml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  host: db
  username: postgres
  password: password

development:
  <<: *default
  database: myapp_development

test:
  <<: *default
  database: myapp_test
```

### Run your server
```
dip rails server
```

