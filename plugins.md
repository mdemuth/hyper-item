# A Plugin Architecture

Suppose your backend/server is a system of [Self-contained Systems (SCS)](http://scs-architecture.org/) which use some form of *service discovery* to locate other services. In such a setup systems may provide *plugins* for other systems. The following Plugin-Architecture is based on the [Eclipse: Menu Contributions](https://help.eclipse.org/neon/index.jsp?topic=%2Forg.eclipse.platform.doc.isv%2Fguide%2Fworkbench_cmd_menus.htm). 

A plugin in the context of the [hyper-item](README.md) media type would contribute *Items*, *Links* or *Actions* to other *Items*. 

- Suppose an API would allow access to a *user* domain type. 
- A user may be open to extension by defining a well known Location-[URI (RFC 3986)](https://tools.ietf.org/html/rfc3986): example.com/user. (A *profile*-URL-Value might also be a good candidate.)
- While exposing a hyper-item representation the API would *ask* the *Plugin-registry* to contribute *Sub-Items*, *Links* and *Actions* to the representation. 
- Selection of appropriate concepts would be based on the predefined Location-URI.
- To give the contributed concept the opportunity to be as specific as possible for a given domain type, the representations URL will - if possible - be *injected* into *Actions* and *Links* as a parameter. The *value* of the [parameter](README.md#35-parameter) with the *name* *url* will be set to the representations URL. This is only possible for a contributed *Link* if it uses the *template* form or a contributed *Action* that specify a *url* parameter.
- Now suppose you would need to add a customer specific *PDF-Export* for users.
- Without a plugin architecture the *PDF-Export* would need to be integrated into the *auth* service that owns the *users*.
- With a plugin architecture a customer specific *PDF-Export-Self-Contained-System* could contribute a *Export*-Link to the *example.com/user* Location-URI:

```json
{
    "location": "example.com/user",
    "link": {
        "label": "Export",
        "rel": "alternate",
        "type": "application/pdf",
        "template": "/customer-a/export/user{?url}",
        "parameters": [
            {
                "name": "url",
                "type": "hidden"
            }
        ]
    }
}
```

- The resulting *user* representation would look like:

```json
{
    "label": "Alice",
    "type": "user",
    "id": "0001",
    "properties": [
        ...
    ],
    "items": [
        ...
    ],
    "links": [
        {
            "label": "Reload",
            "rel": "self",
            "href": "/auth/users/0001"
        },
        {
            "label": "Export",
            "rel": "alternate",
            "type": "application/pdf",
            "template": "/customer-a/export/user{?url}",
            "parameters": [
                {
                    "name": "url",
                    "type": "hidden",
                    "value": "/auth/users/0001"
                }
            ]
        }
    ],
    "actions": [
        ...
    ]
}
```

- and a Request for the export:

```
GET /customer-a/export/user?url=http://www.example.com/auth/users/0001 HTTP/1.1
Host: www.example.com

```

- of course any additional headers that are required by the API should also be send: *Accept-Language*, *Authorization*, ...
- The above is analogous for Action-Contributions. 
- For Item-Contributions the parameter *injection* will be done for each of its specified links and actions. The *render* property for items may be set to *transclude* to seamlessly embed additional information for the domain-type.


## Related Material

- [hyper-item](README.md)
- [Self-contained System (SCS)](http://scs-architecture.org/)
- [Eclipse: Menu Contributions](https://help.eclipse.org/neon/index.jsp?topic=%2Forg.eclipse.platform.doc.isv%2Fguide%2Fworkbench_cmd_menus.htm)
- [URI (RFC 3986)](https://tools.ietf.org/html/rfc3986)