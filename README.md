# hyper-item: a hypermedia specification

## Description

Hyper-item is a hypermedia type that trys to combine the best parts of the [collection+json](http://amundsen.com/media-types/collection/) and [siren](https://github.com/kevinswiber/siren) media types to enable the creation of [task-based](https://cqrs.wordpress.com/documents/task-based-ui/)/[inductive](https://msdn.microsoft.com/en-us/library/ms997506.aspx) user interfaces that have the ability to be extended by new features (properties, links, actions and subitems) at runtime.

![item](img/Expanded-Item.png)

The image above shows how an item in a generic task-based UI might look like.

![collection](img/Collection.png)

The image above shows a representation of a generic collection.

![filter](img/Collection-Filter.png)

The image above shows a representation of a generic filter component.

![sort](img/Collection-Sort.png)

The image above shows a representation of a generic sort component.

## Examples

### A User Collection

The following example shows a list of users configured within an auth service.  The `self` link shows that his collection has been filtered to only include users that have had their last-login before noon on Jan 9 2017 and sorted by the users Name in ascending order. 

```json
{
    "label": "Users",
    "type" : "users",
    "items":[
        {
            "label": "Alice",
            "type": "user",
            "id": "0001",
            "properties": [
                {
                    "label": "Name",
                    "type": "text",
                    "name": "name",
                    "value": "Alice"
                },
                {
                    "label": "Status",
                    "type": "text",
                    "name": "status",
                    "value": "activated",
                    "display": "Activated"
                },
                {
                    "label": "Last Login",
                    "type": "date",
                    "name": "last-login",
                    "value": "2017-01-08T15:09:12Z",
                    "display": "Jan 8, 2017"
                }
            ],
            "links": [
                {
                    "label": "Details",
                    "rel": "details",
                    "href": "/auth/users/0001"
                }
            ]
        },
        {
            "label": "Bob",
            "type": "user",
            "id": "0002",
            "properties": [
                {
                    "label": "Name",
                    "type": "text",
                    "name": "name",
                    "value": "Bob"
                },
                {
                    "label": "Status",
                    "type": "text",
                    "name": "status",
                    "value": "deactivated",
                    "display": "Deactivated"
                },
                {
                    "label": "Last Login",
                    "type": "date",
                    "name": "last-login",
                    "value": "2017-01-09T06:12:18Z",
                    "display": "Jan 9, 2017"
                }
            ],
            "links": [
                {
                    "label": "Details",
                    "rel": "details",
                    "href": "/auth/users/0002"
                }
            ]
        }
    ],
    "links":[
        {
            "label": "Reload",
            "rel": "self",
            "href": "/auth/users/?sort=name,ASC&filter=last-login,lt,2017-01-09T12:00:00Z"
        },
        {
            "label": "Filter",
            "rel": "filter",
            "template": "/auth/users/?sort=name,ASC{&filter*}",
            "parameters": [
                {
                    "name": "filter",
                    "type": "filter",
                    "components": [
                        {
                            "label": "Name",
                            "name": "name",
                            "type": "text",
                            "operators": [
                                {
                                    "label": "Like",
                                    "operator": "like"
                                },
                                {
                                    "label": "Not Like",
                                    "operator": "nlike"
                                }
                            ]
                        },
                        {
                            "label": "Status",
                            "name": "status",
                            "type": "select",
                            "operators": [
                                {
                                    "label": "=",
                                    "operator": "eq"
                                },
                                {
                                    "label": "!=",
                                    "operator": "neq"
                                }
                            ],
                            "options": [
                                {
                                    "label": "Activated",
                                    "value": "activated"
                                },    
                                {
                                    "label": "Deactivated",
                                    "value": "deactivated"
                                }                              
                            ]
                        },
                        {
                            "label": "Last Login",
                            "name": "last-login",
                            "type": "date",
                            "operators": [
                                {
                                    "label": "Before",
                                    "operator": "lt"
                                },
                                {
                                    "label": "After",
                                    "operator": "gt"
                                }
                            ]
                        },
                    ],
                    "value": [
                        {
                            "name": "last-login",
                            "operator": "<",
                            "value": "2017-01-09T12:00:00Z"
                        }
                    ]
                }
            ]
        },
        {
            "label": "Sort",
            "rel": "sort",
            "template": "/auth/users/?filter=last-login,lt,2017-01-09T12:00:00Z{&sort*}",
            "parameters": [
                {
                    "name": "sort",
                    "type": "sort",
                    "components":[
                        {
                            "label": "Name",
                            "name": "name",
                            "orders": [
                                {
                                    "label": "ascending",
                                    "order": "ASC",
                                }
                                {
                                    "label": "descending",
                                    "order": "DESC",
                                }
                            ]
                        }
                        {
                            "label": "Last Login",
                            "name": "last-login",
                            "orders": [
                                {
                                    "label": "ascending",
                                    "order": "ASC",
                                }
                                {
                                    "label": "descending",
                                    "order": "DESC",
                                }
                            ]
                        }
                    ],
                    "value": [
                        {
                            "name": "name",
                            "order": "ASC"
                        }
                    ],
                }
            ],
        },
    ]
}
```

### A Users Details

TODO

## Concepts

![Overview](img/diagram.png)

### <a name="item"></a> Item
An item represents the state of a domain concept.

#### `type`        
Describes the type of the item. Possible values are subject to the domain represented by the item and should be documented and linked to this item via `rel` of `profile`. 

#### `rel`         
Describes the relation of the item to its parent. Possible values are subject to the domain represented by the item and should be documented and linked to this item via `rel` of `profile`. 

#### `id`          
A document local identifier for the item that may be used as the target of URL fragments.

#### `properties`  
A list of [properties](#property) describing the current state of the item.

#### `links`
A list of [links](#link) related to the item. A root item should include a link with `rel` to `self` with the cannonical `href` for this item.

#### `actions`
A list of [actions](#action) related to the item.

#### `items`
A list of (sub-)items related to the item.

#### `render`
A rendering hint. 
- `item` in case the default rendering should be used (may be omitted)
- `none` in case the item should not be rendered. Can be used to provide local URL fragment targets that do not naturally fit into the standard tree hierarchy.
- `transclude` in case the item should be replaced with an item targeted by a link  with `rel` to `details`. (i.e. server-side includes, edge-side includes, resource contributions).

#### `label`       
A localized label for the item. 

#### `description` 
A localized description of the item.



### <a name="property"></a> Property
A property represents a part of the state of an [item](#item). 

#### `name`
The name of the property. Possible values are subject to the domain represented by the item and should be documented and linked to this item via `rel` of `profile`.

#### `value`
The value of the property. 

#### `type`
The type of the property.

- `date`
- `email`
- `hidden`
- `number`
- `password`
- `text`
- `select`
- ...

#### `label`
A localized label for the property. 

#### `description`
A localized description of the property.

#### `display`
A localized string representation of the value.




### <a name="link"></a> Link

Links the [item](#item) to another concept. The fields `href` and `template` with `parameters` are mutually exclusive. Links should be resolved via the HTTP method GET.

#### `rel`
The relation of the link to its item.
- `self` The `href` should point to the cannonical URL for this item.
- `details` The `href` should point to more detailed information about the item.
- `next` can be used by items representing a result set. It may be used to select the next page of items. 
- `previous` can be used by items representing a result set. It may be used to select the previous page of items.
- `filter` can be used by items respresenting a result set to reduce the number of items. 
- `sort` can be used by items representing a result set to reorder the items.
- `profile` should be used to describe item `types`, `rels` and property `names`.
- ...

#### `href`
The URL of the target. Links should be resolved via the HTTP method `GET`.

#### `template`
A URI template as defined by [RFC 6570 - URI Template](https://tools.ietf.org/html/rfc6570).

#### `parameters`
Parameters that are used within the URI template.

#### `type`
The media type of the target.

#### `render`
A rendering hint.
- `link` in case the default rendering should be used (may be omitted)
- `none` in case the link should not be rendered (candidate: `rel: profile`).

#### `label`
A localized label for the link. 

#### `description`
A localized description of the link. 



### <a name="action"></a> Action

#### `rel`
The relation of the action to the [item](#item). Possible values are subject to the domain represented by the item and should be documented and linked to this item via `rel` of `profile`. 

#### `href`
The URL of the target.

#### `method`
The HTTP Method to use.
- `POST`
- `PATCH`
- `PUT`
- `DELETE`


#### `type`
The media type of the action payload.
- `application/x-www-form-urlencoded`
- `application/json`
- ...

#### `parameters`
Parameters that will need to be sent.

#### `context`
Marks a parameter as the actions context. If a parameters name would be *hair-color* and this action would represent *change-hair-color* then a generic representer could use *context:hair-color* to move the action closer to the representation of the property.

#### `label`
A localized label for the action.  

#### `description`
A localized description of the action.

#### `ok`
A localized ok/submit label for the action.

#### `cancel`
A localized cancel label for the action.




### <a name="parameter"></a> Parameter

#### `name`
The name of the parameter.

#### `type`
The type of the parameter.

#### `value`
The currently used or default value.

#### `options`
A list of [select-options](#select-options) or [select-groups](#select-groups) for a select type parameter.

#### `related`
The web-related [select-options](#select-options) or [select-groups](#select-groups) for a select type parameter.

#### `components`
A list of [sort-components](#sort-component) for a sort type parmeter. A list of [filter-components](#filter-component) for a filter type parameter.

#### `pattern`
A pattern for a text type parameter.

#### `min`
A min value for a number or date type parameter.

#### `max`
A max value for a number or date type parameter.

#### `max-length`

#### `size`

#### `step`
The step size of number type parameters.

#### `cols`
The number of columns in a text-area type parameter.

#### `rows`
The number of rows in a text-area type paramenter.

#### `required`
Specifies if the parameter is required.

#### `read-only`
Specifies if the parameter is read only.

#### `multiple`
Specifies if the parameter may be set multiple times.

#### `label`
A localized label for the link.

#### `description`
A localized description of the link.




### <a name="select-option"></a> Select Option

#### `value`
The value of a select-option.

#### `label`
A localized label for the select-option.

#### `description`
A localized description of the select-option.




### <a name="select-group"></a> Select Group

#### `options`
A list of select-options or select-groups for a select-group.

#### `label`
A localized label for the select-group.

#### `description`
A localized description of the select-group.






### <a name="filter-component"></a> Filter Component

#### `name`
The name of the filter component.

#### `operators`
A list of possible filter [operators](#filter-operator) for the filter component.

#### `type`
The type of value to be set.

#### `options`
The select-options for a select type filter component.

#### `related`
The web-related select-options for a select type filter component.

#### `multiple`
Specifies if the value is an array.

#### `label`
A localized label for the filter component.

#### `description` 
A localized description of the filter component.



### <a name="filter-operator"></a> Filter Operator

#### `operator`
The operator that should be used.
- `eq` equal
- `neq` not equal
- `lt` less than
- `gt` greater than
- `leq` less than or equal to
- `geq` greater than or equal to
- `in` in 
- `nin` not in
- `like` like
- `nlike` not like
- `bet` between
- `nbet` not between
- ...


#### `label`
A localized label for the filter operator. 

#### `description`
A localized description of the filter operator.

#### `infix`
If the operator has an infix part this is a localized infix text.  (`bet`: `start` **and** `end`)





### <a name="sort-component"></a> Sort Component

#### `name`
The name of the sort component.

#### `orders`
A list of possible sort [orders](#sort-order) of the sort component.

#### `label`
A localized label for the sort component. 

#### `description`
A localized description of the sort component.



### <a name="sort-order"></a> Sort Order

#### `label` 
A localized label for the sort order.

#### `description`
A localized description of the sort order.

#### `order`
The order that should be used.  
- `ASC` ascending
- `DESC` descending

## Related Material

[collection+json](http://amundsen.com/media-types/collection/)

[siren](https://github.com/kevinswiber/siren)

[RFC 3986 - URI] (https://tools.ietf.org/html/rfc3986)

[RFC 6570 - URI Template](https://tools.ietf.org/html/rfc6570)

[e4 application model](http://help.eclipse.org/neon/index.jsp?topic=%2Forg.eclipse.platform.doc.isv%2Freference%2Fapi%2Forg%2Feclipse%2Fe4%2Fui%2Fmodel%2Fapplication%2Fpackage-summary.html)

[Task-Based UI](https://cqrs.wordpress.com/documents/task-based-ui/)

[Inductive UI](https://msdn.microsoft.com/en-us/library/ms997506.aspx)