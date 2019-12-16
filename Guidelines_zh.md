# Microsoft REST API 指南

## Microsoft REST API 指南工作组

Name | Name | Name |
---------------------------- | -------------------------------------- | ----------------------------------------
Dave Campbell (CTO C+E)      | Rick Rashid (CTO ASG)                  | John Shewchuk (Technical Fellow, TED HQ)
Mark Russinovich (CTO Azure) | Steve Lucco (Technical Fellow, DevDiv) | Murali Krishnaprasad (Azure App Plat)
Rob Howard (ASG)             | Peter Torr  (OSG)                      | Chris Mullins (ASG)


<div style="font-size:150%">
文档维护者: John Gossman (C+E), Chris Mullins (ASG), Gareth Jones (ASG), Rob Dolin (C+E), Mark Stafford (C+E)<br/>
</div>

# Microsoft REST API 指南

## 1. Abstract
作为设计原则，Microsoft REST API指南鼓励应用程序开发人员通过RESTful HTTP接口访问资源。为了在遵循Microsoft REST API准则的平台上为开发人员提供尽可能流畅的体验，REST API应当遵循一致的设计准则，以使其使用起来简单直观。

本文档建立了微软REST API应该遵循的准则，以便一致地开发RESTful接口。

## 2. 目录
<!-- TOC depthFrom:2 depthTo:4 orderedList:false updateOnSave:false withLinks:true -->

