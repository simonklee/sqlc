# sqlc: A SQL Compiler

![go](https://github.com/sqlc-dev/sqlc/workflows/go/badge.svg)
[![Go Report Card](https://goreportcard.com/badge/github.com/sqlc-dev/sqlc)](https://goreportcard.com/report/github.com/sqlc-dev/sqlc)

sqlc generates **type-safe code** from SQL. Here's how it works:

1. You write queries in SQL.
1. You run sqlc to generate code with type-safe interfaces to those queries.
1. You write application code that calls the generated code.

Check out [an interactive example](https://play.sqlc.dev/) to see it in action, and the [introductory blog post](https://conroy.org/introducing-sqlc) for the motivation behind sqlc.

## Streaming support

This fork contains streaming support using an `:iter` keyword. The keyword is implemented
for both Go and Kotlin generated code.

Example `:iter` query:

```
-- name: StreamAuthors :iter
SELECT * from authors;
```

Will produce query functions like:

`StreamAuthors(ctx context.Context, iter func(author Author) error) error`

The iter function will be called for each row in the sql result. You are responsible for
the actual streaming within the passed function. Example:

```
w := initSomeWriter()
iter := func(author Author) error {
    err = w.Write(author)
    if err != nil {
        return err
    }
    return nil
}

err := queries.StreamAuthors(ctx, iter)
```

Also note that if the iter function return an error, the streaming query will close and return the error.

### Building this fork

Clone and run `CGO_ENABLED=0 go build ./cmd/sqlc/main.go` from the project root.

## Overview

- [Documentation](https://docs.sqlc.dev)
- [Installation](https://docs.sqlc.dev/en/latest/overview/install.html)
- [Playground](https://play.sqlc.dev)
- [Website](https://sqlc.dev)
- [Downloads](https://downloads.sqlc.dev/)
- [Community](https://discord.gg/EcXzGe5SEs)

## Supported languages

- [sqlc-gen-go](https://github.com/sqlc-dev/sqlc-gen-go)
- [sqlc-gen-kotlin](https://github.com/sqlc-dev/sqlc-gen-kotlin)
- [sqlc-gen-python](https://github.com/sqlc-dev/sqlc-gen-python)
- [sqlc-gen-typescript](https://github.com/sqlc-dev/sqlc-gen-typescript)

Additional languages can be added via [plugins](https://docs.sqlc.dev/en/latest/reference/language-support.html#community-language-support).

## Acknowledgments

sqlc was inspired by [PugSQL](https://pugsql.org/) and
[HugSQL](https://www.hugsql.org/).
