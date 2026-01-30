# README

Demonstrate issue with JSON column and ActiveRecord when using MariaDB instead of MySQL.

## Setup

Start MariaDB in one terminal:

```
docker-compose up
```

Create and migrate database in another terminal:

```
bin/rails db:create
bin/rails db:migrate
```

Create some data and try to iterate from a Rails console `bin/rails console`:

```ruby
user = User.create(username: "alice", fav_fruits: ["apple", "orange"])
user.fav_fruits.each do |fruit|
  puts fruit
end
# (irb):5:in `<main>': undefined method `each' for "[\"apple\", \"orange\"]":String (NoMethodError)
# user.fav_fruits.each do |fruit|
```

Connect to MariaDB running in Docker container from laptop (enter password from docker-compose.yml when prompted):

```
docker compose exec db mariadb -u root -p maria_development
mysql> select * from users;
+----+----------+---------------------+----------------------------+----------------------------+
| id | username | fav_fruits          | created_at                 | updated_at                 |
+----+----------+---------------------+----------------------------+----------------------------+
|  1 | alice    | ["apple", "orange"] | 2022-12-03 11:56:43.533841 | 2022-12-03 11:56:43.533841 |
+----+----------+---------------------+----------------------------+----------------------------+
1 row in set (0.00 sec)
```

## References

* [MariaDB Official Image](https://hub.docker.com/_/mariadb)
* [MySQL Command Line Reference](https://dev.mysql.com/doc/refman/8.0/en/mysql-command-options.html)
