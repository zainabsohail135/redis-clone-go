# Redis-Compatible In-Memory Database Server (Go)

A Redis-compatible in-memory key-value database server built from scratch in Go — no external libraries, just the Go standard library.

Implements the [RESP (Redis Serialization Protocol)](https://redis.io/docs/reference/protocol-spec/), so it works with the real `redis-cli` and any Redis-compatible client.

## Features

- **TCP server** with concurrent connection handling (goroutines) — supports multiple simultaneous clients
- **RESP protocol parser and serializer** — reads and writes Simple Strings, Bulk Strings, Arrays, Errors, and Null responses
- **Commands implemented:** `PING`, `SET`, `GET`, `HSET`, `HGET`, `DEL`
- **Thread-safe in-memory storage** using `sync.RWMutex` to safely handle concurrent reads/writes
- **AOF (Append-Only File) persistence** — every write command is logged to disk and replayed on startup, so data survives server restarts

## Architecture

| File | Responsibility |
|---|---|
| `main.go` | TCP server, connection loop, command routing |
| `resp.go` | RESP protocol parsing (bytes → `Value`) and serialization (`Value` → bytes) |
| `writer.go` | Writes serialized responses back to the client |
| `handler.go` | Command implementations (`SET`, `GET`, `HSET`, `HGET`, `DEL`, `PING`) and in-memory data store |
| `aof.go` | Append-only file persistence — write-ahead logging and replay on startup |

## Running it

```bash
go run .
```

The server listens on port `6379` (Redis's default port).

## Testing it

Using the real `redis-cli`:

```bash
redis-cli -p 6379 set name Ahmed
redis-cli -p 6379 get name
redis-cli -p 6379 del name
```

## What this project demonstrates

- Building a network protocol parser/serializer from raw bytes
- TCP socket programming and concurrent connection handling in Go
- Safe concurrent access to shared state (mutexes, race condition avoidance)
- Basic database durability concepts (write-ahead logging / append-only file persistence)

## Built following

[Build Redis from Scratch](https://www.build-redis-from-scratch.dev/) by Ahmed Ash, with additional fixes for concurrent multi-client support.
