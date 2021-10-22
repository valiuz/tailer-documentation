# API To Storage usage examples

## Pub/Sub to GCS examples

### Pub/Sub endpoint

Once the ATS configuration file has been deployed, a Pub/Sub endpoint will be available and ready to receive data.

The Pub/Sub URL will be constructed as follow:

`https://pubsub.googleapis.com/v1/projects/[GCP_PROJECT_ID]/topics/ats-topic-[CONFIGURATION_ID]:publish`

Considering the following value coming from your ATS configuration :

* gcp\_project\_id (from "source" attribute: fd-io-jarvis-demo-dlk
* configuration\_id : 000099-ats-example-products

The resulting Pub/Sub endpoint will be:

`https://pubsub.googleapis.com/v1/projectsfd-io-jarvis-demo-dlk/topics/ats-topic-000099-ats-example-products:publish`

### How to build a data payload

Pub/Sub expects a Base64 encoded JSON string.

API To Storage processor expects an array of objects build with the following schema:

```
{
    "input-data": [
        { JSON Object 1 },
        { JSON Object 2 },
        { JSON Object 3 },
        { JSON Object 4 },
        ...
    ]
}
```

Example:

```
{
    "input-data": [
        {
            "product_id": "123456789",
            "label": "Some label ABC",
            "description": "A specific description for product 123456789",
            "new_attribute": "test"
        },
        {
            "product_id": "987654321",
            "label": "Some label YUI",
            "description": "A specific description for product 987654321"
        },
        {
            "product_id": "66668888",
            "label": "Some label XYZ",
            "description": "A specific description for product 66668888"
        }
    ]
}
```

Then, the JSON string must be Base64 encoded. Using the previous example, the result will be:

`eyAgICAiaW5wdXQtZGF0YSI6IFsgICAgICAgIHsgICAgICAgICAgICAicHJvZHVjdF9pZCI6ICIxMjM0NTY3ODkiLCAgICAgICAgICAgICJsYWJlbCI6ICJTb21lIGxhYmVsIEFCQyIsICAgICAgICAgICAgImRlc2NyaXB0aW9uIjogIkEgc3BlY2lmaWMgZGVzY3JpcHRpb24gZm9yIHByb2R1Y3QgMTIzNDU2Nzg5IiwgICAgICAgICAgICAibmV3X2F0dHJpYnV0ZSI6ICJ0ZXN0IiAgICAgICAgfSwgICAgICAgIHsgICAgICAgICAgICAicHJvZHVjdF9pZCI6ICI5ODc2NTQzMjEiLCAgICAgICAgICAgICJsYWJlbCI6ICJTb21lIGxhYmVsIFlVSSIsICAgICAgICAgICAgImRlc2NyaXB0aW9uIjogIkEgc3BlY2lmaWMgZGVzY3JpcHRpb24gZm9yIHByb2R1Y3QgOTg3NjU0MzIxIiAgICAgICAgfSwgICAgICAgIHsgICAgICAgICAgICAicHJvZHVjdF9pZCI6ICI2NjY2ODg4OCIsICAgICAgICAgICAgImxhYmVsIjogIlNvbWUgbGFiZWwgWFlaIiwgICAgICAgICAgICAiZGVzY3JpcHRpb24iOiAiQSBzcGVjaWZpYyBkZXNjcmlwdGlvbiBmb3IgcHJvZHVjdCA2NjY2ODg4OCIgICAgICAgIH0gICAgXX0=`

### Send data payload to Pub/Sub

Using CURL, we can now simply send the data payload to Pub/Sub:

```
curl --request POST \
--http2 \
--header "content-type:application/json" \
--header "Authorization: Bearer ${TOKEN}" \
--data '{"messages": [
    {"attributes": {}, "data": "eyAgICAiaW5wdXQtZGF0YSI6IFsgICAgICAgIHsgICAgICAgICAgICAicHJvZHVjdF9pZCI6ICIxMjM0NTY3ODkiLCAgICAgICAgICAgICJsYWJlbCI6ICJTb21lIGxhYmVsIEFCQyIsICAgICAgICAgICAgImRlc2NyaXB0aW9uIjogIkEgc3BlY2lmaWMgZGVzY3JpcHRpb24gZm9yIHByb2R1Y3QgMTIzNDU2Nzg5IiwgICAgICAgICAgICAibmV3X2F0dHJpYnV0ZSI6ICJ0ZXN0IiAgICAgICAgfSwgICAgICAgIHsgICAgICAgICAgICAicHJvZHVjdF9pZCI6ICI5ODc2NTQzMjEiLCAgICAgICAgICAgICJsYWJlbCI6ICJTb21lIGxhYmVsIFlVSSIsICAgICAgICAgICAgImRlc2NyaXB0aW9uIjogIkEgc3BlY2lmaWMgZGVzY3JpcHRpb24gZm9yIHByb2R1Y3QgOTg3NjU0MzIxIiAgICAgICAgfSwgICAgICAgIHsgICAgICAgICAgICAicHJvZHVjdF9pZCI6ICI2NjY2ODg4OCIsICAgICAgICAgICAgImxhYmVsIjogIlNvbWUgbGFiZWwgWFlaIiwgICAgICAgICAgICAiZGVzY3JpcHRpb24iOiAiQSBzcGVjaWZpYyBkZXNjcmlwdGlvbiBmb3IgcHJvZHVjdCA2NjY2ODg4OCIgICAgICAgIH0gICAgXX0="}]}' \
"https://pubsub.googleapis.com/v1/projectsfd-io-jarvis-demo-dlk/topics/ats-topic-000099-ats-example-products:publish"
```

Note that the Pub/Sub endpoint is not publicly callable. You need to specify a JWT in the HTTP Authorization header to go though.

The user MUST have the Pub/Sub Publisher role to the according GCP Project to have access.

This is a link to GCP documentation on how to generate a JWT from a service account credential file : [https://cloud.google.com/endpoints/docs/openapi/service-account-authentication](https://cloud.google.com/endpoints/docs/openapi/service-account-authentication)

