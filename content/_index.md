---
title: Documenting APIs
type: docs
---

# Documenting APIS

This will cover the basics of documenting a REST API. There are other [resources](docs/resources/) that go much more in depth, but this is enough to get started. The other information about different types of APIs and different ways to document an API are in a different section, 

# Prerequisites

1. Sign up for [Stoplight](https://next.stoplight.io/)
    1. Within Stoplight, click on the Projects tab. 
    2. Then click on the project with {your-name} - Documenting APIs

![Click your project name](https://raw.githubusercontent.com/tperry-r7/ux-writing-training/master/static/images/project_stoplight.png "Image Title")

## Definitions
Throughout this course, you will see the following definitions used interchangeably . 

* repo - repository
* spec - specification
* OpenAPI - Swagger
* request - Refers to any type of request against a REST API. This can include, GET, PUT, POST and DELETE.

# About Stoplight

There a a lot of tools that can be use to render an OpenAPI specification file, but Stoplight has the best UI and doesn't try to add a lot of "junk" to the file. You can add things using a UI and then click over to see the actual code. 

# What is OpenAPI specification?

OpenAPI specification allows you to describe the functionality of [REST APIs](https://restfulapi.net/).  It is not code, it is a way of documenting and visualizing RESTful APIs. 


## History

OpenAPI was previously known as Swagger. Swagger was [developed](https://en.wikipedia.org/wiki/OpenAPI_Specification#History) in 2010 by Tony Tam. It was then purchased in 2015 by [SmartBear Software](https://swagger.io/docs/specification/about/).  Later in 2015, SmartBear launched the [OpenAPI Initiative](https://www.openapis.org/) which open sourced the specification. In 2016, it was renamed from Swagger to [OpenAPI Specification (OAS)](https://github.com/OAI/OpenAPI-Specification).

Before purchase SmartBear developed a lot of tooling and structure around Swagger including documentation, automation and creating SDKs. OAS is still referred to as Swagger because of the work around the specification that SmartBear did. SmartBear developed this tooling before purchasing the project. 

{{< hint info >}}
Swagger and OpenAPI (OAS) are the same thing. Most people still call it Swagger. 
{{< /hint >}}

Most of the reference links used will refer to the [Swagger documentation](https://swagger.io/docs/specification/about/) and not the [GitHub repository](https://github.com/OAI/OpenAPI-Specification). This is because the GitHub repo acts as a deep dive reference while the Swagger documentation is a good overall description and easier to digest. After working with OpenAPI spec for a while, you will start to gravitate to the GitHub repo for advanced functionality. 

# 01 Basic Structure

OpenAPI specifications can be written using [JSON](https://www.json.org/json-en.html) or [YAML](https://yaml.org/). We will use JSON for now. 

## Metadata
Every OpenAPI specification must include the following metadata:

* swagger - The version of swagger that is being used. The current available versions are 2.0 and 3.0. Depending on the version being used, it will allow you to describe your REST API in different ways. The actual code base did not change, but how you are communicating it changes. 
* info - A json object that includes the title and version. 
* title - Title of the API
* version - API version. This is related to your own APIs versioning. Not the specification version. 
* host - The URL or IP address to return data. For example, `[https://us.api.insight.rapid7.com`
* paths - The paths against the host the API is available at. For example, `v2`

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

## Base path, host and schemes

When making a request against and API, there is a basic structure REST APIs follow. 
- operation - The verb (action) being taken. In the example below, GET will return data. 
-  scheme - This is usually http or https. Other, less common schemes are
	- ws - [WebSocket](https://en.wikipedia.org/wiki/WebSocket)
	- wss - [WebSocketSecure](https://en.wikipedia.org/wiki/WebSocket)
- host
- basePath
- path
- query parameter - 

![Swagger API path](https://static1.smartbear.co/swagger/media/images/url-structure.png)

