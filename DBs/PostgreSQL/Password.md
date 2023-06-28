### Enter PSQL without password

```bash
sudo su - postgres

psql -U postgres
```

### Enter PSQL with password

```bash
psql -h localhost -U postgres -d postgres
```