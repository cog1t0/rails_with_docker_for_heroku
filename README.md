# Build Rails development environment with docker

## Run this command to start developing rails
**You need to install dip gem if you didn't**

### Create new project
```
dip rails new . --force --no-deps --database=postgresql
docker compose build app --no-cache	
```

### Create your database
```
dip rails db:create
```

### Run your server
```
dip rails server
```
