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

## Notes for Postgres