- [Microsoft REST API 指南工作组](#microsoft-restapi-指南工作组)
- [1. Abstract](#1-abstract)
- [2. 目录](#2-目录)
- [3. 介绍](#3-介绍)
    - [3.1. 推荐读物](#31-推荐读物)
- [4. 解释准则](#4-解释准则)
    - [4.1. 准则的适用](#41-准则的适用)
    - [4.2. 现有服务和服务版本指导](#42-现有服务和服务版本指导)
    - [4.3. 需求语言](#43-需求语言)
    - [4.4. License](#44-license)
- [5. 分类](#5-分类)
    - [5.1. 错误](#51-错误)
    - [5.2. 缺点](#52-缺点)
    - [5.3. 潜在](#53-潜在)
    - [5.4. Time to complete](#54-time-to-complete)
    - [5.5. Long running API faults](#55-long-running-api-faults)
- [6. Client guidance](#6-client-guidance)
    - [6.1. Ignore rule](#61-ignore-rule)
    - [6.2. Variable order rule](#62-variable-order-rule)
    - [6.3. Silent fail rule](#63-silent-fail-rule)
- [7. Consistency fundamentals](#7-consistency-fundamentals)
    - [7.1. URL structure](#71-url-structure)
    - [7.2. URL length](#72-url-length)
    - [7.3. Canonical identifier](#73-canonical-identifier)
    - [7.4. Supported methods](#74-supported-methods)
        - [7.4.1. POST](#741-post)
        - [7.4.2. PATCH](#742-patch)
        - [7.4.3. Creating resources via PATCH (UPSERT semantics)](#743-creating-resources-via-patch-upsert-semantics)
        - [7.4.4. Options and link headers](#744-options-and-link-headers)
    - [7.5. Standard request headers](#75-standard-request-headers)
    - [7.6. Standard response headers](#76-standard-response-headers)
    - [7.7. Custom headers](#77-custom-headers)
    - [7.8. Specifying headers as query parameters](#78-specifying-headers-as-query-parameters)
    - [7.9. PII parameters](#79-pii-parameters)
    - [7.10. Response formats](#710-response-formats)
        - [7.10.1. Clients-specified response format](#7101-clients-specified-response-format)
        - [7.10.2. Error condition responses](#7102-error-condition-responses)
    - [7.11. HTTP Status Codes](#711-http-status-codes)
    - [7.12. Client library optional](#712-client-library-optional)
- [8. CORS](#8-cors)
    - [8.1. Client guidance](#81-client-guidance)
        - [8.1.1. Avoiding preflight](#811-avoiding-preflight)
    - [8.2. Service guidance](#82-service-guidance)
- [9. Collections](#9-collections)
    - [9.1. Item keys](#91-item-keys)
    - [9.2. Serialization](#92-serialization)
    - [9.3. Collection URL patterns](#93-collection-url-patterns)
        - [9.3.1. Nested collections and properties](#931-nested-collections-and-properties)
    - [9.4. Big collections](#94-big-collections)
    - [9.5. Changing collections](#95-changing-collections)
    - [9.6. Sorting collections](#96-sorting-collections)
        - [9.6.1. Interpreting a sorting expression](#961-interpreting-a-sorting-expression)
    - [9.7. Filtering](#97-filtering)
        - [9.7.1. Filter operations](#971-filter-operations)
        - [9.7.2. Operator examples](#972-operator-examples)
        - [9.7.3. Operator precedence](#973-operator-precedence)
    - [9.8. Pagination](#98-pagination)
        - [9.8.1. Server-driven paging](#981-server-driven-paging)
        - [9.8.2. Client-driven paging](#982-client-driven-paging)
        - [9.8.3. Additional considerations](#983-additional-considerations)
    - [9.9. Compound collection operations](#99-compound-collection-operations)
- [10. Delta queries](#10-delta-queries)
    - [10.1. Delta links](#101-delta-links)
    - [10.2. Entity representation](#102-entity-representation)
    - [10.3. Obtaining a delta link](#103-obtaining-a-delta-link)
    - [10.4. Contents of a delta link response](#104-contents-of-a-delta-link-response)
    - [10.5. Using a delta link](#105-using-a-delta-link)
- [11. JSON standardizations](#11-json-standardizations)
    - [11.1. JSON formatting standardization for primitive types](#111-json-formatting-standardization-for-primitive-types)
    - [11.2. Guidelines for dates and times](#112-guidelines-for-dates-and-times)
        - [11.2.1. Producing dates](#1121-producing-dates)
        - [11.2.2. Consuming dates](#1122-consuming-dates)
        - [11.2.3. Compatibility](#1123-compatibility)
    - [11.3. JSON serialization of dates and times](#113-json-serialization-of-dates-and-times)
        - [11.3.1. The `DateLiteral` format](#1131-the-dateliteral-format)
        - [11.3.2. Commentary on date formatting](#1132-commentary-on-date-formatting)
    - [11.4. Durations](#114-durations)
    - [11.5. Intervals](#115-intervals)
    - [11.6. Repeating intervals](#116-repeating-intervals)
- [12. Versioning](#12-versioning)
    - [12.1. Versioning formats](#121-versioning-formats)
        - [12.1.1. Group versioning](#1211-group-versioning)
    - [12.2. When to version](#122-when-to-version)
    - [12.3. Definition of a breaking change](#123-definition-of-a-breaking-change)
- [13. Long running operations](#13-long-running-operations)
    - [13.1. Resource based long running operations (RELO)](#131-resource-based-long-running-operations-relo)
    - [13.2. Stepwise long running operations](#132-stepwise-long-running-operations)
        - [13.2.1. PUT](#1321-put)
        - [13.2.2. POST](#1322-post)
        - [13.2.3. POST, hybrid model](#1323-post-hybrid-model)
        - [13.2.4. Operations resource](#1324-operations-resource)
        - [13.2.5. Operation resource](#1325-operation-resource)
        - [13.2.6. Operation tombstones](#1326-operation-tombstones)
        - [13.2.7. The typical flow, polling](#1327-the-typical-flow-polling)
        - [13.2.8. The typical flow, push notifications](#1328-the-typical-flow-push-notifications)
        - [13.2.9. Retry-After](#1329-retry-after)
    - [13.3. Retention policy for operation results](#133-retention-policy-for-operation-results)
- [14. Throttling, Quotas, and Limits](#14-throttling-quotas-and-limits)
    - [14.1. Principles](#141-principles)
    - [14.2. Return Codes (429 vs 503)](#142-return-codes-429-vs-503)
    - [14.3. Retry-After and RateLimit Headers](#143-retry-after-and-ratelimit-headers)
    - [14.4. Service Guidance](#144-service-guidance)
        - [14.4.1. Responsiveness](#1441-responsiveness)
        - [14.4.2. Rate Limits and Quotas](#1442-rate-limits-and-quotas)
        - [14.4.3. Overloaded services](#1443-overloaded-services)
        - [14.4.4. Example Response](#1444-example-response)
    - [14.5. Caller Guidance](#145-caller-guidance)
    - [14.6. Handling callers that ignore Retry-After headers](#146-handling-callers-that-ignore-retry-after-headers)
- [15. Push notifications via webhooks](#15-push-notifications-via-webhooks)
    - [15.1. Scope](#151-scope)
    - [15.2. Principles](#152-principles)
    - [15.3. Types of subscriptions](#153-types-of-subscriptions)
    - [15.4. Call sequences](#154-call-sequences)
    - [15.5. Verifying subscriptions](#155-verifying-subscriptions)
    - [15.6. Receiving notifications](#156-receiving-notifications)
        - [15.6.1. Notification payload](#1561-notification-payload)
    - [15.7. Managing subscriptions programmatically](#157-managing-subscriptions-programmatically)
        - [15.7.1. Creating subscriptions](#1571-creating-subscriptions)
        - [15.7.2. Updating subscriptions](#1572-updating-subscriptions)
        - [15.7.3. Deleting subscriptions](#1573-deleting-subscriptions)
        - [15.7.4. Enumerating subscriptions](#1574-enumerating-subscriptions)
    - [15.8. Security](#158-security)
- [16. Unsupported requests](#16-unsupported-requests)
    - [16.1. Essential guidance](#161-essential-guidance)
    - [16.2. Feature allow list](#162-feature-allow-list)
        - [16.2.1. Error response](#1621-error-response)
- [17. Naming guidelines](#17-naming-guidelines)
    - [17.1. Approach](#171-approach)
    - [17.2. Casing](#172-casing)
    - [17.3. Names to avoid](#173-names-to-avoid)
    - [17.4. Forming compound names](#174-forming-compound-names)
    - [17.5. Identity properties](#175-identity-properties)
    - [17.6. Date and time properties](#176-date-and-time-properties)
    - [17.7. Name properties](#177-name-properties)
    - [17.8. Collections and counts](#178-collections-and-counts)
    - [17.9. Common property names](#179-common-property-names)
- [18. Appendix](#18-appendix)
    - [18.1. Sequence diagram notes](#181-sequence-diagram-notes)
        - [18.1.1. Push notifications, per user flow](#1811-push-notifications-per-user-flow)
        - [18.1.2. Push notifications, firehose flow](#1812-push-notifications-firehose-flow)

<!-- /TOC -->

## 3. 介绍
开发人员通过HTTP接口访问大多数Microsoft Cloud Platform资源。尽管每种服务通常都提供特定于语言的框架来包装其API，但其所有操作最终都归结为HTTP请求。Microsoft必须支持广泛的客户端和服务，并且不能依赖可用于每个开发环境的丰富框架。因此，这些准则的目标是确保任何具有基本HTTP支持的客户端都可以轻松，一致地使用Microsoft REST API。

为了为开发人员提供尽可能流畅的体验，使这些API遵循一致的设计准则非常重要，因此使它们使用起来既简单又直观。本文档建立了Microsoft REST API开发人员遵循的准则，以一致地开发此类API。

一致性的好处也从总体上产生； 一致性使团队可以利用通用代码，模式，文档和设计决策。

这些准则旨在实现以下目标:
- 为Microsoft的所有API端点定义一致的做法和模式。
- 尽可能遵守整个行业中公认的REST / HTTP最佳实践。 [\*]
- 使所有应用程序开发人员都可以通过REST界面轻松访问Microsoft Services。
- 允许服务开发人员利用其他服务的先前工作来实施，测试和记录一致定义的REST端点。
- 允许合作伙伴（例如，非Microsoft实体）使用这些准则进行自己的REST端点设计。

[\*] Note: 该准则旨在与符合REST体系结构样式的建筑服务保持一致，尽管它们并未解决或要求遵循REST约束的建筑服务。
在整个文档中，术语“ REST”用于表示符合REST精神的服务，而不是本书所遵循的REST。*

### 3.1. 推荐阅读
为了开发良好的基于HTTP的服务，建议理解REST体系结构风格的原理。如果您不熟悉RESTful设计，这里有一些不错的资源：

[REST on Wikipedia][rest-on-wikipedia] -- Overview of common definitions and core ideas behind REST.

[REST Dissertation][fielding] -- The chapter on REST in Roy Fielding's dissertation on Network Architecture, "Architectural Styles and the Design of Network-based Software Architectures"

[RFC 7231][rfc-7231] -- Defines the specification for HTTP/1.1 semantics, and is considered the authoritative resource.

[REST in Practice][rest-in-practice] -- Book on the fundamentals of REST.

## 4. Interpreting the guidelines
### 4.1. Application of the guidelines
These guidelines are applicable to any REST API exposed publicly by Microsoft or any partner service.
Private or internal APIs SHOULD also try to follow these guidelines because internal services tend to eventually be exposed publicly.
 Consistency is valuable to not only external customers but also internal service consumers, and these guidelines offer best practices useful for any service.

There are legitimate reasons for exemption from these guidelines.
Obviously, a REST service that implements or must interoperate with some externally defined REST API must be compatible with that API and not necessarily these guidelines.
Some services MAY also have special performance needs that require a different format, such as a binary protocol.

### 4.2. Guidelines for existing services and versioning of services
We do not recommend making a breaking change to a service that predates these guidelines simply for the sake of compliance.
The service SHOULD try to become compliant at the next version release when compatibility is being broken anyway.
When a service adds a new API, that API SHOULD be consistent with the other APIs of the same version.
So if a service was written against version 1.0 of the guidelines, new APIs added incrementally to the service SHOULD also follow version 1.0. The service can then upgrade to align with the latest version of the guidelines at the service's next major release.

### 4.3. Requirements language
The keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

### 4.4. License

This work is licensed under the Creative Commons Attribution 4.0 International License.
To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/ or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.

## 5. Taxonomy
As part of onboarding to Microsoft REST API Guidelines, services MUST comply with the taxonomy defined below.

### 5.1. Errors
Errors, or more specifically Service Errors, are defined as a client passing invalid data to the service and the service _correctly_  rejecting that data.
Examples include invalid credentials, incorrect parameters, unknown version IDs, or similar.
These are generally "4xx" HTTP error codes and are the result of a client passing incorrect or invalid data.

Errors do _not_ contribute to overall API availability.

### 5.2. Faults
Faults, or more specifically Service Faults, are defined as the service failing to correctly return in response to a valid client request.
These are generally "5xx" HTTP error codes.

Faults _do_ contribute to the overall API availability.

Calls that fail due to rate limiting or quota failures MUST NOT count as faults.
Calls that fail as the result of a service fast-failing requests (often for its own protection) do count as faults.

### 5.3. Latency
Latency is defined as how long a particular API call takes to complete, measured as closely to the client as possible.
This metric applies to both synchronous and asynchronous APIs in the same way.
For long running calls, the latency is measured on the initial request and measures how long that call (not the overall operation) takes to complete.

### 5.4. Time to complete
Services that expose long operations MUST track "Time to Complete" metrics around those operations.

### 5.5. Long running API faults
For a Long Running API, it's possible for both the initial request which begins the operation and the request which retrieves the results to technically work (each passing back a 200) but for the underlying operation to have failed.
Long Running faults MUST roll up as faults into the overall Availability metrics.

## 6. Client guidance
To ensure the best possible experience for clients talking to a REST service, clients SHOULD adhere to the following best practices:

### 6.1. Ignore rule
For loosely coupled clients where the exact shape of the data is not known before the call, if the server returns something the client wasn't expecting, the client MUST safely ignore it.
  
Some services MAY add fields to responses without changing versions numbers.
Services that do so MUST make this clear in their documentation and clients MUST ignore unknown fields.

### 6.2. Variable order rule
Clients MUST NOT rely on the order in which data appears in JSON service responses.
For example, clients SHOULD be resilient to the reordering of fields within a JSON object.
When supported by the service, clients MAY request that data be returned in a specific order.
For example, services MAY support the use of the _$orderBy_ querystring parameter to specify the order of elements within a JSON array.
Services MAY also explicitly specify the ordering of some elements as part of the service contract.
For example, a service MAY always return a JSON object's "type" information as the first field in an object to simplify response parsing on the client.
Clients MAY rely on ordering behavior explicitly identified by the service.

### 6.3. Silent fail rule
Clients requesting OPTIONAL server functionality (such as optional headers) MUST be resilient to the server ignoring that particular functionality.

## 7. Consistency fundamentals
### 7.1. URL structure
Humans SHOULD be able to easily read and construct URLs.

This facilitates discovery and eases adoption on platforms without a well-supported client library.

An example of a well-structured URL is:

```
https://api.contoso.com/v1.0/people/jdoe@contoso.com/inbox
```

An example URL that is not friendly is:

```
https://api.contoso.com/EWS/OData/Users('jdoe@microsoft.com')/Folders('AAMkADdiYzI1MjUzLTk4MjQtNDQ1Yy05YjJkLWNlMzMzYmIzNTY0MwAuAAAAAACzMsPHYH6HQoSwfdpDx-2bAQCXhUk6PC1dS7AERFluCgBfAAABo58UAAA=')
```

A frequent pattern that comes up is the use of URLs as values.
Services MAY use URLs as values.
For example, the following is acceptable:

```
https://api.contoso.com/v1.0/items?url=https://resources.contoso.com/shoes/fancy
```

### 7.2. URL length
The HTTP 1.1 message format, defined in RFC 7230, in section [3.1.1][rfc-7230-3-1-1], defines no length limit on the Request Line, which includes the target URL.
From the RFC:

> HTTP does not place a predefined limit on the length of a
   request-line. [...] A server that receives a request-target longer than any URI it wishes to parse MUST respond
   with a 414 (URI Too Long) status code.

Services that can generate URLs longer than 2,083 characters MUST make accommodations for the clients they wish to support.
Here are some sources for determining what target clients support:

 * [http://stackoverflow.com/a/417184](http://stackoverflow.com/a/417184)
 * [https://blogs.msdn.microsoft.com/ieinternals/2014/08/13/url-length-limits/](https://blogs.msdn.microsoft.com/ieinternals/2014/08/13/url-length-limits/)
 
Also note that some technology stacks have hard and adjustable URL limits, so keep this in mind as you design your services.

### 7.3. Canonical identifier
In addition to friendly URLs, resources that can be moved or be renamed SHOULD expose a URL that contains a unique stable identifier.
It MAY be necessary to interact with the service to obtain a stable URL from the friendly name for the resource, as in the case of the "/my" shortcut used by some services.

The stable identifier is not required to be a GUID.

An example of a URL containing a canonical identifier is:

```
https://api.contoso.com/v1.0/people/7011042402/inbox
```

### 7.4. Supported methods
Operations MUST use the proper HTTP methods whenever possible, and operation idempotency MUST be respected.
HTTP methods are frequently referred to as the HTTP verbs.
The terms are synonymous in this context, however the HTTP specification uses the term method.

Below is a list of methods that Microsoft REST services SHOULD support.
Not all resources will support all methods, but all resources using the methods below MUST conform to their usage.

Method  | Description                                                                                                                | Is Idempotent
------- | -------------------------------------------------------------------------------------------------------------------------- | -------------
GET     | Return the current value of an object                                                                                      | True
PUT     | Replace an object, or create a named object, when applicable                                                               | True
DELETE  | Delete an object                                                                                                           | True
POST    | Create a new object based on the data provided, or submit a command                                                        | False
HEAD    | Return metadata of an object for a GET response. Resources that support the GET method MAY support the HEAD method as well | True
PATCH   | Apply a partial update to an object                                                                                        | False
OPTIONS | Get information about a request; see below for details.                                                                    | True

<small>Table 1</small>

#### 7.4.1. POST
POST operations SHOULD support the Location response header to specify the location of any created resource that was not explicitly named, via the Location header.

As an example, imagine a service that allows creation of hosted servers, which will be named by the service:

```http
POST http://api.contoso.com/account1/servers
```

The response would be something like:

```http
201 Created
Location: http://api.contoso.com/account1/servers/server321
```

Where "server321" is the service-allocated server name.

Services MAY also return the full metadata for the created item in the response.

#### 7.4.2. PATCH
PATCH has been standardized by IETF as the method to be used for updating an existing object incrementally (see [RFC 5789][rfc-5789]).
Microsoft REST API Guidelines compliant APIs SHOULD support PATCH.

#### 7.4.3. Creating resources via PATCH (UPSERT semantics)
Services that allow callers to specify key values on create SHOULD support UPSERT semantics, and those that do MUST support creating resources using PATCH.
Because PUT is defined as a complete replacement of the content, it is dangerous for clients to use PUT to modify data.
Clients that do not understand (and hence ignore) properties on a resource are not likely to provide them on a PUT when trying to update a resource, hence such properties could be inadvertently removed.
Services MAY optionally support PUT to update existing resources, but if they do they MUST use replacement semantics (that is, after the PUT, the resource's properties MUST match what was provided in the request, including deleting any server properties that were not provided).

Under UPSERT semantics, a PATCH call to a nonexistent resource is handled by the server as a "create," and a PATCH call to an existing resource is handled as an "update." To ensure that an update request is not treated as a create or vice versa, the client MAY specify precondition HTTP headers in the request.
The service MUST NOT treat a PATCH request as an insert if it contains an If-Match header and MUST NOT treat a PATCH request as an update if it contains an If-None-Match header with a value of "*".

If a service does not support UPSERT, then a PATCH call against a resource that does not exist MUST result in an HTTP "409 Conflict" error.

#### 7.4.4. Options and link headers
OPTIONS allows a client to retrieve information about a resource, at a minimum by returning the Allow header denoting the valid methods for this resource.

In addition, services SHOULD include a Link header (see [RFC 5988][rfc-5988]) to point to documentation for the resource in question:

```http
Link: <{help}>; rel="help"
```

Where {help} is the URL to a documentation resource.

For examples on use of OPTIONS, see [preflighting CORS cross-domain calls][cors-preflight].

### 7.5. Standard request headers
The table of request headers below SHOULD be used by Microsoft REST API Guidelines services.
Using these headers is not mandated, but if used they MUST be used consistently.

All header values MUST follow the syntax rules set forth in the specification where the header field is defined.
Many HTTP headers are defined in [RFC7231][rfc-7231], however a complete list of approved headers can be found in the [IANA Header Registry][IANA-headers]."

Header                            | Type                                  | Description
--------------------------------- | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Authorization                     | String                                           | Authorization header for the request
Date                              | Date                                             | Timestamp of the request, based on the client's clock, in [RFC 5322][rfc-5322-3-3] date and time format.  The server SHOULD NOT make any assumptions about the accuracy of the client's clock.  This header MAY be included in the request, but MUST be in this format when supplied.  Greenwich Mean Time (GMT) MUST be used as the time zone reference for this header when it is provided.  For example: `Wed, 24 Aug 2016 18:41:30 GMT`.  Note that GMT is exactly equal to UTC (Coordinated Universal Time) for this purpose.
Accept                            | Content type                                     | The requested content type for the response such as: <ul><li>application/xml</li><li>text/xml</li><li>application/json</li><li>text/javascript (for JSONP)</li></ul>Per the HTTP guidelines, this is just a hint and responses MAY have a different content type, such as a blob fetch where a successful response will just be the blob stream as the payload. For services following OData, the preference order specified in OData SHOULD be followed.
Accept-Encoding                   | Gzip, deflate                                    | REST endpoints SHOULD support GZIP and DEFLATE encoding, when applicable. For very large resources, services MAY ignore and return uncompressed data.
Accept-Language                   | "en", "es", etc.                                 | Specifies the preferred language for the response. Services are not required to support this, but if a service supports localization it MUST do so through the Accept-Language header.
Accept-Charset                    | Charset type like "UTF-8"                        | Default is UTF-8, but services SHOULD be able to handle ISO-8859-1.
Content-Type                      | Content type                                     | Mime type of request body (PUT/POST/PATCH)
Prefer                            | return=minimal, return=representation            | If the return=minimal preference is specified, services SHOULD return an empty body in response to a successful insert or update. If return=representation is specified, services SHOULD return the created or updated resource in the response. Services SHOULD support this header if they have scenarios where clients would sometimes benefit from responses, but sometimes the response would impose too much of a hit on bandwidth.
If-Match, If-None-Match, If-Range | String                                           | Services that support updates to resources using optimistic concurrency control MUST support the If-Match header to do so. Services MAY also use other headers related to ETags as long as they follow the HTTP specification.

### 7.6. Standard response headers
Services SHOULD return the following response headers, except where noted in the "required" column.

Response Header    | Required                                      | Description
------------------ | --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Date               | All responses                                 | Timestamp the response was processed, based on the server's clock, in [RFC 5322][rfc-5322-3-3] date and time format.  This header MUST be included in the response.  Greenwich Mean Time (GMT) MUST be used as the time zone reference for this header.  For example: `Wed, 24 Aug 2016 18:41:30 GMT`. Note that GMT is exactly equal to UTC (Coordinated Universal Time) for this purpose.
Content-Type       | All responses                                 | The content type
Content-Encoding   | All responses                                 | GZIP or DEFLATE, as appropriate
Preference-Applied | When specified in request                     | Whether a preference indicated in the Prefer request header was applied
ETag               | When the requested resource has an entity tag | The ETag response-header field provides the current value of the entity tag for the requested variant. Used with If-Match, If-None-Match and If-Range to implement optimistic concurrency control.

### 7.7. Custom headers
Custom headers MUST NOT be required for the basic operation of a given API.

Some of the guidelines in this document prescribe the use of nonstandard HTTP headers.
In addition, some services MAY need to add extra functionality, which is exposed via HTTP headers.
The following guidelines help maintain consistency across usage of custom headers.

Headers that are not standard HTTP headers MUST have one of two formats:

1. A generic format for headers that are registered as "provisional" with IANA ([RFC 3864][rfc-3864])
2. A scoped format for headers that are too usage-specific for registration

These two formats are described below.

### 7.8. Specifying headers as query parameters
Some headers pose challenges for some scenarios such as AJAX clients, especially when making cross-domain calls where adding headers MAY not be supported.
As such, some headers MAY be accepted as Query Parameters in addition to headers, with the same naming as the header:

Not all headers make sense as query parameters, including most standard HTTP headers.

The criteria for considering when to accept headers as parameters are:

1. Any custom headers MUST be also accepted as parameters.
2. Required standard headers MAY be accepted as parameters.
3. Required headers with security sensitivity (e.g., Authorization header) MIGHT NOT be appropriate as parameters; the service owner SHOULD evaluate these on a case-by-case basis.

The one exception to this rule is the Accept header.
It's common practice to use a scheme with simple names instead of the full functionality described in the HTTP specification for Accept.

### 7.9. PII parameters
Consistent with their organization's privacy policy, clients SHOULD NOT transmit personally identifiable information (PII) parameters in the URL (as part of path or query string) because this information can be inadvertently exposed via client, network, and server logs and other mechanisms.

Consequently, a service SHOULD accept PII parameters transmitted as headers.

However, there are many scenarios where the above recommendations cannot be followed due to client or software limitations.
To address these limitations, services SHOULD also accept these PII parameters as part of the URL consistent with the rest of these guidelines.

Services that accept PII parameters -- whether in the URL or as headers -- SHOULD be compliant with privacy policy specified by their organization's engineering leadership.
This will typically include recommending that clients prefer headers for transmission and implementations adhere to special precautions to ensure that logs and other service data collection are properly handled.

### 7.10. Response formats
For organizations to have a successful platform, they must serve data in formats developers are accustomed to using, and in consistent ways that allow developers to handle responses with common code.

Web-based communication, especially when a mobile or other low-bandwidth client is involved, has moved quickly in the direction of JSON for a variety of reasons, including its tendency to be lighter weight and its ease of consumption with JavaScript-based clients.

JSON property names SHOULD be camelCased.

Services SHOULD provide JSON as the default encoding.

#### 7.10.1. Clients-specified response format
In HTTP, response format SHOULD be requested by the client using the Accept header.
This is a hint, and the server MAY ignore it if it chooses to, even if this isn't typical of well-behaved servers.
Clients MAY send multiple Accept headers and the service MAY choose one of them.

The default response format (no Accept header provided) SHOULD be application/json, and all services MUST support application/json.

Accept Header    | Response type                      | Notes
---------------- | ---------------------------------- | -------------------------------------------
application/json | Payload SHOULD be returned as JSON | Also accept text/javascript for JSONP cases

```http
GET https://api.contoso.com/v1.0/products/user
Accept: application/json
```

#### 7.10.2. Error condition responses
For non-success conditions, developers SHOULD be able to write one piece of code that handles errors consistently across different Microsoft REST API Guidelines services.
This allows building of simple and reliable infrastructure to handle exceptions as a separate flow from successful responses.
The following is based on the OData v4 JSON spec.
However, it is very generic and does not require specific OData constructs.
APIs SHOULD use this format even if they are not using other OData constructs.

The error response MUST be a single JSON object.
This object MUST have a name/value pair named "error." The value MUST be a JSON object.

This object MUST contain name/value pairs with the names "code" and "message," and it MAY contain name/value pairs with the names "target," "details" and "innererror."

The value for the "code" name/value pair is a language-independent string.
Its value is a service-defined error code that SHOULD be human-readable.
This code serves as a more specific indicator of the error than the HTTP error code specified in the response.
Services SHOULD have a relatively small number (about 20) of possible values for "code," and all clients MUST be capable of handling all of them.
Most services will require a much larger number of more specific error codes, which are not interesting to all clients.
These error codes SHOULD be exposed in the "innererror" name/value pair as described below.
Introducing a new value for "code" that is visible to existing clients is a breaking change and requires a version increase.
Services can avoid breaking changes by adding new error codes to "innererror" instead.

The value for the "message" name/value pair MUST be a human-readable representation of the error.
It is intended as an aid to developers and is not suitable for exposure to end users.
Services wanting to expose a suitable message for end users MUST do so through an [annotation][odata-json-annotations] or custom property.
Services SHOULD NOT localize "message" for the end user, because doing so might make the value unreadable to the app developer who may be logging the value, as well as make the value less searchable on the Internet.

The value for the "target" name/value pair is the target of the particular error (e.g., the name of the property in error).

The value for the "details" name/value pair MUST be an array of JSON objects that MUST contain name/value pairs for "code" and "message," and MAY contain a name/value pair for "target," as described above.
The objects in the "details" array usually represent distinct, related errors that occurred during the request.
See example below.

The value for the "innererror" name/value pair MUST be an object.
The contents of this object are service-defined.
Services wanting to return more specific errors than the root-level code MUST do so by including a name/value pair for "code" and a nested "innererror." Each nested "innererror" object represents a higher level of detail than its parent.
When evaluating errors, clients MUST traverse through all of the nested "innererrors" and choose the deepest one that they understand.
This scheme allows services to introduce new error codes anywhere in the hierarchy without breaking backwards compatibility, so long as old error codes still appear.
The service MAY return different levels of depth and detail to different callers.
For example, in development environments, the deepest "innererror" MAY contain internal information that can help debug the service.
To guard against potential security concerns around information disclosure, services SHOULD take care not to expose too much detail unintentionally.
Error objects MAY also include custom server-defined name/value pairs that MAY be specific to the code.
Error types with custom server-defined properties SHOULD be declared in the service's metadata document.
See example below.

Error responses MAY contain [annotations][odata-json-annotations] in any of their JSON objects.

We recommend that for any transient errors that may be retried, services SHOULD include a Retry-After HTTP header indicating the minimum number of seconds that clients SHOULD wait before attempting the operation again.

##### ErrorResponse : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`error` | Error | ✔ | The error object.

##### Error : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`code` | String | ✔ | One of a server-defined set of error codes.
`message` | String | ✔ | A human-readable representation of the error.
`target` | String |  | The target of the error.
`details` | Error[] |  | An array of details about specific errors that led to this reported error.
`innererror` | InnerError |  | An object containing more specific information than the current object about the error.

##### InnerError : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`code` | String |  | A more specific error code than was provided by the containing error.
`innererror` | InnerError |  | An object containing more specific information than the current object about the error.

##### Examples

Example of "innererror":

```json
{
  "error": {
    "code": "BadArgument",
    "message": "Previous passwords may not be reused",
    "target": "password",
    "innererror": {
      "code": "PasswordError",
      "innererror": {
        "code": "PasswordDoesNotMeetPolicy",
        "minLength": "6",
        "maxLength": "64",
        "characterTypes": ["lowerCase","upperCase","number","symbol"],
        "minDistinctCharacterTypes": "2",
        "innererror": {
          "code": "PasswordReuseNotAllowed"
        }
      }
    }
  }
}
```

In this example, the most basic error code is "BadArgument," but for clients that are interested, there are more specific error codes in "innererror."
The "PasswordReuseNotAllowed" code may have been added by the service at a later date, having previously only returned "PasswordDoesNotMeetPolicy."
Existing clients do not break when the new error code is added, but new clients MAY take advantage of it.
The "PasswordDoesNotMeetPolicy" error also includes additional name/value pairs that allow the client to determine the server's configuration, validate the user's input programmatically, or present the server's constraints to the user within the client's own localized messaging.

Example of "details":

```json
{
  "error": {
    "code": "BadArgument",
    "message": "Multiple errors in ContactInfo data",
    "target": "ContactInfo",
    "details": [
      {
        "code": "NullValue",
        "target": "PhoneNumber",
        "message": "Phone number must not be null"
      },
      {
        "code": "NullValue",
        "target": "LastName",
        "message": "Last name must not be null"
      },
      {
        "code": "MalformedValue",
        "target": "Address",
        "message": "Address is not valid"
      }
    ]
  }
}
```

In this example there were multiple problems with the request, with each individual error listed in "details."

### 7.11. HTTP Status Codes
Standard HTTP Status Codes SHOULD be used; see the HTTP Status Code definitions for more information.

### 7.12. Client library optional
Developers MUST be able to develop on a wide variety of platforms and languages, such as Windows, macOS, Linux, C#, Python, Node.js, and Ruby.

Services SHOULD be able to be accessed from simple HTTP tools such as curl without significant effort.

Service developer portals SHOULD provide the equivalent of "Get Developer Token" to facilitate experimentation and curl support.

## 8. CORS
Services compliant with the Microsoft REST API Guidelines MUST support [CORS (Cross Origin Resource Sharing)][cors].
Services SHOULD support an allowed origin of CORS * and enforce authorization through valid OAuth tokens.
Services SHOULD NOT support user credentials with origin validation.
There MAY be exceptions for special cases.

### 8.1. Client guidance
Web developers usually don't need to do anything special to take advantage of CORS.
All of the handshake steps happen invisibly as part of the standard XMLHttpRequest calls they make.

Many other platforms, such as .NET, have integrated support for CORS.

#### 8.1.1. Avoiding preflight
Because the CORS protocol can trigger preflight requests that add additional round trips to the server, performance-critical apps might be interested in avoiding them.
The spirit behind CORS is to avoid preflight for any simple cross-domain requests that old non-CORS-capable browsers were able to make.
All other requests require preflight.

A request is "simple" and avoids preflight if its method is GET, HEAD or POST, and if it doesn't contain any request headers besides Accept, Accept-Language and Content-Language.
For POST requests, the Content-Type header is also allowed, but only if its value is "application/x-www-form-urlencoded," "multipart/form-data" or "text/plain."
For any other headers or values, a preflight request will happen.

### 8.2. Service guidance
 At minimum, services MUST:
- Understand the Origin request header that browsers send on cross-domain requests, and the Access-Control-Request-Method request header that they send on preflight OPTIONS requests that check for access.
- If the Origin header is present in a request:
  - If the request uses the OPTIONS method and contains the Access-Control-Request-Method header, then it is a preflight request intended to probe for access before the actual request. Otherwise, it is an actual request. For preflight requests, beyond performing the steps below to add headers, services MUST perform no additional processing and MUST return a 200 OK. For non-preflight requests, the headers below are added in addition to the request's regular processing.
  - Add an Access-Control-Allow-Origin header to the response, containing the same value as the Origin request header. Note that this requires services to dynamically generate the header value. Resources that do not require cookies or any other form of [user credentials][cors-user-credentials] MAY respond with a wildcard asterisk (*) instead. Note that the wildcard is acceptable here only, and not for any of the other headers described below.
  - If the caller requires access to a response header that is not in the set of [simple response headers][cors-simple-headers] (Cache-Control, Content-Language, Content-Type, Expires, Last-Modified, Pragma), then add an Access-Control-Expose-Headers header containing the list of additional response header names the client should have access to.
  - If the request requires cookies, then add an Access-Control-Allow-Credentials header set to "true."
  - If the request was a preflight request (see first bullet), then the service MUST:
    - Add an Access-Control-Allow-Headers response header containing the list of request header names the client is permitted to use. This list need only contain headers that are not in the set of [simple request headers][cors-simple-headers] (Accept, Accept-Language, Content-Language). If there are no restrictions on headers the service accepts, the service MAY simply return the same value as the Access-Control-Request-Headers header sent by the client.
    - Add an Access-Control-Allow-Methods response header containing the list of HTTP methods the caller is permitted to use.

Add an Access-Control-Max-Age pref response header containing the number of seconds for which this preflight response is valid (and hence can be avoided before subsequent actual requests). Note that while it is customary to use a large value like 2592000 (30 days), many browsers self-impose a much lower limit (e.g., five minutes).

Because browser preflight response caches are notoriously weak, the additional round trip from a preflight response hurts performance.
Services used by interactive Web clients where performance is critical SHOULD avoid patterns that cause a preflight request
- For GET and HEAD calls, avoid requiring request headers that are not part of the simple set above. Allow them to be provided as query parameters instead.
  - The Authorization header is not part of the simple set, so the authentication token MUST be sent through the "access_token" query parameter instead, for resources requiring authentication. Note that passing authentication tokens in the URL is not recommended, because it can lead to the token getting recorded in server logs and exposed to anyone with access to those logs. Services that accept authentication tokens through the URL MUST take steps to mitigate the security risks, such as using short-lived authentication tokens, suppressing the auth token from getting logged, and controlling access to server logs.

- Avoid requiring cookies. XmlHttpRequest will only send cookies on cross-domain requests if the "withCredentials" attribute is set; this also causes a preflight request.
  - Services that require cookie-based authentication MUST use a "dynamic canary" to secure all APIs that accept cookies.

- For POST calls, prefer simple Content-Types in the set of ("application/x-www-form-urlencoded," "multipart/form-data," "text/plain") where applicable. Any other Content-Type will induce a preflight request.
  - Services MUST NOT contravene other API recommendations in the name of avoiding CORS preflight requests. In particular, in accordance with recommendations, most POST requests will actually require a preflight request due to the Content-Type.
  - If eliminating preflight is critical, then a service MAY support alternative mechanisms for data transfer, but the RECOMMENDED approach MUST also be supported.

In addition, when appropriate services MAY support the JSONP pattern for simple, GET-only cross-domain access.
In JSONP, services take a parameter indicating the format (_$format=json_) and a parameter indicating a callback (_$callback=someFunc_), and return a text/javascript document containing the JSON response wrapped in a function call with the indicated name.
More on JSONP at Wikipedia: [JSONP](https://en.wikipedia.org/wiki/JSONP).

## 9. Collections
### 9.1. Item keys
Services MAY support durable identifiers for each item in the collection, and that identifier SHOULD be represented in JSON as "id". These durable identifiers are often used as item keys.

Collections that support durable identifiers MAY support delta queries.

### 9.2. Serialization
Collections are represented in JSON using standard array notation.

### 9.3. Collection URL patterns
Collections are located directly under the service root when they are top level, or as a segment under another resource when scoped to that resource.

For example:

```http
GET https://api.contoso.com/v1.0/people
```

Whenever possible, services MUST support the "/" pattern.
For example:

```http
GET https://{serviceRoot}/{collection}/{id}
```

Where:
- {serviceRoot} – the combination of host (site URL) + the root path to the service
- {collection} – the name of the collection, unabbreviated, pluralized
- {id} – the value of the unique id property. When using the "/" pattern this MUST be the raw string/number/guid value with no quoting but properly escaped to fit in a URL segment.

#### 9.3.1. Nested collections and properties
Collection items MAY contain other collections.
For example, a user collection MAY contain user resources that have multiple addresses:

```http
GET https://api.contoso.com/v1.0/people/123/addresses
```

```json
{
  "value": [
    { "street": "1st Avenue", "city": "Seattle" },
    { "street": "124th Ave NE", "city": "Redmond" }
  ]
}
```

### 9.4. Big collections
As data grows, so do collections.
Planning for pagination is important for all services.
Therefore, when multiple pages are available, the serialization payload MUST contain the opaque URL for the next page as appropriate.
Refer to the paging guidance for more details.

Clients MUST be resilient to collection data being either paged or nonpaged for any given request.

```json
{
  "value":[
    { "id": "Item 1","price": 99.95,"sizes": null},
    { … },
    { … },
    { "id": "Item 99","price": 59.99,"sizes": null}
  ],
  "@nextLink": "{opaqueUrl}"
}
```

### 9.5. Changing collections
POST requests are not idempotent.
This means that two POST requests sent to a collection resource with exactly the same payload MAY lead to multiple items being created in that collection.
This is often the case for insert operations on items with a server-side generated id.

For example, the following request:

```http
POST https://api.contoso.com/v1.0/people
```

Would lead to a response indicating the location of the new collection item:

```http
201 Created
Location: https://api.contoso.com/v1.0/people/123
```

And once executed again, would likely lead to another resource:

```http
201 Created
Location: https://api.contoso.com/v1.0/people/124
```

While a PUT request would require the indication of the collection item with the corresponding key instead:

```http
PUT https://api.contoso.com/v1.0/people/123
```

### 9.6. Sorting collections
The results of a collection query MAY be sorted based on property values.
The property is determined by the value of the _$orderBy_ query parameter.

The value of the _$orderBy_ parameter contains a comma-separated list of expressions used to sort the items.
A special case of such an expression is a property path terminating on a primitive property.

The expression MAY include the suffix "asc" for ascending or "desc" for descending, separated from the property name by one or more spaces.
If "asc" or "desc" is not specified, the service MUST order by the specified property in ascending order.

NULL values MUST sort as "less than" non-NULL values.

Items MUST be sorted by the result values of the first expression, and then items with the same value for the first expression are sorted by the result value of the second expression, and so on.
The sort order is the inherent order for the type of the property.

For example:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name
```

Will return all people sorted by name in ascending order.

For example:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name desc
```

Will return all people sorted by name in descending order.

Sub-sorts can be specified by a comma-separated list of property names with OPTIONAL direction qualifier.

For example:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name desc,hireDate
```

Will return all people sorted by name in descending order and a secondary sort order of hireDate in ascending order.

Sorting MUST compose with filtering such that:

```http
GET https://api.contoso.com/v1.0/people?$filter=name eq 'david'&$orderBy=hireDate
```

Will return all people whose name is David sorted in ascending order by hireDate.

#### 9.6.1. Interpreting a sorting expression
Sorting parameters MUST be consistent across pages, as both client and server-side paging is fully compatible with sorting.

If a service does not support sorting by a property named in a _$orderBy_ expression, the service MUST respond with an error message as defined in the Responding to Unsupported Requests section.

### 9.7. Filtering
The _$filter_ querystring parameter allows clients to filter a collection of resources that are addressed by a request URL.
The expression specified with _$filter_ is evaluated for each resource in the collection, and only items where the expression evaluates to true are included in the response.
Resources for which the expression evaluates to false or to null, or which reference properties that are unavailable due to permissions, are omitted from the response.

Example: return all Products whose Price is less than $10.00

```http
GET https://api.contoso.com/v1.0/products?$filter=price lt 10.00
```

The value of the _$filter_ option is a Boolean expression.

#### 9.7.1. Filter operations
Services that support _$filter_ SHOULD support the following minimal set of operations.

Operator             | Description           | Example
-------------------- | --------------------- | -----------------------------------------------------
Comparison Operators |                       |
eq                   | Equal                 | city eq 'Redmond'
ne                   | Not equal             | city ne 'London'
gt                   | Greater than          | price gt 20
ge                   | Greater than or equal | price ge 10
lt                   | Less than             | price lt 20
le                   | Less than or equal    | price le 100
Logical Operators    |                       |
and                  | Logical and           | price le 200 and price gt 3.5
or                   | Logical or            | price le 3.5 or price gt 200
not                  | Logical negation      | not price le 3.5
Grouping Operators   |                       |
( )                  | Precedence grouping   | (priority eq 1 or city eq 'Redmond') and price gt 100

#### 9.7.2. Operator examples
The following examples illustrate the use and semantics of each of the logical operators.

Example: all products with a name equal to 'Milk'

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk'
```

Example: all products with a name not equal to 'Milk'

```http
GET https://api.contoso.com/v1.0/products?$filter=name ne 'Milk'
```

Example: all products with the name 'Milk' that also have a price less than 2.55:

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk' and price lt 2.55
```

Example: all products that either have the name 'Milk' or have a price less than 2.55:

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk' or price lt 2.55
```

Example: all products that have the name 'Milk' or 'Eggs' and have a price less than 2.55:

```http
GET https://api.contoso.com/v1.0/products?$filter=(name eq 'Milk' or name eq 'Eggs') and price lt 2.55
```

#### 9.7.3. Operator precedence
Services MUST use the following operator precedence for supported operators when evaluating _$filter_ expressions.
Operators are listed by category in order of precedence from highest to lowest.
Operators in the same category have equal precedence:

| Group           | Operator | Description           |
|:----------------|:---------|:----------------------|
| Grouping        | ( )      | Precedence grouping   |
| Unary           | not      | Logical Negation      |
| Relational      | gt       | Greater Than          |
|                 | ge       | Greater than or Equal |
|                 | lt       | Less Than             |
|                 | le       | Less than or Equal    |
| Equality        | eq       | Equal                 |
|                 | ne       | Not Equal             |
| Conditional AND | and      | Logical And           |
| Conditional OR  | or       | Logical Or            |

### 9.8. Pagination
RESTful APIs that return collections MAY return partial sets.
Consumers of these 
