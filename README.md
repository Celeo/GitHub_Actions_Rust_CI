# GitHub_Actions_Rust_CI

```yml
name: Rust CI

on:
  push:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest
    if: |
      !(github.event_name == 'push' && contains(github.event.head_commit.message, '[skip ci]'))
    steps:
    - uses: actions/checkout@v2
    - name: Cache cargo registry
      uses: actions/cache@v1
      with:
        path: ~/.cargo/registry
        key: ${{ runner.os }}-cargo-registry-${{ hashFiles('**/Cargo.lock') }}
    - name: Cache cargo index
      uses: actions/cache@v1
      with:
        path: ~/.cargo/git
        key: ${{ runner.os }}-cargo-index-${{ hashFiles('**/Cargo.lock') }}
    - name: Cache cargo build
      uses: actions/cache@v1
      with:
        path: target
        key: ${{ runner.os }}-cargo-build-target-${{ hashFiles('**/Cargo.lock') }}
    - name: Build
      run: cargo build --verbose
    - name: Run tests
      run: cargo test --verbose

```
