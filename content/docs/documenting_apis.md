---
title: Documenting APIs
type: docs
weight: 1
---



# Documenting APIS

This will cover the basics of documenting a REST API. It is meant to be a surface level overview so you can learn to dive deeper into documenting APIs and be able to communicate with engineers (who themselves may not know how to document an API). This is based on *Documenting APIs by Tom Johnson* [https://idratherbewriting.com/learnapidoc/docapis_introtoapis.html](https://idratherbewriting.com/learnapidoc/docapis_introtoapis.html). 

## How this works
 - This will be mostly hands on, but at times there will be sections explaining more as you go along. 
 - We will be using OpenAPI 2.0. There have been changes since 3.0 but it is good to know both. Many of the Rapid7 APIs still use 2.0. 
 - Most of the reference links used will refer to the Swagger documentation (https://swagger.io/docs/specification/about/) and not the official GitHub repository (https://github.com/OAI/OpenAPI-Specification). This is because the GitHub repository acts as a deep dive reference while the Swagger documentation is a good overall description and easier to digest. After working with OpenAPI spec for a while, you will start to gravitate to the GitHub repo for advanced functionality. 

You will be building a file from scratch and comparing the generated code to the UI elements. If you get stuck in Stoplight jump to the **Check In** sections, where the file json is located. You can also find the corresponding json in the `spec` folder for this site ([https://github.com/tperry-r7/ux-writing-training/tree/master/content/spec](https://github.com/tperry-r7/ux-writing-training/tree/master/content/spec)) . They correspond to the section numbers. You can jump ahead to see the final section code. 

I was also used this as a chance to experiment a lot with displaying and organizing information, so let me know if something doesn't make sense. 

Also, remember to **SAVE**!

{{< hint info >}}
See [Resources](/docs/resources) for a list of resources about APIs and documentation. 
{{< /hint >}}

## Prerequisites

1. Sign up for [Stoplight](https://next.stoplight.io/)
    1. Within Stoplight, click on the Projects tab. 
    2. Then click on the project with {your-name} - Documenting APIs
    3. Click on the file `petstore.oas2`

![Click your project name](https://raw.githubusercontent.com/tperry-r7/ux-writing-training//master/assets/images/project_stoplight.png "Click Your Project Name In Stoplight")

{{< hint info >}}
**About the file names**<br>
The file type `.oas2` is specific to Stoplight only. Files should be saved elsewhere as `.json` or `.yaml`
{{< /hint >}}


## Definitions

Throughout this course, you will see the following definitions used interchangeably . 

* **repo** - repository
* **spec** - specification
* **OpenAPI** - Swagger
* **OAS** - OpenAPI specification, OpenAPI
* **request** - Refers to any type of request against a REST API. This can include, GET, PUT, POST and DELETE.

## About Stoplight

There are a lot of tools that can be use to write an OpenAPI specification file. Some such as [editor.swagger.io](http://editor.swagger.io/) just provide an interface, but Stoplight provides a much cleaner UI that allows you to fill in boxes and see the resulting code. It's a great tool to learn with. Stoplight also doesn't try to add a lot of extra "junk" to the file. 

If you are trying to describe an API that is a bit more complicated and need to use descriptors such as OAuth Flows (ttps://github.com/OAI/OpenAPI-Specification/blob/v3.0.1/versions/3.0.1.md#oauthFlowsObject) or Discriminators (https://github.com/OAI/OpenAPI-Specification/blob/v3.0.1/versions/3.0.1.md#discriminatorObject), then you'll need to manually write the spec.


# What is OpenAPI specification?

OpenAPI specification allows you to describe the functionality of [REST APIs](https://restfulapi.net/).  It is not code, it is a way of documenting and visualizing RESTful APIs. Think of OpenAPI specification as a schematic or a blueprint for a house. OpenAPI is a format that can be read by both humans and machines. 

{{< hint info >}}
This article uses Python to walk through creating a simple API. It illustrates how the documentation is not the actual code. 
[This is how easy it is to create a REST API](https://codeburst.io/this-is-how-easy-it-is-to-create-a-rest-api-8a25122ab1f3) by [Leon Wee](https://codeburst.io/@leonweecs?source=post_page-----8a25122ab1f3----------------------)
{{< /hint >}}

<!-- No need to read it now. It's for reference later -->

## OpenAPI history

OpenAPI was previously known as Swagger. Swagger was developed (https://en.wikipedia.org/wiki/OpenAPI_Specification#History) in 2010 by Tony Tam. It was then purchased in 2015 by SmartBear Software (https://swagger.io/docs/specification/about/). Later in 2015, SmartBear launched the OpenAPI Initiative (https://www.openapis.org/) which open sourced the specification. In 2016, it was renamed from Swagger to OpenAPI Specification (OAS)(https://github.com/OAI/OpenAPI-Specification).

Before purchase SmartBear developed a lot of tooling and structure around Swagger including documentation, automation and creating SDKs. OAS is still referred to as Swagger because of the work around the specification that SmartBear did. SmartBear developed a lot of tooling before purchasing the project. 

{{< hint info >}}
Swagger and OpenAPI (OAS) are the same thing. Most people still call it Swagger. 
{{< /hint >}}

# Map out our API

You are building an API for a petstore. To help us visualize what we are documenting, let's think about an API in the way engineers and product managers do. We are building a Petstore API. 

We know we want people to: 
 - View information about their pets
 - View all the pets in a store
 - Add a new pet
 
 Since this is a store, you want to be able to:
 - Create a new order
 - View information about an order 

With this in mind, you can decide what information they can create (add a pet) and what information they can view (view all the information about their pets). 

<!-- I already worked out the fields that will be available. We'll get to them later on.-->

# 01 Basic structure

OpenAPI specifications can be written using [JSON](https://www.json.org/json-en.html) or [YAML](https://yaml.org/). You will use JSON for now. JSON versus YAML is a preference and doesn't change anything about the file or description of the API. 

##  Review your specification file

1. Make sure you are in the `petstore.oas` file. 
4. It will open on the **Design** tab. 
 
![Design Tab in Project](https://raw.githubusercontent.com/tperry-r7/ux-writing-training/master/assets/images/01_petstore_design.png)

5. Click on the **Code** tab and compare what you see there versus the **Design** tab. 


## Metadata
<!-- Talk about the fields that are there and show your screen -->

Every OpenAPI specification must include the following [metadata](https://swagger.io/docs/specification/basic-structure/):

* `swagger` - The version of swagger that is being used. The current available versions are 2.0 and 3.0. Depending on the version being used, it will allow you to describe your REST API in different ways. The actual code base did not change, but how you are communicating it changes. 
* `info` - A json object that includes the title and version. 
* `title` - Title of the API
* `version` - API version. This is related to your own APIs versioning. Not the specification version. 
* `host` - The URL or IP address to return data. For example, `https://us.api.insight.rapid7.com`
* `paths` - The paths against the host the API is available at. For example, `v2`. 

```json
{
  "swagger": "2.0",
  "info": {
    "title": "01_Petstore API",
    "version": "1.0"
  },
  "host": "example.com",
  "paths": {}
}
```

# 02 More basic data

We are going to add more basic information for Petstore including the `host`, `basepath`, `version` and `schemes`.  This information helps provide some basic data to the user and machine about the API. 

## Add more metadata
<!-- Review this section on your screen. This data is because I am using a sample API provided by SmartBear (Swagger). Let's go ahead and add these fields, then I'll go over some of the less obvious ones. -->

 1. Make sure you are on the **Design** tab and **Home** is highlighted. 
 2. Change the following fields to match.
 
 - **Title** - Petstore API
 - **Description** - Whatever you want this to be. *Optional*
 - **API Host** - petstore.swagger.io
 - **API Base Path** - /v2
 - **Version** - 1.0.5. *Optional*
 - **Terms of Service URL** - http://swagger.io/terms/
 - **Global Settings**
	 - **Supported Schemes** - HTTP and HTTPS
	 - **Request Mime Types** - application/json
	 - **Response Mime Types** - application/json
 - **Contact**
	 - **Email** - apiteam@swagger.io *Optional*
 - **License**
	 - **Name** - Apache 2.0 *Optional*
	 - **URL** - http://www.apache.org/licenses/LICENSE-2.0.html *Optional*

<!-- Go over these terms here-->

A few new terms were introduced here:
* **MIME Type** -  (https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) is used to identify the type of data.
* **Supported Schemes** - The supported transfer protocol. It is also known as the type of security for the data transfer.
* **API Host and Base Path** - Combined these make up the server URL. We will cover more on base paths in section [04 Add paths and methods](documenting_apis/#04-paths-and-methods). 

3. Click on the **Code** tab and compare what you see there versus the **Design** tab. 


{{< hint success >}}
# Check In
At this point your json should look like this:
{{< expand "02 More Basic Data" >}}
```json
{
  "swagger": "2.0",
  "info": {
    "title": "Petstore API",
    "version": "1.0.5",
    "description": "This is a sample server Petstore server. For this sample, you can use the api key `special-key` to test the authorization filters.",
    "termsOfService": "http://swagger.io/terms/",
    "contact": {
      "email": "apiteam@swagger.io"
    },
    "license": {
      "name": "Apache 2.0",
      "url": "http://www.apache.org/licenses/LICENSE-2.0.html"
    }
  },
  "host": "petstore.swagger.io",
  "paths": {},
  "basePath": "/v2",
  "schemes": [
    "http",
    "https"
  ],
  "consumes": [
    "application/json"
  ],
  "produces": [
    "application/json"
  ]
}
```
{{< /expand >}}
{{< /hint >}}


# 03 Tags

[Tags](https://swagger.io/docs/specification/2-0/grouping-operations-with-tags/) are a way to group similar endpoints. Based on mapping out our API, we know we will have Pets and a Store. Tags are usually based on the endpoint, but you can create them to organize the information however you want. 

## Add tags

This time, we will be adding tags in the **Code** tab. 
<!-- Show the steps on your screen -->

1. In the `json` find the line that says:
```json
  "produces": [
    "application/json"
  ]
  ```
 2. Directly below that line, add a `tags` array. Don't forget the comma. 
 
 ```json
 ...
  "produces": [
    "application/json"
  ],
  "tags": []
 ```

2. Add an object for `Store` that includes a name and description. 

 ```json
 ...
  "produces": [
    "application/json"
  ],
  "tags": [
    {
      "name": "Store",
      "description": "All about the store"
    }
]
 ```

 3. Repeat this for the `Pet` object. 
 ```json
 ...
  "produces": [
    "application/json"
  ],
  "tags": [
    {
      "name": "Store",
      "description": "All about the store"
    },
      {
      "name": "Pet",
      "description": "Everything about your pet. "
    }
]
 ``` 
4. Click on the **Design** tab and compare what you see there versus the **Code** tab. 

{{< hint success >}}
# Check In
At this point your json should look like this:
{{< expand "03 Tags" >}}
```json
{
  "swagger": "2.0",
  "info": {
    "title": "Petstore API",
    "version": "1.0.5",
    "description": "This is a sample server Petstore server. For this sample, you can use the api key `special-key` to test the authorization filters.",
    "termsOfService": "http://swagger.io/terms/",
    "contact": {
      "email": "apiteam@swagger.io"
    },
    "license": {
      "name": "Apache 2.0",
      "url": "http://www.apache.org/licenses/LICENSE-2.0.html"
    }
  },
  "host": "petstore.swagger.io",
  "paths": {},
  "basePath": "/v2",
  "schemes": [
    "http",
    "https"
  ],
  "consumes": [
    "application/json"
  ],
  "produces": [
    "application/json"
  ],
  "tags": [
    {
      "name": "Store",
      "description": "All about the store"
    },
    {
      "name": "Pet",
      "description": "Everything about your pet. "
    }
  ]
}
```
{{< /expand >}}
{{< /hint >}}

# 04 Paths and methods
A method is the type of request being made. Such as `GET` data, `PUT` or update data. For a full list of methods see https://restfulapi.net/http-methods/. 

A resource is either singular or it is a collection. For example `pet` vs `pets`.
A common structure is to use a collection `pets` then use a sub resource to return a singular piece of data in the resource `pets/{petId}`. 

A path is the location on the resource. For example `/pets` is the resource, while `/pets/getPetByID` is the path. 

We are going to add a GET method for pets. The path will be `/pets`. This is because we want our path to reflect the resource being used. For more on naming resources (endpoints), see https://restfulapi.net/resource-naming/.

{{< hint info >}}
OpenAPI 2.0 and OpenAPI 3.0 treat these differently. While we are focusing on 2.0, it's worth it to go back later and review OpenAPI 3.0 for host and bath paths. https://swagger.io/docs/specification/api-host-and-base-path/
{{< /hint >}}

## Add paths and methods

Let's add a method for returning all pets. 

1. Make sure you are on the **Design** tab. 
2. Click the `+` sign next to **Paths**. 
3. Take a moment to review the methods available in the drop down. 
4. Make sure the method is `GET`
5. Change `/newPath` to say `/pets`. 
6. Change summary to `Returns all pets in a store`.
7. Change operation ID to `getAllPets`
8. Change `tags` to `Pet`.
9. Click on the **Code** tab and compare what you see there versus the **Design** tab. 

Follow the same steps to add the following two endpoints. If you're up to the challenge try writing one by hand. Look at the json for `/pet` and try to replicate that for one of the endpoints below. 

 - Add a Pet
	 - Method - POST
	 - Endpoint - /pet
	 - Summary - Add a new pet
	 - Description - Adds a new pet to the store.
	 - OperationID - addNewPet
	 - Tag - Pet

 - Create an Order
	 - Method - POST
	 - Endpoint - /store/order
	 - Summary - Create an order
	 - Description - Create an order in the store. 
	 - OperationID - placeOrder
	 - Tag - Store

## Base path, host and schemes

A basic API request will look like this:

```bash
curl --request GET \
  --url https://petstore.swagger.io/v2/pet/3 \
  --header 'api_key: asdfasdf'
```
The [URL](https://swagger.io/docs/specification/2-0/api-host-and-base-path/) tells the user and machine where to find the data, the operation to perform and if the returning data needs to be filtered.

![Swagger API path](https://static1.smartbear.co/swagger/media/images/url-structure.png)

- `operation` - The verb (action) being taken. In the example, GET will return data. 
-  `scheme` - The transfer protocols used by the API. Usually HTTP and HTTPS are used. Other, less common schemes are
	- `ws` - [WebSocket](https://en.wikipedia.org/wiki/WebSocket)
	- `wss` - [WebSocketSecure](https://en.wikipedia.org/wiki/WebSocket)
- `host` - The domain name or IP address (IPv4) of the host that serves the API. It may include the port number. 
- `basePath` - The URL prefix for all API paths, relative to the host root. It must start with a leading slash `/`. If `basePath` is not specified, it defaults to `/`, that is, all paths start at the host root.
- `path` - The resources being requested. Using the InsightIDR API as an example https://[region].api.insight.rapid7.com/idr/v1/`investigations`. Investigations is the path. 
- `query parameter` - Specify a filter for the data either being sent or returned. The query parameters and format can vary by API. 

When all of the above elements are combined they are known as a Uniform Resource Identifier (URI) [https://restfulapi.net/resource-naming/](https://restfulapi.net/resource-naming/). This is also often called the URL. The structure of the URI is important because it lets a developer know the server and actions they can take. 

{{< hint success >}}
# Check In
At this point your json should look like this:
{{< expand "04 Add paths and methods" >}}
```json
{
  "swagger": "2.0",
  "info": {
    "title": "Petstore API",
    "version": "1.0.5",
    "description": "This is a sample server Petstore server. For this sample, you can use the api key `special-key` to test the authorization filters.",
    "termsOfService": "http://swagger.io/terms/",
    "contact": {
      "email": "apiteam@swagger.io"
    },
    "license": {
      "name": "Apache 2.0",
      "url": "http://www.apache.org/licenses/LICENSE-2.0.html"
    }
  },
  "host": "petstore.swagger.io",
  "paths": {
    "/pets": {
      "get": {
        "responses": {
          "200": {
            "description": ""
          }
        },
        "summary": "Returns all pets in a store",
        "tags": [
          "Pet"
        ],
        "description": "Returns a list of a all pets in a store.",
        "operationId": "getAllPets"
      }
    },
    "/pet": {
      "post": {
        "responses": {
          "200": {
            "description": "",
            "schema": {
              "type": "object",
              "properties": {}
            }
          }
        },
        "summary": "Add a new pet",
        "description": "Adds a new pet to the store.",
        "operationId": "addNewPet",
        "tags": [
          "Pet"
        ]
      }
    },
    "/store/order": {
      "post": {
        "responses": {
          "200": {
            "description": "",
            "schema": {
              "type": "object",
              "properties": {}
            }
          }
        },
        "summary": "Create an order",
        "tags": [
          "Store"
        ],
        "operationId": "placeOrder",
        "description": "Create an order in the store. "
      }
    }
  },
  "basePath": "/v2",
  "schemes": [
    "http",
    "https"
  ],
  "consumes": [
    "application/json"
  ],
  "produces": [
    "application/json"
  ],
  "tags": [
    {
      "name": "Pet",
      "description": "Everything about your pet. "
    },
    {
      "name": "Store",
      "description": "All about the store"
    }
  ]
}
```
{{< /expand >}}
{{< /hint >}}


# 05 Parameters

If you go back to our [API Map](documenting_apis/#map-out-our-api), we have added to following endpoints. 

-   View information about a single pet
-   ~~Add a new pet~~
-   ~~Create a new order~~
-   View information about an order
-  ~~View all pets in a store~~

These endpoints don't require you to know any information about a specific pet or order. We still need to add ID information. So instead of needing to sort through all the data, we can return or update the data we want. Using an ID in a path is called a parameter ([https://swagger.io/docs/specification/describing-parameters/](https://swagger.io/docs/specification/describing-parameters/)). In OpenAPI specs, parameters are identified by curly braces. `{}`. You can use curly braces in the server url as well. It means the data will be user supplied. 

Other systems might allow you to get away with using brackets `[]` and `<>`. This means the file is not valid and will not work when using it for other applications or they wrote some extra code to make it work for them. It is not a valid spec file. 

## Add parameters

Using the steps from [04 Add paths and methods](documenting_apis/#04-paths-and-methods), add the following endpoints. 

 - View information about a single pet
	 - Method - GET
	 - Endpoint - /pet/{petId}
	 - Summary - Return a pet by ID
	 - Description - Returns a single pet.
	 - OperationID - getPetById
	 - Tag - Pet

 - View information about an order
	 - Method - GET
	 - Endpoint - /store/order
	 - Summary - Create an order
	 - Description - Create an order in the store. 
	 - OperationID - placeOrder
	 - Tag - Store

Click on the **Code** tab and compare what you see there versus the **Design** tab. 
<!-- Also review the read tag to see what it looks like so far and how its organized based on tags-->



{{< hint success >}}
# Check In
At this point your json should look like this:
{{< expand "05 Parameters" >}}
```json
{
  "swagger": "2.0",
  "info": {
    "title": "Petstore API",
    "version": "1.0.5",
    "description": "This is a sample server Petstore server. For this sample, you can use the api key `special-key` to test the authorization filters.",
    "termsOfService": "http://swagger.io/terms/",
    "contact": {
      "email": "apiteam@swagger.io"
    },
    "license": {
      "name": "Apache 2.0",
      "url": "http://www.apache.org/licenses/LICENSE-2.0.html"
    }
  },
  "host": "petstore.swagger.io",
  "paths": {
    "/pets": {
      "get": {
        "responses": {
          "200": {
            "description": ""
          }
        },
        "summary": "Returns all pets in a store",
        "tags": [
          "Pet"
        ],
        "description": "Returns a list of a all pets in a store.",
        "operationId": "getAllPets"
      }
    },
    "/pet": {
      "post": {
        "responses": {
          "200": {
            "description": "",
            "schema": {
              "type": "object",
              "properties": {}
            }
          }
        },
        "summary": "Add a new pet",
        "description": "Adds a new pet to the store.",
        "operationId": "addNewPet",
        "tags": [
          "Pet"
        ]
      }
    },
    "/store/order": {
      "post": {
        "responses": {
          "200": {
            "description": "",
            "schema": {
              "type": "object",
              "properties": {}
            }
          }
        },
        "summary": "Create an order",
        "tags": [
          "Store"
        ],
        "operationId": "placeOrder",
        "description": "Create an order in the store. "
      }
    },
    "/pet/{petId}": {
      "get": {
        "responses": {
          "200": {
            "description": "",
            "schema": {
              "type": "object",
              "properties": {}
            }
          }
        },
        "summary": "Return a pet by ID",
        "description": "Returns a single pet.",
        "operationId": "getPetById",
        "tags": [
          "Pet"
        ]
      },
      "parameters": [
        {
          "name": "petId",
          "in": "path",
          "type": "string",
          "required": true
        }
      ]
    },
    "/store/order/{orderId}": {
      "get": {
        "responses": {
          "200": {
            "description": "",
            "schema": {
              "type": "object",
              "properties": {}
            }
          }
        },
        "summary": "Return an order by ID",
        "description": "Return a single order",
        "operationId": "getOrderByID",
        "tags": [
          "Store"
        ]
      },
      "parameters": [
        {
          "name": "orderId",
          "in": "path",
          "type": "string",
          "required": true
        }
      ]
    }
  },
  "basePath": "/v2",
  "schemes": [
    "http",
    "https"
  ],
  "consumes": [
    "application/json"
  ],
  "produces": [
    "application/json"
  ],
  "tags": [
    {
      "name": "Pet",
      "description": "Everything about your pet. "
    },
    {
      "name": "Store",
      "description": "All about the store"
    }
  ]
}
```
{{< /expand >}}
{{< /hint >}}

# 06 Definitions

Now that we have our endpoints defined, we need to provide a definition for request and response. These definitions are also commonly known as models. Models are important because they help prevent duplication. So instead of needing to update a change in 5 locations, you can now update one location. The `$ref` is used to reference models ([https://swagger.io/docs/specification/using-ref/](https://swagger.io/docs/specification/using-ref/)) in other documents or in the same document. If you have a really large code base that interacts with each other, referencing models in other files is common. 


## Create definitions

We will break this into two steps, first we are going to create our definitions, then we will link our definitions back to the endpoints using the `$ref` tag. 

1. On the **Design** tab, click the `+` sign next to models. 

![Add new model](https://raw.githubusercontent.com/tperry-r7/ux-writing-training/master/assets/images/new_model.png)

2. For key enter Pet. The key should be unique within the whole document. 
3. Title can be left blank. It is not required. Titles are often used when they key is labeled as `petKey` or some other format. A title is a human friendly title. 
4. Click on **Editor**. We are going to add the Pet schema listed below. 

<!-- Go through the object-->

{{< expand "Pet Object">}}
```json
{
  "type": "object",
  "properties": {
    "id": {
      "type": "integer",
      "format": "int64"
    },
    "category": {
      "$ref": "#/definitions/Category"
    },
    "name": {
      "type": "string",
      "example": "doggie"
    },
    "photoUrls": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "status": {
      "type": "string",
      "description": "pet status in the store",
      "enum": [
        "available",
        "pending",
        "sold"
      ]
    }
  },
  "required": [
    "name",
    "photoUrls"
  ]
}
```
{{< /expand >}}

The Pet schema represents what a Pet looks like in the database. These are the fields that will be returned for viewing a single pet `/pet/{petId}` and the fields that are accepted when creating a new pet `/pet`. 

The schema for a request and a response can be different and when documenting the API you should make the best decision for showing the API completely and reducing duplication. 

5. Stoplight automatically loads an empty object for us. Click the `+` sign next to object and then it drops down to show a new field to fill in. 

![Add new field ](https://raw.githubusercontent.com/tperry-r7/ux-writing-training/master/assets/images/add_new_field_model.png)

Take a look at our json object. In `properties`.  The first property is `id`, it is an integer and has format of `int64`. 

6. Change field to `id`. 
7. Click **string** and change it to **integer**. Click **string** again to deselect it. Click the **X** to close the popup. 
8. Click on **0 validations**, in the format drop down choose int64.  

![Add property](https://raw.githubusercontent.com/tperry-r7/ux-writing-training/master/assets/images/add_property.gif)

9. Save the document. 
10. Review the Viewer, Raw Schema and Code tabs to see you changes. 

To add `name`:
1.  Click the `+` sign next to object and then it drops down to show a new field to fill in. 
2. Change field to `name`. 
3. Click the pencil next to `name`, add in the example `doggie` to the example field. 
<!-- Go over this box-->

To add `category`:
1.  Click the `+` sign next to object and then it drops down to show a new field to fill in. 
2. Change field to `category`. 
3. Click **string** and change it to `$ref`.  Leave the `$ref` field blank for now. We will create the model for it later. 

To add `photoUrls`:

1.  Click the `+` sign next to object and then it drops down to show a new field to fill in. 
2. Change field to `photoUrls`. 
3. Click **string** and change it to `array`.  Then change the Array Items Type to **string**.

In the `photoUrls` array, there is a object called `items`. This is not referring to a specific item. It is a standard item key that refers to items in an array. 

To add `status`:

1.  Click the `+` sign next to object and then it drops down to show a new field to fill in. 
2. Change field to `status`. 
3. Leave the type as string. 
4. Click the pencil next to `status`, add in the description from the Pet object. 
5. Click **0 Validations**. 
6. In the enum field, add the three items in the enum array. 

Enum (https://swagger.io/docs/specification/data-models/enums/) lets you set all possible values for a string field. 


Repeat the process for the Order and Category Object. 

**Order**
```json
{
  "type": "object",
  "properties": {
    "id": {
      "type": "integer",
      "format": "int64"
    },
    "petId": {
      "type": "integer",
      "format": "int64"
    },
    "quanitity": {
      "type": "integer",
      "format": "int64"
    },
    "shipDate": {
      "type": "string",
      "format": "date-time"
    },
    "status": {
      "type": "string",
      "enum": [
        "placed",
        "approved",
        "delivered"
      ]
    },
    "complete": {
      "type": "boolean"
    }
  }
```
**Category**
```json
{
  "type": "object",
  "properties": {
    "id": {
      "type": "integer",
      "format": "int64"
    },
    "name": {
      "type": "string"
    }
  }
}
```

## Request 

Request bodies are typically used with create and update operations (POST, PUT, PATCH). For example, when creating a resource using POST or PUT, the request body usually contains the representation of the resource to be created. Using our Petstore API as an example, you would make a request to create (POST) a new pet.

In this example the `--data` contains the json object we are sending to the pestore server to create a new pet with an ID of 123. 

```bash
curl --request POST \
  --url http://petstore.swagger.io/v2/pet \
  --header 'content-type: application/json' \
  --data '{
	"id": 123,
	"category": {
		"id": 123,
		"name": "string"
	},
	"name": "string",
	"photoUrls": ["string"],
	"status": "available"
}'
```


## Response

The response schema documents the response in a more comprehensive, general way, listing each property that could possibly be returned, what each property contains, the data format of the values, the structure, and other details. A response is what you expect to get back from the server after making your request. 

### Response Codes
A response code tells you and the server the status of the request. If a request was successfully, it will return a 200 or a 201 and usually the data requested. 

Errors in the 400 range indicates there was an issue on the client (user) end. 
Errors in the 500 range indicates there was an issue server side. 

Not all servers will return all error codes. This is usually decided upon when designing the API. For a full list of response codes, see HTTP Response Codes (https://developer.mozilla.org/en-US/docs/Web/HTTP/Status). 

```json
{
  "error": {
    "status": 401,
    "message": "Invalid access token"
  }
}
```

## 07 Finishing up our API

Now that you have defined all the models and endpoints, we need to link the models to the right locations in the document. 
<!-- Right now Stoplight is giving you an error that Pet and Category are not linked -->

To link Category to Pet: 

1. In the Models list, click on **Pet**. 
2. Next to category click **$ref**. 
3. In the dropdown, choose **This File** and select **Category**. 

<!-- Have them look at the section for Category and Pet-->

Now we need to update the endpoints to have the correct request and response data. For now we will leave all of our response codes as 200. This will walk you through updating `/pet Add a new pet`, then you will be able to update the rest on your own. 

1. On the **Design** tab, click **Add a new pet**. 
2. Find the heading **Request**, then click **Request Body**. 
3. Choose type body(json/XML). 
4. Click the **Editor** tab. 
5. Click on **Object**.
6. Change the type to **$ref** and select **Pet**. 
7. Find the heading **Response**.
4. Click the **Editor** tab. 
5. Click on **Object**.
6. Change the type to **$ref** and select **Pet**.  
7. Review the changes in **Code** and **Design** tabs.
<!-- Review the /pet in the code tab and go over that with them.-->

Update the remaning endpoints. 
* Return a pet by ID - Pet response
* Returns all pets in a store - Pet response
* Create an order - Order request and response
* Return an order by ID - Order response

Form data can be accepted by API. This is usually a key value pair. The most common use for uploading a file. 

# Extras

COMING SOON!!! 

- Change order of the tags
- Other ways to use params
- Other places to put responses
- Other places you can use models
- Compare this to a 3.0 OpenAPI Spec
- Queries and filters
- More on headers
- Security defintions


