# How to create a COVID test 
---

### TL;DR
To create a test you must first create a `payment`. This payment will return a code, by which you will be able to create one or more `logisticts` requests. Upon the logistics request, we will inform you which tests you can create, through a callback function that will send a list of barcodes. These barcodes will be the unique identifiers of the tests. In the end of the process we will send you the result of the test, identified by its barcode.

### Create a payment request
A payment request is the first step to create a Mendelics test. A payment request is needed to accountability of Mendelics Financial Team and also to make sure that no `logistics` can be created without an associated payment. To create a payment request see this [link](https://endpointsportal.api-mendelics-dev.cloud.goog/docs/api-dev-1zjrwr95p92zz.apigateway.api-mendelics-dev.cloud.goog/0/routes/erp/payments/insert/post).

### Create a logistics request
A logistics request is necessary so Mendelics Logistics Team can separate and send you the tubes that will be used to collect the samples to be tested. This process may take a while. When it's finished, the Logistics Team will send, through a callback function, a list of barcodes that will be sent along the tubes, so you can identify the samples and create the tests. To create a logistics request see this [link](https://endpointsportal.api-mendelics-dev.cloud.goog/docs/api-dev-1zjrwr95p92zz.apigateway.api-mendelics-dev.cloud.goog/0/routes/logistica/order/request_from_covid_payment/post). 

### Creating a test
Once you have the barcodes that were associated with your payment, you will be able to create a test for each one of them. This step must be done before sending back the tubes with the collected material. If any tube arrives in our Triage Team without a respective test created, the barcode will be invalidated. To create the test see this  [link](https://endpointsportal.api-mendelics-dev.cloud.goog/docs/api-dev-1zjrwr95p92zz.apigateway.api-mendelics-dev.cloud.goog/0/routes/diagnostics/tests/create/post). 

### Receiving the results
After creating and sending the test to Mendelics, the laboratory will analyze the test and send the results. The results will be send as a `json`, with the following format:

```
{
    "sample_code": "COV1234567890",
    "result": "<result>"
}
```

Where `<result>` can be `negative`, `positive` or `non_compliance`. 
