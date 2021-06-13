# NuGet Packages

These .NET libraries reduce development effort to build and test an RTA service:

MAT.OCS.RTA.API
: Client API for testing and integration;  
  Supports .NET Standard 2.0 and 2.1

MAT.OCS.RTA.Services.AspNetCore
: By far the quickest way to get a new RTA service off the ground;  
  Supports ASP.NET Core 2.1, 3.1 and 5.0

MAT.OCS.RTA.Toolkit.API.GrpcClients
: Pre-compiled gRPC clients for the [Toolkit Services](../reference/services);  
  Supports .NET Standard 2.1 and .NET 5.0

Refer to the [Developer Guide](../devguide) for more details.

These libraries are all available from our [GitHub packages feed](https://github.com/mat-docs/packages).  
A free [GitHub](https://github.com/) account is required.

=== "RTA Libraries"

    | Package                             | Description                                                                |
    |-------------------------------------|----------------------------------------------------------------------------|
    | MAT.OCS.RTA.API                     | RTA client library, supporting the RTA REST API and Web Socket interface.  |
    | MAT.OCS.RTA.Model                   | RTA data model, including classes modelling the API responses.             |
    | MAT.OCS.RTA.Services                | Service interfaces to be implemented by customer-specific backends.        |
    | MAT.OCS.RTA.Services.AspNetCore     | ASP.NET API Controllers and Formatters implementing the RTA specification. |
    | MAT.OCS.RTA.Toolkit.API.GrpcClients | Pre-compiled gRPC clients for use from services or utilities.              |
    | MAT.OCS.RTA.Toolkit.API.GrpcServers | Pre-compiled gRPC servers/clients for use from services only.              |
    | MAT.OCS.RTA.StreamBuffer            | RTA stream buffering via Redis.                                            |

=== "Supporting Libraries"

    | Package                             | Description                                                                |
    |-------------------------------------|----------------------------------------------------------------------------|
    | MAT.OCS.Configuration               | ATLAS configuration model, including a builder API.                        |
    | MAT.OCS.Configuration.Json          | Binds the configuration model to JSON.                                     |
    | MAT.OCS.FFC                         | Fast Configuration API - a binary configuration format (FFC).              |
    | MAT.OCS.FFC.Configuration.Format    | Fast Configuration implementation.                                         |
    | MAT.OCS.FFC.Configuration           | Reads Fast Configuration (FFC) resources as MAT.OCS.Configuration.         |
    | MAT.OCS.FFC.ConfigSerializer        | Serializes MAT.OCS.Configuration models as Fast Configuration (FFC).       |

