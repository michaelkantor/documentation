# Platform API

The Layer Platform API is designed to empower developers to automate, extend, and integrate functionality provided by the Layer platform
with other services and applications. Example use cases include creating new conversations among a set of users matched by an application, feeding messages into conversations from external systems, and sending announcements to a group of users. For more on this see our [blog post](http://blog.layer.com/introducing-layer-platform-api).

The Platform API achieves these goals by allowing servers to

1. Trigger events based on actions within your own ecosystem
2. Interact with Layer Messages and Conversations with administrative privileges.

These goals are supported for requests from your servers only.  The only supported requests that browsers may perform are as authenticated users via the REST API (coming soon).

All API access is over `HTTPS`, and accessed from the following domain:
```text
https://api.layer.com
```
All data is sent and received as `JSON`.

## API Versioning

The API is versioned using a custom media type that encodes the wire format and the version desired. Developers must explicitly request a specific version via the `Accept` header:

```text
Accept: application/vnd.layer+json; version=1.0
```

Failure to request a specific version of the API will result in `406 (Not Acceptable)`
```
{
    id: "invalid_header",
    code: 107,
    message: "Invalid Accept header; must be of form application/vnd.layer+json; version=x.y",
    url: "https://developer.layer.com/docs/platform#overview",
    data: {
        header: "Accept"
    }
}
```

## PATCH Requests with X-HTTP-Method-Override

For environments that are unable to send `PATCH` requests, a `POST` request with the `X-HTTP-Method-Override: PATCH` header is also supported.

```console
curl  -X POST \
      -H 'X-HTTP-Method-Override: PATCH'
      -H 'Accept: application/vnd.layer+json; version=1.0' \
      -H 'Authorization: Bearer TOKEN' \
      -H 'Content-Type: application/vnd.layer-patch+json' \
      -d '[{"operation": "set",    "property": "metadata.stats.counter", "value": "10"}, \
           {"operation": "delete", "property": "metadata.admin"}]' \
      https://api.layer.com/apps/APP_UUID/conversations/CONVERSATION_UUID
```

## Sample Code
For sample code, visit the [Layer for Web Sample Code](https://github.com/layerhq/samples-web-apis) repo on Github.
