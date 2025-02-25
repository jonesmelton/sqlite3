# SQLite bindings

This native module proves sqlite bindings for janet.

This fork updates sqlite to version 3.40.1 and includes some optional modules: json, rtree, and fts5.

## Install

```
jpm install sqlite3
```

## Building

To build, use the `jpm` tool and make sure you have janet installed.

```
jpm build
```
## Using the System's SQLite library

Usually, a Linux system has a package for SQLite installed globally.
This packages will usually have many plugins for SQlite enabled (e.g
JSON1, FTS3 etc.). The [Debian package][1] or [Gentoo ebuild][2] are good examples.

This package allows one to use it instead of the SQLite sources included with the package.
To do this use:

```
export JANET_SYSTEM_SQLITE=1
jpm build
```

Note, if you intend to install the package globally, use:

```
sudo -E jpm build
```

## Update the embedded SQLite version

You can use the jpm rule to update the version of SQLite included.
```
jpm run update-sqlite3
```

You can find the latest version https://sqlite.org/index.html.

## Example Usage

Next, enter the repl and create a database and a table.
By default, the generated module will be in the build folder.

```
janet:1:> (import build/sqlite3 :as sql)
nil
janet:2:> (def db (sql/open "test.db"))
<sqlite3.connection 0x5561A138C470>
janet:3:> (sql/eval db `CREATE TABLE customers(id INTEGER PRIMARY KEY, name TEXT);`)
@[]
janet:4:> (sql/eval db `INSERT INTO customers VALUES(:id, :name);` {:name "John" :id 12345})
@[]
janet:5:> (sql/eval db `SELECT * FROM customers;`)
@[{"id" 12345 "name" "John"}]
```

Finally, close the database connection when done with it.

```
janet:6:> (sql/close db)
nil
```

[1]: https://git.launchpad.net/ubuntu/+source/sqlite3/tree/debian/rules?h=debian/sid#n41
[2]: https://github.com/gentoo/gentoo/blob/653b190ffe5f4433112ad6786d1bfd2e26143711/dev-db/sqlite/sqlite-3.34.0.ebuild
