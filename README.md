# HP Docker TechTalk

### Requirements
[Docker for Mac](https://docs.docker.com/engine/installation/mac/)

### Add Docker to an existing project
0. CD into rails project directory

1. Create a ``Dockerfile``
  ```
  FROM ruby:2.2.0
  RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
  RUN mkdir /project
  WORKDIR /project
  ADD Gemfile /project/Gemfile
  ADD Gemfile.lock /project/Gemfile.lock
  RUN bundle install
  ADD . /project
  ```

2. Create a ``docker-compose.yml`` file
  ```
  version: '2'
  services:
    db:
      image: postgres
    web:
      build: .
      command: bundle exec rails s -p 3000 -b '0.0.0.0'
      volumes:
        - .:/project
      ports:
        - "3000:3000"
      depends_on:
        - db
  ```
  
3. Update ``config/database.yml``
  ```
  development: &default
    database: myapp_development
    username: postgres
    password:
    host: db
  ```
  
4. Build the image
  ```
  docker-compose build
  ```
  
5. docker-compose up
  ```
  docker-compose up
  ```
  
6. Create the PostgreSQL database
  ```
  docker-compose run web rake db:create
  ```

### Startup
```
docker-compose build
docker-compose up
docker-compose run web rake db:create
```
