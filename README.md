# kwkhtmltopdf_server

A web server accepting [wkhtmlpdf](https://wkhtmltopdf.org) options and files
to convert as multipart form data.

It is written in go.

# kwkhtmltopdf_client

A drop-in replacement for [wkhtmlpdf](https://wkhtmltopdf.org) which invokes
the above server defined in the `KWKHTMLTOPDF_SERVER_URL` environment variable.

There are two clients:

* a go client (preferred)
* a python client, which only depends on the `requests` library.
  It should work with any python version supported by `requests`.

# Quick start

## Run the server

```
$ docker run -it --rm -p 8080:8080 acsone/kwhkhtmltopdf
```

or

```
$ go run server/kwkhtmltopdf_server.go
```

The server should now listen on http://localhost:8080.

## Run the client

```
$ go build -o client/go/kwkhtmltopdf_client client/go/kwkhtmltopdf_client.go
$ env KWKHTMLTOPDF_SERVER_URL=http://localhost:8080 \
    client/go/kwkhtmltopdf_client https://wkhtmltopdf.org /tmp/test.pdf
```

or

```
$ env KWKHTMLTOPDF_SERVER_URL=http://localhost:8080 \
    client/python/kwkhtmltopdf_client.py https://wkhtmltopdf.org /tmp/test.pdf
```

This should generate a printout of the wkhtmltopdf home page to /tmp/test.pdf.

# Run tests

1. Start the server.
2. Set and export `KWKHTMLTOPDF_SERVER_URL` environment variable.
3. Run `tox`.

# Roadmap

See [issues on GitHub](<https://github.com/acsone/kwkhtmltopdf/issues>)
as well as some TODO's in the source code.

# WARNING

The server is not meant to be exposed to untrusted clients.

Several attack vectors exist (local file access being the most obvious).
Mitigating them is not a priority, since the main use case is
to use it as a private service.

# Releasing

Push the master branch and ensure tests pass on travis.

Create and push a git tag.

Travis will create a GitHub release with the client and server binaries.

[Docker Hub](https://cloud.docker.com/u/acsone/repository/docker/acsone/kwkhtmltopdf) will build the images.

# Credits

Author: stephane.bidoul@acsone.eu.

Contributors:

* Nils Hamerlinck

# License

MIT
