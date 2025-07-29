# TLS 1.3 0-RTT Example with Replay Resistance and Forward Secrecy

This repository demonstrates a custom TLS 1.3 setup based on a fork of [mint](https://github.com/michellejns/mint), focused on:

- **0-RTT data transmission using PSKs**
- **Replay resistance through key puncturing and session tracking**
- **Forward secrecy**
- **Benchmarking performance: latency, efficiency, and crypto cost**

## Features

- Modified TLS 1.3 handshake using the [mint fork by michellejns](https://github.com/michellejns/mint)
- Persistent PSK caching via `psk_cache.gob`
- Support for `EarlyData` and `PSKModeDHEKE`
- Key invalidation after use (puncturing) for forward secrecy
- Session ID tracking to detect and prevent replay attempts
- Benchmark loops for both full handshakes and 0-RTT handshakes
- CSV-based performance evaluation and Go-based analysis tool

## Project Structure

├── client.go # TLS client with persistent PSK cache and 0-RTT support
├── server.go # TLS server with EarlyData support and replay protection
├── demomintclient.go # Minimal client using in-memory PSK cache
├── demomintserver.go # Minimal example server with basic TLS setup
├── keymanager.go # Manages used session IDs for replay resistance
├── analyze_benchmark.go # Analyzes CSV benchmark data
├── psk_cache.gob # Serialized PSK cache (created during runtime)
├── benchmark_results.csv # Results of 0-RTT handshake benchmarks
├── fullhandshake_benchmark.csv # Results of full TLS handshake benchmarks




## Usage

### 1. Start the TLS Server

bash
go run server.go

This starts a TLS 1.3 server on localhost:4444 with:

RSA self-signed certificate
PSK resumption support
EarlyData support
Puncturing of PSKs after 0-RTT use

go run client.go

First run: performs a full handshake and stores the PSK in psk_cache.gob
Subsequent runs: use 0-RTT if a valid PSK is found in the cache
Outputs CSV benchmark data


go run analyze_benchmark.go

This script parses both benchmark_results.csv and fullhandshake_benchmark.csv, 

printing:
Number of runs
Min, max, and mean handshake time
Standard deviation

Go 1.20 or later
Fork of mint by michellejns
Make sure it is cloned and available in your Go module path

## Background

This project was created as part of a Master's thesis at FernUniversität in Hagen on the topic of:

“Vorwärtssichere und Replay-Resistente 0-RTT Schlüsselaustauschverfahren”

The goal was to implement and analyze the performance and security of 0-RTT handshakes with focus on:

Replay protection
Forward secrecy
Benchmark-based evaluation
