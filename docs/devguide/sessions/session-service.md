# Session Service Basics

!!! tip

    Explore the gRPC API using [gRPC UI](../utilities.md#grpc-ui).

    This provides an intuitive web interface to issue API calls and see the responses.

## Opening a Channel

The network connection to a server is called a _channel_ in most gRPC bindings.  
This should be reused across service calls where possible, for efficiency.

The session store service is provided by the Toolkit [Session Service](../../services/rta-sessionsvc/grpc.md) (default port 2652) and [RTA Server](../../services/rta-server/grpc.md) (default port 8082).

```c#
using var sessionChannel = GrpcChannel.ForAddress("http://localhost:2652");
var sessionClient = new SessionStore.SessionStoreClient(sessionChannel);
```

## Creating/Updating a Session

In this example, `CreateIfNotExists` defines the initial properties to set if the session does not already exist, along with `Updates` to be applied in the same transaction.

If the session _does_ exist, only the `Updates` are applied.

=== "C# Sample"

    ```c#
    await sessionClient.CreateOrUpdateSessionAsync(new CreateOrUpdateSessionRequest
    {
        Identity = "001",
        CreateIfNotExists = new()
        {
            Identifier = "Demo Session",
            Timestamp = "2021-03-14T12:15:30Z"
        },
        Updates =
        {
            new SessionUpdate
            {
                SetState = (int)SessionState.Closed
            }
        }
    });
    ```

=== "JSON"

    ```json
    {
        "identity": "001",
        "state": "closed",
        "timestamp": "2021-03-14T12:15:30Z",
        "identifier": "Demo session"
    }
    ```

Each `SessionUpdate` can only do one operation:

???+ bug "Incorrect"

    In this example, only `SetQuality` is executed:

    ```c#
    await sessionClient.CreateOrUpdateSessionAsync(new CreateOrUpdateSessionRequest
    {
        Identity = "example",
        Updates =
        {
            new SessionUpdate
            {
                SetType = "type66",
                SetQuality = 0.25
            }
        }
    });
    ```

??? success "Correct"

    In this example, `SetType` and `SetQuality` are executed in a single transaction:

    ```c#
    await sessionClient.CreateOrUpdateSessionAsync(new CreateOrUpdateSessionRequest
    {
        Identity = "example",
        Updates =
        {
            new SessionUpdate
            {
                SetType = "type66"
            },
            new SessionUpdate
            {
                SetQuality = 0.25
            }
        }
    });
    ```

Updates to collections offer a consistent pattern of operations:

* `Set...` &mdash; overwrites the collection
* `Put...` &mdash; adds and updates items
* `Delete...` &mdash; deletes the specified items

For example, when defining [relationships](relationships.md), these two samples are equivalent:

=== "Using `Put` and `Delete` to modify data"

    ??? example "Starting with `"ref:aaa"`, `"ref:bbb"`, `"ref:ccc"`"

        ```c#
        await sessionClient.CreateOrUpdateSessionAsync(new CreateOrUpdateSessionRequest
        {
            Identity = "example",
            Updates =
            {
                new SessionUpdate
                {
                    SetRefAnchors = new()
                    {
                        RefUris =
                        {
                            "ref:aaa",
                            "ref:bbb",
                            "ref:ccc"
                        }
                    }
                }
            }
        });
        ```

    Merge update using `Put` and `Delete`:

    ```c#
    await sessionClient.CreateOrUpdateSessionAsync(new CreateOrUpdateSessionRequest
    {
        Identity = "example",
        Updates =
        {
            new SessionUpdate
            {
                PutRefAnchors = new()
                {
                    RefUris =
                    {
                        "ref:ccc",
                        "ref:ddd"
                    }
                }
            },
            new SessionUpdate
            {
                DeleteRefAnchors = new()
                {
                    RefUris =
                    {
                        "ref:aaa"
                    }
                }
            }
        }
    });
    ```

    Result:

    * `"ref:bbb"`
    * `"ref:ccc"`
    * `"ref:ddd"`

    !!! note

        Notice that the overlap with `"ref:ccc"` is allowed following usual REST-style PUT semantics.

=== "Using `Set` to overwrite"

    ??? example "Starting with `"ref:aaa"`, `"ref:bbb"`, `"ref:ccc"`"

        ```c#
        await sessionClient.CreateOrUpdateSessionAsync(new CreateOrUpdateSessionRequest
        {
            Identity = "example",
            Updates =
            {
                new SessionUpdate
                {
                    SetRefAnchors = new()
                    {
                        RefUris =
                        {
                            "ref:aaa",
                            "ref:bbb",
                            "ref:ccc"
                        }
                    }
                }
            }
        });
        ```

    Overwriting with `Set`:

    ```c#
    await sessionClient.CreateOrUpdateSessionAsync(new CreateOrUpdateSessionRequest
    {
        Identity = "example",
        Updates =
        {
            new SessionUpdate
            {
                SetRefAnchors = new()
                {
                    RefUris =
                    {
                        "ref:bbb",
                        "ref:ccc",
                        "ref:ddd"
                    }
                }
            }
        }
    });
    ```

    Result:

    * `"ref:bbb"`
    * `"ref:ccc"`
    * `"ref:ddd"`

    !!! note

        Notice that `"ref:aaa"` is removed from the list.

## Fetching and Listing Sessions

The [Session Service API](../../services/rta-sessionsvc/grpc.md) provides the same [query capabilities](queries.md) as the REST API, but with an extended data model covering [Relationships](relationships.md), [Folders](folders.md) and [Data Bindings](data-bindings.md).

These elements are opt-in through the `SessionElements` mask:

```c#
var session = await sessionClient.GetSessionAsync(new GetSessionRequest
{
    Identity = "example",
    Elements = new()
    {
        Json = true,
        RefAnchors = true,
        ChildOfRefs = true,
        AlternateOfRefs = true,
        Folders = true,
        DataBindings = true
    }
});
```

!!! tip

    The _MAT.OCS.RTA.Toolkit.API.GrpcClients_ [NuGet package](../../downloads/nuget.md) provides a convenience method to retrieve and parse the JSON Session Model:

    ```c#
    var sessionModel = await sessionClient.GetSessionJsonAsync("example");
    ```

Listing sessions works the same way, using the same [query](queries.md) dialect as the REST API:

```c#
var list = await sessionClient.ListSessionsAsync(new ListSessionsRequest
{
    Query = "{\"identifier\": \"Example\"}",
    SessionElements = new()
    {
        Json = true,
        DataBindings = true
    }
});
```

## Sequenced Updates

The `CreateOrUpdateSessionRequest` has an optional `sequence` field, which can help avoid re-applying stale session updates when working with a checkpoint-based system like [Apache Kafka](https://kafka.apache.org/).

If populated, the service will apply the update only if the specified sequence number is greater than previous sequence numbers. As the sequence monotonically increases &mdash; for example, using a Kafka message offset &mdash; it is safe to replay a sequence of updates without causing the session metadata to roll back.

```c#
await sessionClient.CreateOrUpdateSessionAsync(new CreateOrUpdateSessionRequest
{
    Identity = "example",
    Sequence = 100,
    Updates =
    {
        new SessionUpdate
        {
            SetType = "aaa"
        }
    }
});

await sessionClient.CreateOrUpdateSessionAsync(new CreateOrUpdateSessionRequest
{
    Identity = "example",
    Sequence = 120,
    Updates =
    {
        new SessionUpdate
        {
            SetType = "bbb"
        }
    }
});
```

## Deleting Sessions

Multiple sessions can be deleted in one operation, and deletion completes without error even if a session does not exist:

```c#
await sessionClient.DeleteSessionsAsync(new DeleteSessionsRequest
{
    Identities =
    {
        "example",
        "xxx-does-not-exist-xxx"
    }
});
```
