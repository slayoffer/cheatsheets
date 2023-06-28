### Migrate

```bash
flyway -user=my_sql -password=23e2f345f3 -locations='filesystem:db/migration' -url="jdbc:sqlserver://192.168.1.30;database=test" -target=4  migrate

# or
flyway -user=postgres -password=postgres -url="jdbc:postgresql://<ip postgres server>:5432;database=postgres" migrate

# https://flywaydb.org/documentation/concepts/migrations
```

[Миграции](https://flywaydb.org/documentation/concepts/migrations)
