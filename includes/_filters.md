# Product filters

Product filters can help you find the products you need in a big catalog. 

Ecwid API allows you to get the available filter facets in a store and then use them to search for products in the REST API and in storefront. 

**Table of contents**

- [Get filter facets](https://developers.ecwid.com/api-documentation/get-store-filters)
- [Find products](https://developers.ecwid.com/api-documentation/find-products-using-filters)

There is a native product filters widget available for the Ecwid storefronts. Learn more about it in our [Help Center](https://support.ecwid.com/hc/en-us/articles/207807925).

## Get store filters

Each store has general set of filter facets enabled by default: filter by price, availability, availability in stock, whether it belongs to a specific category and whether it's on sale. 

Store owners can also choose their own set of product filters, for example: specific product options, like `Size` and `Color`, or attributes, like `Brand`.

The product options and attribute names are available in the product details. So first you should learn what kind of options and attributes a merchant's store has by inspecting the catalog. 

Afterwards, can get store filter facets from the Ecwid API – find available values for specific product options, price, keywords, attributes and more.

<aside class="notice">
To access the Ecwid API Platform features, make sure you have a registered application and a test Ecwid store on a paid plan.
</aside>

> Request example

```http
GET /api/v3/4870020/products/filters?filterParentCategoryId=123532&inventory=instock&lang=ru&token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
Accept-Encoding: gzip
```

`GET https://app.ecwid.com/api/v3/{storeId}/products/filters?filterFields={filterFields}&filterFacetLimit={filterFacetLimit}&filterParentCategoryId={filterParentCategoryId}&keyword={keyword}&priceFrom={priceFrom}&priceTo={priceTo}&categories={categories}&includeProductsFromSubcategories={includeProductsFromSubcategories}&createdFrom={createdFrom}&createdTo={createdTo}&updatedFrom={updatedFrom}&updatedTo={updatedTo}&enabled={enabled}&inventory={inventory}&onsale={onsale}&field{attributeName}={attributeValues}&field{attributeId}={attributeValues}&option_{optionName}={optionValues}&attribute_{attributeName}={attributeValues}&lang={lang}&token={token}`

<aside class="notice">
Parameters in <strong>bold</strong> are mandatory
</aside>

**Filter limits**

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**filterFields** | string | Comma-separated list of filters for Ecwid to return. Supported filters: `"price"`,`"inventory"`,`"onsale"`,`"categories"`, `"option_{optionName}"`, `"attribute_{attributeName}"`. Example: `"price,inventory,option_Size,attribute_Brand,categories"`. If option or attrubute has a comma or a backslash in its name, escape it with a backslash: `"\"`. I.e. option name `"Color, size"` will transform to `"option_Color\, size"` in your request URL
filterFacetLimit | string | Set the number of filter values in response. Individual limit example: `"onsale:all,attribute_Brand:50,option_Color:10"`. General limit example: `"10"`. Use `"all"` to return all facets. Default limit is 50
filterParentCategoryId | string | Set parent category ID limit for `categories` filter field in response. `"0"` or `"home"` or empty value means there is no parent category. If you want to limit product results by categories, use `categories` product limit field below

**Product limits**

Name | Type    | Description
---- | ------- | --------------
keyword | string | Search term. Use quotes to search for exact match. Ecwid searches products over multiple fields: <ul><li>title</li><li>description</li><li>SKU</li><li>product options</li><li>category name</li><li>gallery image descriptions</li><li>attribute values (except for hidden attributes). If your keywords contain special characters, it may make sense to URL encode them before making a request
priceFrom | number | Minimum product price
priceTo | number | Maximum product price
categories | string | Limit results by category IDs separated by a comma. Use `"home"` to get Store Home Page products. Example: `"18265,12324,home"`
includeProductsFromSubcategories | boolean | Use `true` if you need to include products from subcategories. Use `false` otherwise. `false` is the default value
createdFrom | string | Product create date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul> Example: <ul><li>`1447804800`</li> </ul>
createdTo | string | Product last create date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedFrom | string | Product last update date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedTo | string | Product last update date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
enabled | boolean | Use `true` if you need only enabled products. Use `false` if you need both enabled and disabled products
option_{optionName} | string | Filter by product option values. Format: `option_{optionName}=param[,param]`, where `optionName` is the attribute name and `param` is the attribute value. You can place several values separated by comma. In that case, values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `option_Size=S,M,L&option_Color=Red,Black` 
attribute_{attributeName} | string | Filter by product attribute values. Format: `attribute_{attributeName}param[,param]`, where `attributeName` is the attribute name and `param` is the attribute value. You can place several values separated by comma. In that case, values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `attribute_Brand=Apple&attribute_Capacity=32GB,64GB` 
inventory | string | Use `"instock"` to get in stock items only or `"outofstock"` for out of stock items. 
onsale | string | Use `"onsale"` to get on sale items only or `"notonsale"` for items not currently on sale
lang | string | Preferred language for the translated filter fields in search results. If a certain field does not have the translation available for the set language, the default language text will be used for that field


#### Response

> Response example (JSON)

```json
{
    "productCount": 45,
    "filters": {
        "price": {
            "minValue": 0,
            "maxValue": 14776.3
        },
        "inventory": {
            "values": [
                {
                    "id": "instock",
                    "title": "In stock",
                    "productCount": 35
                },
                {
                    "id": "outofstock",
                    "title": "Out of stock",
                    "productCount": 10
                }
            ]
        },
        "onsale": {
            "values": [
                {
                    "id": "notonsale",
                    "title": "Regular price",
                    "productCount": 43
                },
                {
                    "id": "onsale",
                    "title": "On sale",
                    "productCount": 2
                }
            ]
        },
        "categories": {
            "values": [
                {
                    "id": 19563207,
                    "title": "Shorts",
                    "titleTranslated": "Шорты",
                    "productCount": 5
                },
                {
                    "id": 21579001,
                    "title": "Hats",
                    "titleTranslated": "Шляпы",
                    "productCount": 4
                },
                {
                    "id": 21898001,
                    "title": "Sale",
                    "titleTranslated": "Распродажа",
                    "productCount": 2
                },
                {
                    "id": 22515002,
                    "title": "Accessories",
                    "titleTranslated": "Аксессуары",
                    "productCount": 2
                }
                {
                    "id": 19976009,
                    "title": "Apparel",
                    "titleTranslated": "Apparel",
                    "productCount": 2
                }
            ]
        },
        "attribute_Brand": {
            "values": [
                {
                    "title": "Sunshine",
                    "productCount": 2
                },
                {
                    "title": "Envelope",
                    "productCount": 1
                }
            ]
        },
        "option_Size": {
            "title": "Size",
            "titleTranslated": "Размер",
            "values": [
                {
                    "title": "S",
                    "titleTranslated": "S",
                    "productCount": 7
                },
                {
                    "title": "L",
                    "titleTranslated": "L",
                    "productCount": 6
                },
                {
                    "title": "M",
                    "titleTranslated": "M",
                    "productCount": 6
                },
                {
                    "title": "Large",
                    "titleTranslated": "Large",
                    "productCount": 4
                },
                {
                    "title": "Medium",
                    "titleTranslated": "Medium",
                    "productCount": 4
                },
                {
                    "title": "Small",
                    "titleTranslated": "Small",
                    "productCount": 4
                },
                {
                    "title": "XL",
                    "titleTranslated": "XL",
                    "productCount": 4
                },
                {
                    "title": "2XL",
                    "titleTranslated": "2XL",
                    "productCount": 3
                }
            ]
        }
    }
}
```

A JSON object of type 'SearchResult' with the following fields:

#### SearchResult

Field | Type | Description
----- | ---- | -----------
productCount | number | The total number of found products
filters | \<*ProductFilters*\> | Store filters returned for this request

#### ProductFilters

Field | Type | Description
----- | ---- | -----------
price | \<*PriceFilter*\> | Range of available prices
inventory | Array \<*FilterValue*\> | Available values for inventory filter
onsale | Array \<*FilterValue*\> | Available values for on sale filter
categories | Array \<*FilterValue*\> | List of categories result products are assigned to
option_{optionName} | Array \<*FilterValue*\> | List of available values for a specific product option
attribute_{attributeName} | Array \<*FilterValue*\> | List of available values for a specific product attribute

#### PriceFilter

Field | Type | Description
----- | ---- | -----------
minValue | number | The minimum price found for these products
maxValue | number | The maximum price found for these products

#### FilterValue

Field | Type | Description
----- | ---- | -----------
id | number/string | Filter value ID: category ID, `"onsale"`, `"outofstock"`
**title** | string | Filter value name: option value, attribute value, "On sale", "Out of Stock", etc.
titleTranslated | string | Translated title for the current entity for the `lang` value provided in your request. If translation is not available, default langage translation is used
**productCount** | number | Number of products found for this specific filter

#### Errors

> Error response example

```http
HTTP/1.1 400 Bad request
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning | Code (optional)
------------|--------|-----------
400 | Request parameters are malformed | 
403 | Access token doesn't have `read_catalog` scope | 
415 | Unsupported content-type: expected `application/json` or `text/json` | 
500 | Cannot complete request because of an error on the server | 

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Find products using filters

You can find products according to your filter facets both in the Ecwid REST API and in merchant's storefront. 

Filter facets help you find available values for the search process. For example, learn that a store has a `Red` value for `Color` product option and `Adidas` value for `Brand` attiribute. 

**Step 1. Get filter facets**

Get available filters [in the REST API](https://developers.ecwid.com/api-documentation/get-store-filters).

**Step 2. Get products according to filters**

Once you have the required filter facets, start searching for products: 

- [Find products in Ecwid REST API](https://developers.ecwid.com/api-documentation/find-products-using-filters#find-products-in-ecwid-rest-api)
- [Find products in storefront](https://developers.ecwid.com/api-documentation/find-products-using-filters#find-products-in-storefront)

### Find products in Ecwid REST API

> Using filters in REST API example 

```http
GET https://app.ecwid.com/api/v3/api/v3/4870020/products?attribute_Brand=Adidas&option_Color=Red&priceTo=100&token=1234567890qwqeertt 
HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
Accept-Encoding: gzip
```

When you received all the needed filter facets from the Ecwid API, use them when [searching for products](https://developers.ecwid.com/api-documentation/products#search-products).

As a result, you will have the list of products that follow your search rules and all of their details, like price, SKU, images and many others.

### Find products in storefront

> Filters in query parameters on search page example

```http
GET https://mdemo.ecwid.com/search?keyword=surfboard&inventory=instock&priceFrom=20
```

You can use query parameters of a URL in the storefront to filter products on a page. Filters in query parameters are available for search and category pages. 

<aside class="notice">
You can also get selected filter facets using <a href="https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onpageload-ecwid-onpageloaded">Ecwid JS API</a> for "SEARCH" and "CATEGORY" page types.
</aside>

**Query parameters for filters**

Field | Type |  Description
--------- | ---------| -----------
keyword | string | Search by a specific phrase
priceFrom | number | Minimum product price
priceTo | number | Maximum product price
categories | string | Search within these categories. Supports IDs separated by comma and value `"home"` that refers to the Store Home Page
includeProductsFromSubcategories | boolean | Defines whether Ecwd should search in subcategories too. Only makes sense when `categories` parameter is set
createdFrom | string | Product create date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul> Example: <ul><li>`1447804800`</li> </ul>
createdTo | string | Product last create date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedFrom | string | Product last update date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedTo | string | Product last update date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
option_{optionName} | string | Filter by product option values. Format: `option_{optionName}=param[,param]`, where `optionName` is the attribute name and `param` is the attribute value. You can place several values separated by comma. In that case, values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `option_Size=S,M,L&option_Color=Red,Black` 
attribute_{attributeName} | string | Filter by product attribute values. Format: `attribute_{attributeName}=param[,param]`, where `attributeName` is the attribute name and `param` is the attribute value. You can place several values separated by comma. In that case, values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `attribute_Brand=Apple&attribute_Capacity=32GB,64GB` 
inventory | string | Search instock or out of stock products. Possible values: `"instock"`,`"outofstock"`
onsale | string | Search on sale products. Possible values: `"onsale"`, `"notonsale"`



