# Controlend ([thecontrolend.com](http://thecontralend.com))

The base URL of the API is [`https://site.controlend.com/api/v1`](https://site.enchant.com/api/v1) where `site` should be replaced with your help controlend identifier.

We use standard HTTP verbs to indicate intent of a request:

- `GET` - To retrieve a resource or a collection of resources
- `POST` - To create a resource
- `PATCH` - To modify a resource
- `PUT` - To set a resource
- `DELETE` - To delete a resource

## **HTTP Status Codes**

We use HTTP status codes to indicate success or failure of a request.

Success codes:

- `200 OK` - Request succeeded. Response included
- `201 Created` - Resource created. URL to new resource in Location header
- `204 No Content` - Request succeeded, but no response body

Error codes:

- `400 Bad Request` - Could not parse request
- `401 Unauthorized` - No authentication credentials provided or authentication failed
- `403 Forbidden` - Authenticated user does not have access
- `404 Not Found` - Resource not found
- `415 Unsupported Media Type` - POST/PUT/PATCH request occurred without a `application/json` content type
- `422 Unprocessable Entry` - A request to modify or create a resource failed due to a [validation error](http://dev.enchant.com/api/v1#validation)
- `429 Too Many Requests` - Request rejected due to [rate limiting](http://dev.enchant.com/api/v1#ratelimit)
- `500, 501, 502, 503, etc` - An internal server error occured

## GRPC

Because of this and the corresponding increase in call load, we implemented gRPC as our microservices communications framework. It allows our microservices to talk to each other in a more robust and performant way.

## Let me cURL my gRPC endpoint

In addition to these tools, we managed to re-enable our existing REST tools by using Envoy and JSON transcoding. This works by sending HTTP/1.1 requests with a JSON payload to an Envoy proxy configured as a [gRPC-JSON transcoder](https://www.envoyproxy.io/docs/envoy/v1.5.0/api-v1/http_filters/grpc_json_transcoder_filter.html#config-http-filters-grpc-json-transcoder-v1). Envoy will translate the request into the corresponding gRPC call, with the response message translated back into JSON

## grpc benefits

Additionally, mobile clients, regardless of the device power in a local sense, can use gRPC to efficiently tie into external cloud systems, servers, and processes, leveraging these mechanics for co-processing to match local processing ability. Having the ability to offload data crunching while using local resources to process local, extreme low latency needs can do a lot to multiply the apparent power of said local device, and could go a long way towards making mobile devices lighter, more power efficient, yet more effective in their common functions.

Controlend hopes to become a premier end-to-end lending infrastructure platform.

We build technology that powers Airbnb’s massive daily transaction volume to collect payments from guests and distribute payouts to hosts. Our goal is to make the payment experience on Airbnb delightful, magical, and intuitive.

Our sphere has grown to include compliance (sales taxes, earnings taxes, licenses, and more) as well as reconciliation and financial accounting according to generally accepted accounting principles.

online application generator (OAG) market for Android applications.

Online application generators enable app development using wizard-like, point-and-click web interfaces in which developers only need to add and suitably interconnect UI elements that represent application components… There is no need and typically no option to write custom code. A typical OAG offers a palette of components that can be configured together to deliver app services.

[https://www.swiftic.com/](https://www.swiftic.com/) 

[https://www.appsgeyser.com/](https://www.appsgeyser.com/)

[https://github.com/velvia/links](https://github.com/velvia/links)

[https://github.com/typelevel/squants](https://github.com/typelevel/squants)

[https://stripe.com/atlas/guides/saas-pricing](https://stripe.com/atlas/guides/saas-pricing) 

[https://stripe.com/atlas/guides/business-of-saas](https://stripe.com/atlas/guides/business-of-saas)

[https://www.youtube.com/watch?v=BT9hUusNKQ8](https://www.youtube.com/watch?v=BT9hUusNKQ8)

# Accounting Softwares

[https://www.manager.io/](https://www.manager.io/)

[https://github.com/ledgersmb/LedgerSMB](https://github.com/ledgersmb/LedgerSMB)

[https://www.aellacredit.com/](https://www.aellacredit.com/) 

[https://www.mambu.com/](https://www.mambu.com/)

[https://developer.mambu.com/](https://developer.mambu.com/)

[https://finplusgroup.com/solutions/](https://finplusgroup.com/solutions/)

[https://www.microfinancegateway.org/sites/default/files/publication_files/digital_credit_survey_-_kenya_presentation_cgap_v3.pdf](https://www.microfinancegateway.org/sites/default/files/publication_files/digital_credit_survey_-_kenya_presentation_cgap_v3.pdf)

[http://fsdkenya.org/blog/kenyas-digital-credit-revolution-5-years-on/](http://fsdkenya.org/blog/kenyas-digital-credit-revolution-5-years-on/) 

Get APP APK's [https://www.apkmonk.com/](https://www.apkmonk.com/)

# Notes: Targeted for the mobile economy. Africa 2.0

[https://www.thoughtworks.com/insights/blog/intelligent-bank-part-1-hidden-weakness-financial-services-business-models](https://www.thoughtworks.com/insights/blog/intelligent-bank-part-1-hidden-weakness-financial-services-business-models) 

[https://www.thoughtworks.com/insights/blog/intelligent-bank-mindset-cars-not-faster-horses](https://www.thoughtworks.com/insights/blog/intelligent-bank-mindset-cars-not-faster-horses) 

[https://www.thoughtworks.com/insights/blog/intelligent-bank-power-platform-thinking](https://www.thoughtworks.com/insights/blog/intelligent-bank-power-platform-thinking) 

[https://www.thoughtworks.com/insights/blog/intelligent-bank-part-1-hidden-weakness-financial-services-business-models](https://www.thoughtworks.com/insights/blog/intelligent-bank-part-1-hidden-weakness-financial-services-business-models) 

[https://www.thoughtworks.com/insights/blog/precision-banking-segment-one](https://www.thoughtworks.com/insights/blog/precision-banking-segment-one) 

[https://www.thoughtworks.com/insights/blog/lost-millennialswill-our-financial-services-sector-ever-find-them](https://www.thoughtworks.com/insights/blog/lost-millennialswill-our-financial-services-sector-ever-find-them) 

[https://www.thoughtworks.com/insights/blog/intelligent-risk-and-compliance](https://www.thoughtworks.com/insights/blog/intelligent-risk-and-compliance) 

[https://www.thoughtworks.com/insights/blog/moving-center-customer-s-universe](https://www.thoughtworks.com/insights/blog/moving-center-customer-s-universe) 

[https://www.thoughtworks.com/insights/blog/platform-powered-independence-0](https://www.thoughtworks.com/insights/blog/platform-powered-independence-0)

[https://www.thoughtworks.com/insights/blog/connected-things-not-devices](https://www.thoughtworks.com/insights/blog/connected-things-not-devices) 

[https://www.thoughtworks.com/insights/blog/iot-testing-atlas](https://www.thoughtworks.com/insights/blog/iot-testing-atlas) 

[https://www.thoughtworks.com/insights/blog/two-opposing-iot-revolutions-play](https://www.thoughtworks.com/insights/blog/two-opposing-iot-revolutions-play) 

[https://www.thoughtworks.com/insights/blog/iot-first-hype-then-plumbing](https://www.thoughtworks.com/insights/blog/iot-first-hype-then-plumbing)  

[https://www.thoughtworks.com/insights/blog/apollos-temple-context-quantified-self-and-internet-things](https://www.thoughtworks.com/insights/blog/apollos-temple-context-quantified-self-and-internet-things) 

# API DESIGN

[https://cloud.google.com/apis/design/](https://cloud.google.com/apis/design/)

[https://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api](https://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)

[https://open.nytimes.com/building-a-text-editor-for-a-digital-first-newsroom-f1cb8367fc21](https://open.nytimes.com/building-a-text-editor-for-a-digital-first-newsroom-f1cb8367fc21)

[http://www.president.go.ke/2015/03/11/integrated-data-system-to-make-e-government-a-reality/](http://www.president.go.ke/2015/03/11/integrated-data-system-to-make-e-government-a-reality/) 

[http://www.techweez.com/2016/02/09/iprs-integration-with-records/](http://www.techweez.com/2016/02/09/iprs-integration-with-records/) 

# DESIGN

[https://www.flutterwave.com/](https://www.flutterwave.com/) 

[https://www.immuta.com/](https://www.immuta.com/) 

[http://formation.ai/](http://formation.ai/)

[https://dribbble.com/stripe](https://dribbble.com/stripe)

[https://adioma.com/](https://adioma.com/) 

- Create clients, maintain information and upload documents
- Create and assign tasks to business users
- Create and manage groups, roles and constraints
- Generate and automate email and SMS communication
- Create, configure and launch new loan and deposit offerings
- Manage workflows and loan lifecycle
- Define accounting rules and manage accounting books
- Define internal controls and security measures

**Tala Kenya 7.12.1** apk requires following permissions on your android device.

- access approximate location.
- access precise location.
- access information about networks.
- access information about Wi-Fi networks.
- connect to paired bluetooth devices.
- access the camera device.
- change network connectivity state.
- list of accounts in the Accounts Service.
- open network sockets.
- see outgoing call numbers/ redirect the call to a different number/ abort the call altogether.
- read the user's calendar data.
- read the user's call log.
- read the user's contacts data.
- read from external storage.
- read only access to phone state.
- read SMS messages.
- receive SMS messages.
- send SMS messages.
- access to the vibrator.
- prevent processor from sleeping or screen from dimming.
- write (but not read) the user's contacts data.
- write to external storage.

Features

- **A super fast sign-up and approval process**
- **Affordable fees, an easy schedule for repayments**
- **Buildable credit system in place**
- **Plenty of motivation and customer success stories**

# Multi Tenancy

## Row-Level Multitenancy

Each table has a `tenant_id` column in its partition key that determines which tenant the record belongs to.

## Table-Level Multitenancy

Each table has `tenant_id` suffix in its name. Tenants are isolated by tables.

## Keyspace-Level Multitenancy

Tenants use different keyspaces. Keyspace name maps to a single `tenant_id`.