# hyper-item: a hypermedia specification

## Description

## Examples

## Concepts

![Overview](img/diagram.svg)

### Item
An item represents the state of a domain concept.

#### `type`        
Describes the type of the item. Possible values are subject to the domain represented by the item and should be documented and linked to this item via `rel` of `profile`. 

#### `rel`         
Describes the relation of the item to its parent. Possible values are subject to the domain represented by the item and should be documented and linked to this item via `rel` of `profile`. 

#### `id`          
A document local identifier for the item that may be used as the target of URL fragments.

#### `properties`  
A list of properties describing the current state of the item.

#### `links`
A list of links related to the item. A root item should include a link with `rel` to `self` with the cannonical `href` for this item.

#### `actions`
A list of actions related to the item.

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



### Property
A property represents a part of the state of an item. 

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
A localized label for the property based on the `Accept-Language` - Header. 

#### `description`
A localized description of the property based on the `Accept-Language` - Header.

#### `display`
A localized string representation of the value based on the `Accept-Language` - Header.




### Link

Links the item to another concept. The fields `href` and `template` with `parameters` are mutually exclusive. Links should be resolved via the HTTP method GET.

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
A URL template.

#### `parameters`
Parameters that are used within the URL template.

#### `type`
The media type of the target.

#### `render`
A rendering hint.
- `link` in case the default rendering should be used (may be omitted)
- `none` in case the link should not be rendered (candidate: `rel: profile`).

#### `label`
A localized label for the link based on the `Accept-Language` - Header. 

#### `description`
A localized description of the link based on the `Accept-Language` - Header. 



### Action

#### `rel`
The relation of the action to the item. Possible values are subject to the domain represented by the item and should be documented and linked to this item via `rel` of `profile`. 

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




### Parameter

#### `name`
The name of the parameter.

#### `type`
The type of the parameter.

#### `value`
The currently used or default value.

#### `options`
A list of select-options or select-groups for a select type parameter.

#### `related`
The web-related select-options or select-groups for a select type parameter.

#### `components`
A list of sort-components for a sort type parmeter. A list of filter-components for a filter type parameter.

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




### Select Option

#### `value`
The value of a select-option.

#### `label`
A localized label for the select-option.

#### `description`
A localized description of the select-option.




### Select Group

#### `options`
A list of select-options or select-groups for a select-group.

#### `label`
A localized label for the select-group.

#### `description`
A localized description of the select-group.






### Filter Component

#### `name`
The name of the filter component.

#### `operators`
A list of possible filter operators for the filter component.

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



### Filter Operator

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





### Sort Component

#### `name`
The name of the sort component.

#### `orders`
A list of possible sort orders of the sort component.
- `ASC` ascending
- `DESC` descending

#### `label`
A localized label for the sort component. 

#### `description`
A localized description of the sort component.



### Sort Order

#### `label` 
A localized label for the sort order.

#### `description`
A localized description of the sort order.

#### `order`
The order that should be used.  

## Related Material