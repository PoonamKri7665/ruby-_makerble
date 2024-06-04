# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...
## Build the project
With those files in place, you can now generate the Rails skeleton app using docker compose run:
```
$ docker-compose run --no-deps web rails new . --force --database=postgresql
```
First, Compose builds the image for the web service using the Dockerfile. The --no-deps tells Compose not to start linked services. Then it runs rails new inside a new container, using that image. Once it's done, you should have generated a fresh app.
list the file and you will get output of all generated rail files
* If you are running Docker on Linux, the files rails new created are owned by root. This happens because the container runs as the root user. If this is the case, change the ownership of the new files.
```
$ sudo chown -R $USER:$USER .
```
Now that you’ve got a new Gemfile, you need to build the image again. (This, and changes to the Gemfile or the Dockerfile, should be the only times you’ll need to rebuild.)
```
$ docker-compose build
```
## Connect the database
The app is now bootable, but you're not quite there yet. By default, Rails expects a database to be running on localhost - so you need to point it at the db container instead. You also need to change the database and username to align with the defaults set by the postgres image.
```
Replace the contents of config/database.yml with the following:
```
  default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: postgres
  password: password
  pool: 5
```
development:
  <<: *default
  database: myapp_development
```
test:
  <<: *default
  database: myapp_test
You can now boot the app with docker compose up. If all is well, you should see some PostgreSQL output:
```
$ docker-compose up
```
rails_db_1 is up-to-date
Creating rails_web_1 ... done
Attaching to rails_db_1, rails_web_1
db_1   | PostgreSQL init process complete; ready for start up.
db_1   |
db_1   | 2018-03-21 20:18:37.437 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
db_1   | 2018-03-21 20:18:37.437 UTC [1] LOG:  listening on IPv6 address "::", port 5432
db_1   | 2018-03-21 20:18:37.443 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db_1   | 2018-03-21 20:18:37.726 UTC [55] LOG:  database system was shut down at 2018-03-21 20:18:37 UTC
db_1   | 2018-03-21 20:18:37.772 UTC [1] LOG:  database system is ready to accept connections
Finally, you need to create the database. In another terminal, run:
```
$ docker-compose run web rake db:create
Starting rails_db_1 ... done
Created database 'myapp_development'
Created database 'myapp_test'
View the Rails welcome page!
That's it. Your app should now be running on port 3000 on your Docker daemon.
```
On Docker Desktop for Mac and Docker Desktop for Windows, go to http://localhost:3000 on a web browser to see the Rails Welcome.
# for further details check the given link
https://github.com/docker/awesome-compose/tree/master/official-documentation-samples/rails/
