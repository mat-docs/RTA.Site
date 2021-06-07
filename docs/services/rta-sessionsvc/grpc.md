# GRPC API

The service provides this private GRPC API to create and manage sessions.

This API enables you to integrate this service into your private data environment.  
It's not part of the RTA interface specification, and never called by ATLAS.

Typical Uses:

* Define sessions as part of a batch process after new data arrives
* Hook into your ingest pipeline, exposing new sessions to ATLAS in real time
* Integrate with your other data management tools and web applications

## Definition

Save this definition to file, and use [GRPC tools](https://grpc.io/) to create a client in your preferred language.

``` protobuf
--8<--
../protos/Toolkit/session_service.proto
--8<--
```