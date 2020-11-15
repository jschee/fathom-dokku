# fathom self hosted with dokku

## about

this should get you up and running a fathom-lite instance on your dokku server. if you haven't heard of fathom, please visit them at
[https://usefathom.com](https://usefathom.com). you can always setup with them directly, but if you do plan on self hosting, please consider spreading the word about fathom, contributing, or [sponsoring](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=LJ5WZVA9ER9GJ) the project.

## setup fathom instance on your server

```
dokku apps:create fathom
docker pull usefathom/fathom:latest
docker tag usefathom/fathom:latest dokku/fathom:latest
dokku tags:deploy fathom latest
```

## create db & link db

```
dokku postgres:create fathom
dokku postgres:link fathom fathom
```

## setup your env variables

first find your database_password
```
dokku postgres:info fathom
# should return something like
...
postgres://postgres:<your password is here>@dokku-postgres-fathom-database:3306
...
```
for the secret key you can just [duckduckgo a random 64 char key](https://duckduckgo.com/?q=generate+password+64&ia=answer)

```
dokku config:set fathom FATHOM_SERVER_ADDR=:8080
dokku config:set fathom FATHOM_GZIP=true
dokku config:set fathom FATHOM_DATABASE_DRIVER=mysql
dokku config:set fathom FATHOM_DATABASE_NAME="fathom-database"
dokku config:set fathom FATHOM_DATABASE_USER=mysql
dokku config:set fathom FATHOM_DATABASE_PASSWORD=<password>
dokku config:set fathom FATHOM_DATABASE_HOST=dokku-mysql-fathom-database:3306
dokku config:set fathom FATHOM_SECRET=<secret>
```

## change fathom default port to match dokkus port number

```
dokku proxy:ports-add fathom http:80:8080
dokku proxy:ports-remove fathom http:8080:8080
```

## add subdomains

```
dokku domains:add fathom fathom.myawesomewebpage.com
dokku domains:remove fathom fathom.dokku.yourdodroplet:8080
```

## letsencrypt it

```
dokku letsencrypt fathom
dokku letsencrypt:cron-job --add
```

## dont forget to set cname and point your fathom app to proper subdomain
CNAME fathom @

## point the website
If you followed instructions to a T. You should now have a fathom lite instance running on your own dokku server.

