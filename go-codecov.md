# Go codecov

## codecov

- Sign in for [codecov](https://codecov.io/).
- Choose Your repo.
- Copy the token of that repo.

## GitHub

- In your README.md file.

  - Copy and add that link:

  ```
    <a href="https://codecov.io/gh/<your_github_user_name>/<your_repo>">
        <img src="https://codecov.io/gh/<your_github_user_name>/<your_repo>/branch/main/graph/badge.svg"/>
    </a>
  ```

  - Change the variables to suit you.

- Open repo setting and add a secret which the key is CODECOV_TOKEN and the value is the token you obtained.

## Snapshot of the code

### in yaml file

- Located in .github/workflow directory

```
name: codecov
on: [push]
jobs:
  codecov:
    name: codecov
    runs-on: ubuntu-latest
    steps:
      - name: Set up Go <go_version>
        uses: actions/setup-go@v1
        with:
          go-version: <go_version>
        id: go

      - name: Check out code into the Go module directory
        uses: actions/checkout@v1

      - name: Get dependencies
        run: |
          go get -v -t -d ./...
          if [ -f Gopkg.toml ]; then
              curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh
              dep ensure
          fi

      - name: Generate coverage report
        run: |
          go test `go list ./...`  -coverprofile=coverage.txt -covermode=atomic

      - name: Upload coverage report
        uses: codecov/codecov-action@v1.0.2
        with:
          token: ${{ secrets.CODECOV_TOKEN }}
          file: ./coverage.txt
          flags: unittests
          name: codecov-umbrella

```

- Change the variable to suit you: <go_version>.

### To exclude directories that aren't required for test(ex. examples directory)

```
      - name: Generate coverage report
        run: |
          go test `go list ./... | grep -v examples`  -coverprofile=coverage.txt -covermode=atomic

```

## Reference

[CODECOV](https://github.com/marketplace/actions/codecov)
