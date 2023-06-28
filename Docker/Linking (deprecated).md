### Link CT to another CT

```bash
docker run -d --name=vote -p 5000:80 --link redis:redis voting-app # redis is CT name, second redis is host name

# link several CTs
docker run -d --name=vote -p 5000:80 --link redis:redis --link db:db voting-app
```

### Better use Docker Compose V2 or V3