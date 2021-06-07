# Useful Utilities

## GRPC UI

[GRPC UI](https://github.com/fullstorydev/grpcui) is exceptionally useful for exploring the [Toolkit Services](../services/index.md).

Each service is self-describing, so you only need to connect to the service on the GRPC port, and create calls using a web browser.

For example, connecting to the [Session Service](../services/rta-sessionsvc/README.md) on the default port:

    grpcui -plaintext localhost:2652

## Postman and Insomnia

[Postman](https://www.postman.com/) is a convenient tool and popular tool for testing REST web services &mdash; such as the RTA API.

[Insomnia](https://insomnia.rest/) is a popular alternative.

## httpstat

[httpstat](https://github.com/davecheney/httpstat) is useful for benchmarking &mdash; particularly when developing a [Data Service](../integration/data-services.md).

```bash
httpstat -H "Accept: application/vnd.mat.protobuf+chunked" -o output.bin http://localhost/rta/v2/sessions/abc123/data/timestamped/0-7f
```
```
HTTP/1.1 200 OK
Server: Kestrel
Content-Type: application/vnd.mat.protobuf+chunked
Date: Wed, 28 Oct 2020 13:16:59 GMT

Body read

   DNS Lookup   TCP Connection   Server Processing   Content Transfer
[       0ms  |           5ms  |            396ms  |         14388ms  ]
             |                |                   |                  |
    namelookup:0ms            |                   |                  |
                        connect:5ms               |                  |
                                      starttransfer:402ms            |
                                                                 total:14790ms
```

## Protocol Buffers Compiler

[protoc](https://github.com/protocolbuffers/protobuf/releases) will generate idiomatic data classes and serializers in a range of languages.

!!! tip

    If you are working in .NET, Visual Studio has [integrated support for compiling protobuf](https://docs.microsoft.com/en-us/aspnet/core/grpc/dotnet-grpc) as part of your project.

    This works very well and is highly recommended.