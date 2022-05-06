## Project Init

1. create Database , user, grant privilege on db to user
```plsql
create database mydb;
create user myuser with encrypted password 'mypass';
grant all privileges on database mydb  to myuser;
```
2. create .env
3. knex migration
```
yarn knex migrate:make 01create_example
yarn knex migrate:latest

###### if necessary 
yarn knex migrate:rollback

yarn knex migrate:up

yarn knex migrate:down

```

4. knex seed
```
yarn knex seed:make 01_example
yarn knex seed:run
```
5. start server 
6. start react app
