# Some notes

## Notes for docker

Connect into the container:

```bash
docker exec -it <container-id> bash
```

Check is user has access to the database:

```bash
psql -d <DB-name> -U <DB username>
```

If successful, you should be able to select data from the `<tablename>` table.

## Notes for go

Panic recovery in background goroutines:

```go
func (app *application) background(fn func()) {
    app.wg.Add(1)
    go func() {
        defer app.wg.Done()
        defer func() {
            if err := recover(); err != nil {
                app.logger.Errorf(fmt.Sprintf("%s", err))
            }
        }()
        
        doSomeBackgroundProcessing()
    }()
}
```

Self signed certificate:

```bash
cd ./tls

go run <GO PATH>src/crypto/tls/generate_cert.go --rsa-bits=2048 --host=localhost
```

DB migrations:

The password can be encoded with this small Python line (use only the password, not the whole connection string):

```bash
python3 -c 'import urllib.parse; print(urllib.parse.quote(input("String to encode: "), ""))'
```

After that, you can run the migration:

```bash
migrate -path ./migrations -database "postgres://<username>:<password>@<url>:<port>/<dbname>?sslmode=disable" up
```

## Notes for migrate

Create a new migration:

```bash
migrate create -ext sql -dir ./migrations -seq <migration-name>
```

Push migration to the database:

```bash
migrate -path ./migrations -database "postgres://<username>:<password>@<url>:<port>/<dbname>?sslmode=disable" up
```

If something went wrong, fix the migration, force to the previous version and then push again:

```bash
migrate -path ./migrations -database "postgres://<username>:<password>@<url>:<port>/<dbname>?sslmode=disable" force <version id>

migrate -path ./migrations -database "postgres://<username>:<password>@<url>:<port>/<dbname>?sslmode=disable" up
```

## Notes for Postgres
