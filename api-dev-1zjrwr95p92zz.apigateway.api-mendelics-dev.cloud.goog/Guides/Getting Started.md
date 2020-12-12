# Getting Started
---

### Before you begin
1. To access the Mendelics API Gateway you will need to contact us in order to obtain a service account and a secret that you will use to generate a [JWT](https://jwt.io/) token that you will have to use to comunicate with our API.

### Generating a JWT token
There are many tutorials on the internet explaining how to get a JWT token from a GCP service account. The google tutorial can be found in this [link](https://cloud.google.com/api-gateway/docs/authenticate-service-account#making_an_authenticated_request). 

### Send the singnet JWT token to api-gateway
In every request to the gateway you will have to sent a signed token in every request. The [link](https://cloud.google.com/api-gateway/docs/authenticate-service-account#making_an_authenticated_request) also show how to send the token in the `Authorization: Bearer` header.

### Using the API

Browse the reference section of this site to see examples of what you can do with this API and how to use it. You can use the **Try this API** tool on the right side of an API method page to generate a sample request.
