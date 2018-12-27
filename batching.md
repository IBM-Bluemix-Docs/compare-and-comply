---

copyright:
  years: 2018
lastupdated: "2018-12-27"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Using batch processing
{: #batching}

The `/v1/batches` APIs enable you to run Compare and Comply methods over a collection of input documents. Batch processing is available only for the `POST /v1/html_conversion`, `POST /v1/element_classification`, and `POST /v1/tables` methods. Batch processing is _not_ available for the `POST /v1/comparison` method.

All batch requests return a batch status object that include a `batch_id`. The `batch_id` can be used to monitor the status of a request and to cancel a request.

Batch-processing requests require access credentials for a Cloud Object Storage (COS) instance and the name of an input and output bucket in that instance. Details are provided in [Before you begin](/docs/services/compare-and-comply/batching.html#before-you-batch).
{: important}

Files submitted through the `/v1/batches` APIs can be up to 50 MB in size.
{: note}

## API endpoints
{: #batching_endpoints}

The batching API endpoints are as follows.

 - `POST /v1/batches`: Submit a batch-processing request.
 - `GET /v1/batches`: Gets the list of submitted batch-processing jobs.
 - `GET /v1/batches/{batch_id}`: Gets information about a specified batch-prcoessing request.
 - `PUT /v1/batches/{batch_id}`: Updates a pending or active batch-processing request.

## Before you begin
{: #before-you-batch}

Before you use batch processing, ensure that you are set with the following:
 - All of the items listed in [Before you begin in Getting Started](/docs/services/compare-and-comply/getting-started.html#before-you-begin).
 - A [Cloud Object Storage (COS) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/cloud-object-storage){:new_window} instance on your IBM Cloud Private cluster. For information, see the COS documentation at:

  - [About IBM Cloud Object Storage ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/cloud-object-storage/about-cos.html#about-ibm-cloud-object-storage){: new_window}
  - [Storing data on IBM Cloud Object Storage ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/containers/cs_storage_cos.html#object_storage){: new_window}

## Create and run a batch processing request
{: #post-batch}

The `POST /v1/batches` method creates a batch request that takes all documents in a COS bucket and uses them as input documents; applies a Compare and Comply method to them; and writes the resulting output into another COS bucket.

In a `bash` shell or equivalent environment such as Cygwin, use the `POST /v1/batches` method to create and run a batch processing request. The method takes the following input parameters:
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
  - `function` (**required** `string`): The method to be run against the documents. Possible values are `html_conversion`, `element_classification`, and `tables`.
  - `model` (optional `string`): The ID of the data kit for the model to use. The default is `contracts` when the `function` is `html_conversion` or `element_classification`. The default is `tables` when the function is `tables`.
  - `input_credentials_file` (**required** `file`): A JSON file that lists the COS input credentials. At a minimum, the credentials must enable `READ` permissions on the bucket defined by the `input_bucket_name` parameter.
  - `input_bucket_location` (**required** `string`): The geographical location of the COS input bucket as listed on the **Endpoint** tab of your COS instance; for example, `us-geo`, `eu-geo`, or `ap-geo`.
  - `input_bucket_name` (**required** `string`): The name of the COS input bucket.
  - `output_credentials_file` (**required** `file`): A JSON file that lists the COS output credentials. At a minimum, the credentials must enable `READ` and `WRITE` permissions on the bucket defined by the `output_bucket_name` parameter.
  - `output_bucket_location` (**required** `string`): The geographical location of the COS output bucket as listed on the **Endpoint** tab of your COS instance; for example, `us-geo`, `eu-geo`, or `ap-geo`.
  - `output_bucket_name` (**required** `string`): The name of the COS output bucket.

```bash
curl -X POST -u "apikey":"{apikey_value}" \
https://{ICP_IP_address}:{port}/{deployment-name}/compare-and-comply/api/v1/batches?version=2018-10-15&function=element_classification \
  -F input_credentials_file=@{path/to/input_credentials_file} \
  -F input_bucket_location={geography} \
  -F input_bucket_name={input_bucket_name} \
  -F output_credentials_file=@{path/to/output_credentials_file} \
  -F output_bucket_location={geography} \
  -F output_bucket_name={output_bucket_name}
  ```
{: codeblock}

The values of the `input_credentials_file` and `output_credentials_file` are files that contain the COS service credentials as a JSON object. You can obtain the JSON from the COS web console page on the **Service credentials** tab. The JSON resembles the following:
{: note}

```
{
  "apikey": "ursB8336HDIeqbUmz1D1iuK_ddM0K2bKxvZjY3EEbl98",
  "endpoints": "https://cos-service.bluemix.net/endpoints",
  "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloud-object-storage:global:a/a75da04c40cf41c9d881eadd3812339c:73389937-2ab2-4f92-83d3-69c2b6b78ea7::",
  "iam_apikey_name": "auto-generated-apikey-112d0b22-259d-4f0d-a302-c882a659d3bb",
  "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Writer",
  "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a75da04c40cf41c9d881eadd3812339c::serviceid:ServiceId-9f4eeeae-e345-4178-91e1-d517328c07b8",
  "resource_instance_id": "crn:v1:bluemix:public:cloud-object-storage:global:a/a75da04c40cf41c9d881eadd3812339c:73389937-2ab2-4f92-83d3-69c2b6b78ea7::"
}
```
{: screen}

The following example command creates and runs a batch request with the following attributes:

  - `function`: `element_classification`
  - `input_credentials_file`: `/Users/jsmith/cos_input_creds.json`
  - `input_bucket_location`: `us-geo`
  - `input_bucket_name`: `my_cos_input_bucket`
  - `output_credentials_file`: `/Users/jsmith/cos_output_creds.json`
  - `output_bucket_location`: `us-geo`
  - `output_bucket_name`: `my_cos_output_bucket`


```bash
curl -X POST -u "apikey":"{apikey_value}" \
https://{ICP_IP_address}:{port}/{deployment-name}/compare-and-comply/api/v1/batches?version=2018-10-15&function=element_classification \
    -F input_credentials_file=@/Users/jsmith/cos_input_creds.json \
    -F input_bucket_location=us-geo \
    -F input_bucket_name=my_cos_input_bucket \
    -F output_credentials_file=@/Users/jsmith/cos_output_creds.json \
    -F output_bucket_location=us-geo \
    -F output_bucket_name=my_cos_output_bucket
```
{: codeblock}

The method returns the batch-request status as a JSON object, as in the following example.

```
{
  "function": "element_classification",
  "input_bucket_location": "us-geo",
  "input_bucket_name": "my_cos_input_bucket",
  "output_bucket_location": "us-geo",
  "output_bucket_name": "my_cos_output_bucket",
  "batch_id": "65698e79-d601-4d8f-b46d-dd1369020b58",
  "document_counts": {
    "total": 5,
    "pending": 0,
    "successful": 5,
    "failed": 0
  },
  "status": "completed",
  "created": "2018-07-30T20:26:03.411+0000",
  "updated": "2018-07-30T20:26:09.433+0000"
}
```
{: screen}

The returned status object, which is identical for all `/v1/batches` methods, contains the count of all documents in the batch (`total`), the count of documents that have not yet been processed (`pending`), the count of documents that have been successfully processed (`successful`), and the count of documents that were unable to be processed (`failed`).

### Batch processing workflow
{: #batch-workflow}

Batch processing proceeds as follows:

  - When you first submit a batch request, the status is `pending`.
  - When the batch engine starts handling documents, the status changes to `active` until the service attempts to process all documents in the batch or the batch job is cancelled by using the [`PUT /v1/batches/{batch_id}` method](#put-cancel-batch).
  - When all documents in the batch job have been processed, the status changes to `completed`.

## Get a list of submitted batch requests
{: #get-list-batch}

You can get a list of batch requests you have submitted by using the `GET /v1/batches` method.

In a `bash` shell or equivalent environment such as Cygwin, use the `GET /v1/batches` method to get the status of all batch processing requests. The method takes the following input parameters:
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
  - `status` (optional `string`): An optional status string used to filter the output. Permitted values are `pending`, `active`, `completed`, and `canceled`.

```bash
curl -X GET -u "apikey":"{apikey_value}" \
https://{ICP_IP_address}:{port}/{deployment-name}/compare-and-comply/api/v1/batches?version=2018-10-15
```
{: codeblock}

The method returns a JSON object that contains batch-request status objects, as in the following example:

```
{
    "batches": [
        {
            "function": "element_classification",
            "input_bucket_location": "us-geo",
            "input_bucket_name": "my_cos_input_bucket",
            "output_bucket_location": "us-geo",
            "output_bucket_name": "my_cos_output_bucket",
            "batch_id": "65698e79-d601-4d8f-b46d-dd1369020b58",
            "document_counts": {
              "total": 5,
              "pending": 0,
              "successful": 5,
              "failed": 0
            },
            "status": "completed",
            "created": "2018-07-30T20:26:03.411+0000",
            "updated": "2018-07-30T20:26:09.433+0000"
        },
        {
            "function": "html_conversion",
            "input_bucket_location": "us-geo",
            "input_bucket_name": "my_cos_input_bucket",
            "output_bucket_location": "us-geo",
            "output_bucket_name": "my_cos_output_bucket",
            "batch_id": "0a7f8ab8-97a0-4b67-9fea-feacafbb0b20",
            "document_counts": {
              "total": 5,
              "pending": 0,
              "successful": 5,
              "failed": 0
            },
            "status": "completed",
            "created": "2018-07-30T19:15:30.197+0000",
            "updated": "2018-07-30T19:15:41.830+0000"
        }
    ]
}
```
{: screen}

## Get the status of a specified batch request
{: #get-spec-batch}

You can get a list of a specific batch requests you have submitted by using the `GET /v1/batches/{batch_id}` method.

In a `bash` shell or equivalent environment such as Cygwin, use the `GET /v1/batches/{batch_id}` method to get the status of a specified batch processing request. The method takes the following input parameters:
  - `id` (**required** `string`): The `batch_id` of the batch request you want to query. Obtain the `batch_id` by using the `GET /v1/batches` method as described in [Get a list of submitted batch requests](#get-list-batch).
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.

```bash
curl -X GET -u "apikey":"{apikey_value}" \
https://{ICP_IP_address}:{port}/{deployment-name}/compare-and-comply/api/v1/batches/0a7f8ab8-97a0-4b67-9fea-feacafbb0b20?version=2018-10-15
```
{: codeblock}

The method returns a JSON object that provides the status of the specified batch request, as in the following example:

```
{
    "function": "html_conversion",
    "input_bucket_location": "us-geo",
    "input_bucket_name": "my_cos_input_bucket",
    "output_bucket_name": "my_cos_output_bucket",
    "batch_id": "0a7f8ab8-97a0-4b67-9fea-feacafbb0b20",
    "document_counts": {
      "total": 5,
      "pending": 5,
      "successful": 0,
      "failed": 0
    },
    "status": "pending",
    "created": "2018-07-30T20:10:30.197+0000",
    "updated": "2018-07-30T20:10:41.830+0000"
}
```
{: screen}

## Rescan the input bucket
{: #put-add-batch}

You can have the service check for new documents in the input bucket by using the `PUT /v1/batches/{batch_id}` method with the `rescan` action. The batch request must already exist and have a status of either `pending` or `active`. The method has no effect on batch requests with a status of `completed` or `canceled`. When you run this method, the service scans the input bucket for documents that have been added or updated since the batch was created or since the last scan.

In a `bash` shell or equivalent environment such as Cygwin, use the `PUT /v1/batches/{batch_id}` method to cancel a batch processing request. The method takes the following input parameters:
  - `action` (**required** `string`): The action to be performed by the method. To add documents to a batch request, use the `rescan` action.
  - `id` (**required** `string`): The `batch_id` of the batch request for which you want to rescan the input bucket. Obtain the `batch_id` by using the `GET /v1/batches` method as described in [Get a list of submitted batch requests](#get-list-batch).  
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.

```bash
curl -X PUT -u "apikey":"{apikey_value}" \
https://{ICP_IP_address}:{port}/{deployment-name}/compare-and-comply/api/v1/batches/0a7f8ab8-97a0-4b67-9fea-feacafbb0b20?version=2018-10-15&action=rescan
```
{: codeblock}

The method returns a JSON object that provides the status of the specified batch request, as in the following example:

```
{
  "function": "html_conversion",
  "input_bucket_location": "us-geo",
  "input_bucket_name": "my_cos_input_bucket",
  "output_bucket_name": "my_cos_output_bucket",
  "batch_id": "0a7f8ab8-97a0-4b67-9fea-feacafbb0b20",
  "document_counts": {
    "total": 8,
    "pending": 2,
    "successful": 5,
    "failed": 1
  },
  "status": "active",
  "created": "2018-07-30T20:15:30.197+0000",
  "updated": "2018-07-30T20:16:22.724+0000"
}
```
{: screen}

## Cancel a batch request
{: #put-cancel-batch}

You can cancel the execution of a batch request by using the `PUT /v1/batches/{batch_id}` method with the `cancel` action. If the batch has already completed or was already cancelled, the service disregards the request.

In a `bash` shell or equivalent environment such as Cygwin, use the `PUT /v1/batches/{batch_id}` method to cancel a batch processing request. The method takes the following input parameters:
  - `action` (**required** `string`): The action to be performed by the method. To cancel a batch request, use the `cancel` action.
  - `id` (**required** `string`): The `batch_id` of the batch request you want to cancel. Obtain the `batch_id` by using the `GET /v1/batches` method as described in [Get a list of submitted batch requests](#get-list-batch).  
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.

```bash
curl -X PUT -u "apikey":"{apikey_value}" \
https://{ICP_IP_address}:{port}/{deployment-name}/compare-and-comply/api/v1/batches/0a7f8ab8-97a0-4b67-9fea-feacafbb0b20?version=2018-10-15&action=cancel
```
{: codeblock}

The method returns a JSON object that provides the status of the specified batch request, as in the following example:

```
{
  "function": "html_conversion",
  "input_bucket_location": "us-geo",
  "input_bucket_name": "my_cos_input_bucket",
  "output_bucket_name": "my_cos_output_bucket",
  "batch_id": "0a7f8ab8-97a0-4b67-9fea-feacafbb0b20",
  "document_counts": {
    "total": 8,
    "pending": 0,
    "successful": 5,
    "failed": 0
  },
  "status": "canceled",
  "created": "2018-07-30T20:20:30.197+0000",
  "updated": "2018-07-30T20:21:22.724+0000"
}
```
{: screen}
