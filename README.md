KVite [![GoDoc](https://godoc.org/github.com/mistifyio/kvite?status.png)](https://godoc.org/github.com/mistifyio/kvite) [![Build Status](https://drone.io/github.com/mistifyio/kvite/status.png)](https://drone.io/github.com/mistifyio/kvite/latest)
=====

## Overview ##

KVite is a simple embedded key/value store for Go that uses [SQLite](http://www.sqlite.org) for storage.

It is also horribly named. K/V + SQLite => KVite.


## Design ##

The API was influenced heavily by
[bolt](https://github.com/boltdb/bolt/). kvite is safe to use with
multiple processes.

Data is stored in a single table per kvite "instance" using a
bucket/key/value.  Keys are stored as `TEXT` in the database and
referenced as `string` in Go.
Values are stores as `BLOB` in the database and referenced as `[]byte` in Go.

Interactions with the datastore are done through transactions.

## Usage ##

Typically, one uses the `Transaction` wrapper. (Error handling omitted for clarity)

```go

import "github.com/mistifyio/kvite"

db, err := kvite.Open("/path/to/my/database.db")

err := db.Transaction(func(tx *Tx) error {
            b, err := tx.CreateBucket("test")

            err = b.Put("foo", []byte("bar"))

            val, err := b.Get("foo")

            err = b.Delete("foo")

            return nil
        })

```

For full documentation see the [godoc](https://godoc.org/github.com/mistifyio/kvite)

# Status #

kvite is currently being used in a larger production quality project.
The API is not yet stabilized, however.

