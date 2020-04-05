---
title: Documenting APIs
type: docs
---

# Documenting APIS

This will cover the basics of documenting a REST API. It is meant to be a surface level overview so you can learn to dive deeper into documenting APIs and be able to communicate with engineers (who themselves may not know how to document an API). This is based on *Documenting APIs by Tom Johnson* [https://idratherbewriting.com/learnapidoc/docapis_introtoapis.html](https://idratherbewriting.com/learnapidoc/docapis_introtoapis.html). 

## How this works
 - This will be mostly hands on, but at times there will be sections explaining more as you go along. 
 - We will be using OpenAPI 2.0. There have been changes since 3.0 but it is good to know both. Many of the Rapid7 APIs still use 2.0. 
 - Most of the reference links used will refer to the [Swagger documentation](https://swagger.io/docs/specification/about/) and not the [official GitHub repository](https://github.com/OAI/OpenAPI-Specification). This is because the GitHub repo acts as a deep dive reference while the Swagger documentation is a good overall description and easier to digest. After working with OpenAPI spec for a while, you will start to gravitate to the GitHub repo for advanced functionality. 

You will be building a file from scratch and comparing the generated code to the UI elements. If you get stuck in Stoplight you will see a list of files labeled `01_petstore`. They correspond to the section numbers. You can jump ahead to see the final section code. 

{{< hint info >}}
See Resources for a loooong list of resources about APIs and documentation. Also see About APIs for a slightly deeper dive into APIs. 
{{< /hint >}}

# Prerequisites

1. Sign up for [Stoplight](https://next.stoplight.io/)
    1. Within Stoplight, click on the Projects tab. 
    2. Then click on the project with {your-name} - Documenting APIs
    3. Click on the file `petstore_master.oas2`

![Click your project name](https://raw.githubusercontent.com/tperry-r7/ux-writing-training//master/assets/images/project_stoplight.png "Click Your Project Name In Stoplight")

{{< hint info >}}
**About the file names**
The file type `.oas2` is specific to Stoplight only. Files should be saved elsewhere as `.json` or `.yaml`
{{< /hint >}}


## Definitions

Throughout this course, you will see the following definitions used interchangeably . 

* **repo** - repository
* **spec** - specification
* **OpenAPI** - Swagger
* **OAS** - OpenAPI specification, OpenAPI
* **request** - Refers to any type of request against a REST API. This can include, GET, PUT, POST and DELETE.

# About Stoplight

There a a lot of tools that can be use to write an OpenAPI specification file. Some such as editor.swagger.io just provide an interface, but Stoplight provides a much cleaner UI that allows you to fill in boxes and see the resulting code. It's a great tool to learn with. Stoplight also doesn't try to add a lot of extra "junk" to the file. 

If you are trying to describe an API that is a bit more complicated and need to use descriptors such as [OAuth Flows](https://github.com/OAI/OpenAPI-Specification/blob/v3.0.1/versions/3.0.1.md#oauthFlowsObject) or [Discriminators](https://github.com/OAI/OpenAPI-Specification/blob/v3.0.1/versions/3.0.1.md#discriminatorObject), then you'll need to manually write the spec.


# What is OpenAPI specification?

OpenAPI specification allows you to describe the functionality of [REST APIs](https://restfulapi.net/).  It is not code, it is a way of documenting and visualizing RESTful APIs. Think of OpenAPI specification as a schematic or a blueprint for a house. 

{{< hint info >}}
This article uses Python to walk through creating a simple API. It illustrates how the documentation is not the actual code. 
[This is how easy it is to create a REST API](https://codeburst.io/this-is-how-easy-it-is-to-create-a-rest-api-8a25122ab1f3) by [Leon Wee](https://codeburst.io/@leonweecs?source=post_page-----8a25122ab1f3----------------------)
{{< /hint >}}

## OpenAPI history

OpenAPI was previously known as Swagger. Swagger was [developed](https://en.wikipedia.org/wiki/OpenAPI_Specification#History) in 2010 by Tony Tam. It was then purchased in 2015 by [SmartBear Software](https://swagger.io/docs/specification/about/).  Later in 2015, SmartBear launched the [OpenAPI Initiative](https://www.openapis.org/) which open sourced the specification. In 2016, it was renamed from Swagger to [OpenAPI Specification (OAS)](https://github.com/OAI/OpenAPI-Specification).

Before purchase SmartBear developed a lot of tooling and structure around Swagger including documentation, automation and creating SDKs. OAS is still referred to as Swagger because of the work around the specification that SmartBear did. SmartBear developed this tooling before purchasing the project. 

{{< hint info >}}
Swagger and OpenAPI (OAS) are the same thing. Most people still call it Swagger. 
{{< /hint >}}

# 01 Basic structure

OpenAPI specifications can be written using [JSON](https://www.json.org/json-en.html) or [YAML](https://yaml.org/). We will use JSON for now. JSON versus YAML is a preference and doesn't change anything about the file or description of the API. 

## In Stoplight

1. Select the file `01_petstore.oas2`
2. Select the `Code` tab. 

![Click Code Tab in Project](https://raw.githubusercontent.com/tperry-r7/ux-writing-training/master/assets/images/01_petstore_code.png)

3. Review the information that is pre-populated for you. 
4. Click the `Design` tab and compare the `Code` tab. 
 
![Click Code Tab in Project](https://raw.githubusercontent.com/tperry-r7/ux-writing-training/master/assets/images/01_petstore_design.png)


## Metadata

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
- `path` - The resources being requested. Using the InsightIDR API as an example `https://[region].api.insight.rapid7.com/idr/v1/investigations`, Investigations is the path. 
- `query parameter` - 

{{< hint info >}}
Compare the Metadata for [OpenAPI 2.0](https://swagger.io/docs/specification/2-0/basic-structure/) to [OpenAPI 3.0](https://swagger.io/docs/specification/basic-structure/).
{{< /hint >}}


# 02 Add more basic data




# 02 Add a Path

# 03 Add a Model

# 04