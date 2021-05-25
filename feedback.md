---

copyright:
  years: 2018, 2021
lastupdated: "2021-05-25"

subcollection: compare-and-comply

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

# Using the feedback APIs
{: #feedback}

Users, preferably subject-matter experts (SMEs), can use the IBM Watson Compare and Comply service's feedback APIs to provide feedback on a parsed document. You can provide feedback on any element that has been labeled by the service. The feedback is associated with the document for future review and consideration. The feedback APIs enable users to submit, get, and delete feedback.
{: shortdesc}

Feedback is not immediately incorporated into the training model, nor is it guaranteed to be incorporated at a later date. Instead, submitted feedback is used to suggest future updates to the training model.
{: important}

## API endpoints
{: #feedback_endpoints}

The feedback API endpoints are as follows.

 - `POST /v1/feedback`: Creates or adds feedback to a document.
 - `GET /v1/feedback`: Gets all feedback from a document.
 - `GET /v1/feedback/{feedback_id}`: Gets specified feedback from a document.
<!-- `DELETE /v1/feedback`: Deletes all feedback from the document. -->
 - `DELETE /v1/feedback/{feedback_id}`: Deletes specified feedback from a document.

## Warnings
{: #feedback_warnings}

Observe the following warnings and precautions when working with the feedback APIs.

  - Users of the feedback API must be able to provide accurate, consistent, and professionally informed feedback on documents.
  - Use extreme caution if you issue the `POST /v1/feedback` or `DELETE /v1/feedback/{feedback_id}` methods.
  - You cannot change posted feedback. You can, however, delete it and replace it by reposting different feedback.
  - The unauthorized use of a `GET /v1/feedback` or `GET /v1/feedback/{feedback_id}` method on a confidential document can potentially result in information being exposed to unauthorized users.

## Workflow for using feedback
{: #feedback_steps}

In the following scenario, an SME named Stuart reviews a parsed governing document and provides feedback on it by using the feedback API. After Stuart finishes his review, an SME named Ann takes the document, reviews it and Stuart's feedback, and uses the feedback API to provide additional feedback. At a high level, the steps are:

1. Stuart reviews the legal document and labels it by using the the `POST /v1/feedback` API method, as described in [Adding feedback](#add_feedback).
1. Ann starts reviewing the legal document that was labeled by Stuart.
1. Ann retrieves all feedback provided on the document by using the `GET /v1/feedback` method, as described in [Getting all feedback](#get_all_feedback).
1. Ann provides her own feedback by using the `POST /v1/feedback` API method.
1. Ann retrieves the feedback she provided by using the `GET /v1/feedback/{feedback_id}` method, as described in [Getting specific feedback](#get_spec_feedback).
1. Ann, Stuart, or another user can optionally delete <!-- all feedback by using the `DELETE /v1/feedback` method, as described in [Deleting feedback](#delete_feedback), or -->specific feedback by using the `DELETE /v1/feedback/{feedback_id}` method, as described in [Deleting specific feedback](#delete_spec_feedback).

These steps are described in more detail in the following sections.

## Adding feedback
{: #add_feedback}

You can add feedback to a document programmatically by using the `POST /v1/feedback` method.

In a `bash` shell or equivalent environment such as Cygwin, issue the following command to add feedback to a document, with values as follows:
  - Replace `{apikey}` with the API key you copied in [Before you begin in Getting Started](/docs/compare-and-comply?topic=compare-and-comply-getting_started#before-you-begin).
  - Create a `feedback_data` object, which is a specifically formatted object specifying the feedback you want to add to the document. The `feedback_data` object must be in the following format.
    ```json
    {
      "document": {
        "hash": string,
        "title": string, # optional, the document title,
      },
      "feedback_type": string, # required, string that identifies the type of feedback; currently supported value is `element_classification`
      "model_id": string, # optional, identifies the model id,
      "model_version": string, # optional, identifies the model version,
      "location": {
        "begin": int,
        "end": int
      },
      "text": string, # required, text to which feedback is being applied
      "original_labels": { # required, empty node is allowed
        "types": [ # required, empty array is allowed
          {
            "label": { # contains labeling info in the form of nature and party attributes
              "nature": string, # required, empty string is allowed,
              "party": string, # required, empty string is allowed
            },
            "provenance_ids": [ # required, empty array is allowed
                string, ...
            ]
          }
        ],
        "categories": [ # required, empty array is allowed
          {
            "label": string, # required, empty string is not allowed,
            "provenance_ids": [ # required, empty array is allowed
                string, ...
            ]
          }
        ]
      },
      "updated_labels": { # required, empty node is allowed
        "types": [
            {
                "label": {
                  "nature": string, # required, empty string is allowed,
                  "party": string, # required, empty string is allowed
                }
            }
        ],
        "categories": [ # required, empty array is allowed
          {
            "label": string, # required, empty string is not allowed
          }
        ]
      }
    }    
    ```
    {: codeblock}

<!--
You can assemble the body of the `feedback_data` object as follows:

  1. Run the `POST /v1/element_classification` method as described at [Step 2: Classify a contract's elements in Getting Started](/docs/compare-and-comply?topic=compare-and-comply-getting_started#parse_contract), and save its output to a file.
  1. For the `feedback_type` field, specify a value of `"element_classification"`.
  1. For the `collection_id` field, optionally specify a value of your choosing.
  1. In the saved output file, look in the `elements` array for the element for which you want to provide feedback. The most common element for posting feedback is `sentence`.
  1. Locate the `begin` and `end` indexes for the element you identified; for example:
    ```
    "sentence": {
        "begin": 31443,
        "end": 31488    
    }
    ```
    {: screen}
  1.
-->

An example command follows.

```bash
curl -X POST -u "apikey:{apikey}" -H 'Content-Type: application/json' 
https://{cluster_CA_domain}/{deployment_name}/compare-and-comply/api/v1/feedback?version=2018-10-15
-d '{
      "user_id": "7uy9c1f4-57dd-42b5-9586-a2ddf3ed8b64",
      "comment": "user comments",
      "feedback_data": {
        "feedback_type": "element_classification",
        "document": {
          "title": "Legal Approval SOW",
          "hash": "91edc2ff254d29f7a4922635ad47276a"
        },
        "model_id": "contracts",
        "model_version": "10.00",
        "location": {
          "begin": 214,
          "end": 237
        },
        "text": "1. IBM will provide a Senior Managing Consultant / expert resource, for up to 80 hours, to assist Florida Power & Light (FPL) with the creation of an IT infrastructure unit cost model for existing infrastructure.",
        "original_labels": {
          "types": [
            {
              "label": {
                "nature": "Obligation",
                "party": "IBM"
              },
              "provenance_ids": [
                  "85f5981a-ba91-44f5-9efa-0bd22e64b7bc",
                  "ce0480a1-5ef1-4c3e-9861-3743b5610795"
              ]
            },
            {
              "label": {
                "nature": "End User",
                "party": "Exclusion"
              },
              "provenance_ids": [
                  "85f5981a-ba91-44f5-9efa-0bd22e64b7bc",
                  "ce0480a1-5ef1-4c3e-9861-3743b5610795"
              ]
            }
          ],
          "categories": [
            {
              "label": "Responsibilities",
              "provenance_ids": [
                  "85f5981a-ba91-44f5-9efa-0bd22e64b7bc"
              ]
            },
            {
              "label": "Amendments",
            }
          ]
        },
        "updated_labels": {
          "types": [
            {
              "label": {
                "nature": "Obligation",
                "party": "IBM"
              }
            },
            {
              "label": {
                "nature": "Disclaimer",
                "party": "Buyer"
              }
            }
          ],
          "categories": [
            {
              "label": "Responsibilities"
            },
            {
              "label": "Audits"
            }
          ]
        }
      }
    }'
```
{: codeblock}

The output of the command resembles the following.

```json
{
  "feedback_id": "9gh7c1f4-57dd-42b5-9586-a2ddf3ed8b34",
  "user_id": "7uy9c1f4-57dd-42b5-9586-a2ddf3ed8b64",
  "comment": "user comments",
  "feedback_data": {
    "feedback_type": "element_classification",
    "document": {
      "title": "Legal Approval SOW",
      "hash": "H1234"
    },
    "model_id": "contracts",
    "model_version": "10.00",
    "location": {
      "begin": 214,
      "end": 237
    },
    "text": "1. IBM will provide a Senior Managing Consultant / expert resource, for up to 80 hours, to assist Florida Power & Light (FPL) with the creation of an IT infrastructure unit cost model for existing infrastructure.",
    "original_labels": {
      "types": [
        {
          "label": {
            "nature": "Obligation",
            "party": "IBM"
          },
          "modification": "not_changed",
          "provenance_ids": [
              "85f5981a-ba91-44f5-9efa-0bd22e64b7bc",
              "ce0480a1-5ef1-4c3e-9861-3743b5610795"
          ]
        },
        {
          "label": {
            "nature": "End User",
            "party": "Exclusion"
          },
          "modification": "removed",
          "provenance_ids": [
              "85f5981a-ba91-44f5-9efa-0bd22e64b7bc",
              "ce0480a1-5ef1-4c3e-9861-3743b5610795"
          ]
        }
      ],
      "categories": [
        {
          "label": "Responsibilities",
          "modification": "not_changed",
          "provenance_ids": [
              "85f5981a-ba91-44f5-9efa-0bd22e64b7bc"
          ]
        },
        {
          "label": "Amendments",
          "modification": "removed"
        }
      ]
    },
    "updated_labels": {
      "types": [
        {
          "label": {
            "nature": "Obligation",
            "party": "IBM"
          },
          "modification": "not_changed"
        },
        {
          "label": {
            "nature": "Disclaimer",
            "party": "Buyer"
          },
          "modification": "added"
        }
      ],
      "categories": [
        {
          "label": "Responsibilities",
          "modification": "not_changed"
        },
        {
          "label": "Audits",
          "modification": "added"
        }
      ]
    }
  }
}
```
{: screen}

The command makes the following feedback:

  - It removes the type label `{"nature": "End User", "party": "Exclusion"}`.
  - It removes the category `{"label": "Amendments"}`.

  Because the previous items are present in the `original_labels` object but not in the `updated_labels` object, the service considers the type and category to have been removed.

  - It adds the type label `{"nature": "Disclaimer", "party": "Buyer"}`.
  - It adds the category `{"label": "Audits"}`.

  Because the previous items are present in the `updated_labels` object but not in the `original_labels` object, the service considers the type and category to have been added.

  - It does not change the type label `{"nature": "Obligation", "party": "IBM"}`.
  - It does not change the category `{"label": "Responsibilities"}`.

  Because the previous items are present in both the `original_labels` and `updated_labels` objects, the service considers the type and category to be correct.


## Getting all feedback
{: #get_all_feedback}

You can retrieval all feedback that has been added to a document by using the `GET /v1/feedback/` method. The method takes the following input parameters:

  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
  - `feedback_type` (optional `string`): If this parameter is specified, the service returns only the records having the specified feedback type. Currently the only supported value is `element_classification`.
  - `user_id` (optional `string`): If this parameter is specified, the service returns only the records with the specified user ID.
  - `before` (optional `string`): If this parameter is specified in the format `YYYY-MM-DD`, the service returns only the records with dates prior to the specified date.
  - `after` (optional `string`): If this parameter is specified in the format `YYYY-MM-DD`, the service returns only the records with dates subsequent to the specified date.
  - `document_title` (optional `string`): If this parameter is specified, the service returns only the records associated with the specified title.
  - `model_id` (optional `string`): If this parameter is specified, the service returns only the records with the specified element model ID.
  - `model_version` (optional `string`): If this parameter is specified, the service returns only the records with the specified element model version.  
  - `category_removed` (optional `string`): A comma-separated list of `categories`. If this parameter is specified, the service returns only the records that have one or more of the specified `categories` removed. See [Categories](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing#contract_categories) for a table of valid `categories`.
  - `category_added` (optional `string`): A comma-separated list of `categories`. If this parameter is specified, the service returns only the records that have one or more of the specified `categories` added. See [Categories](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing#contract_categories) for a table of valid `categories`.
  - `category_not_changed` (optional `string`): A comma-separated list of `categories`. If this parameter is specified, the service returns only the records that have one or more of the specified `categories` unchanged. See [Categories](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing#contract_categories) for a table of valid `categories`.
  - `type_removed` (optional `string`): A comma-separated list of `types` in which each `type` value is of the form `nature:party`. If this parameter is specified, the service returns only the records that have one or more of the specified types removed. See [Types](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing#contract_types) for a table of valid `types` (that is, `nature` and `party` pairs).
  - `type_added` (optional `string`): A comma-separated list of `types` in which each `type` object is of the form `{"nature": "{nature}", "party": "{party}"}`. If this parameter is specified, the service returns only the records that have one or more of the specified types added. See [Types](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing#contract_types) for a table of valid `types` (that is, `nature` and `party` pairs).
  - `type_not_changed` (optional `string`): A comma-separated list of `types` in which each `type` value is of the form `nature:party`. If this parameter is specified, the service returns only the records that have one or more of the specified types unchanged. See [Types](/docs/compare-and-comply?topic=compare-and-comply-contract_parsing#contract_types) for a table of valid `types` (that is, `nature` and `party` pairs).
  - `page_limit` (optional `int`): The number of documents that you want to be returned in the response. The default is `10`. The maximum is `100`.
  - `cursor` (optional `string`): A string that lists the documents you want to be returned in the response.
  - `sort` (optional `string`): A comma-separated list of fields in the document on which to sort returned results. You can optionally specify a sort direction by prefixing the field with `-` for descending order or `+` for ascending order. Ascending order is the default sort direction.

A example command that combines the `type_added` and `category_removed` parameters is:

```bash
curl -X GET -u "apikey:{apikey}" https://{cluster_CA_domain}/{deployment_name}/compare-and-comply/api/v1/feedback?version=2018-10-15&type_added=Definition:None,Disclaimer:Supplier&category_removed=Assignments,Audits
```
{: codeblock}

The output of the command resembles the following.

```
{
  "feedback": [
    {
      "feedback_id": "9730b437-cb86-4d40-9a84-ff6948bb3dd1",
      "user_id": "00000d00-48a3-40ee-9e1b-0d0fa0000b00",
      "created": "2018-07-03T10:16:05-0500",
      "comment": "",
      "feedback_data": {
        "feedback_type": "element_classification",
        "document": {
          "title": "Legal Approval SOW",
          "hash": "dcd82f59c6bb1a289a514b611d531191"
        },
        "model_id": "contracts",
        "model_version": "10.00",
        "original_labels": {
          "types": [
            {
              "label": {
                "nature": "Obligation",
                "party": "IBM"
              },
              "provenance_ids": [
                  "85f5981a-ba91-44f5-9efa-0bd22e64b7bc",
                  "ce0480a1-5ef1-4c3e-9861-3743b5610795"
              ],
              "modification": "not_changed"
            },
            {
              "label": {
                "nature": "End User",
                "party": "Exclusion"
              },
              "provenance_ids": [
                  "85f5981a-ba91-44f5-9efa-0bd22e64b7bc",
                  "ce0480a1-5ef1-4c3e-9861-3743b5610795"
              ],
              "modification": "removed"
            }
          ],
          "categories": [
            {
              "label": "Responsibilities",
              "provenance_ids": [],
              "modification": "not_changed"
            },
            {
              "label": "Amendments",
              "provenance_ids": [],
              "modification": "removed"
            }
          ]
        },
        "location": {
          "begin": 214,
          "end": 237
        },
        "text": "1. IBM will provide a Senior Managing Consultant / expert resource, for up to 80 hours, to assist Florida Power & Light (FPL) with the creation of an IT infrastructure unit cost model for existing infrastructure.",
        "updated_labels": {
          "types": [
            {
              "label": {
                "nature": "Obligation",
                "party": "IBM"
              },
              "modification": "not_changed"
            },
            {
              "label": {
                "nature": "Disclaimer",
                "party": "Buyer"
              },
              "modification": "added"
            }
          ],
          "categories": [
            {
              "label": "Responsibilities",
              "modification": "not_changed"
            },
            {
              "label": "Audits",
              "modification": "added"
            }
          ]
        }
      }
    },
    {
      "feedback_id": "9730b437-cb86-4d40-9a84-ff6948bb3dd1",
      "user_id": "00000d00-48a3-40ee-9e1b-0d0fa0000b00",
      "created": "2018-07-03T10:16:05-0500",
      "comment": "",
      "feedback_data": {
        "feedback_type": "element_classification",
        "document": {
          "title": "Legal Approval SOW",
          "hash": "303ec15cab9ac5653a4a623bd8917dad"
        },
        "model_id": "contracts",
        "model_version": "10.00",
        "original_labels": {
          "types": [
            {
              "label": {
                "nature": "Obligation",
                "party": "FPL"
              },
              "provenance_ids": [
                  "85f5981a-ba91-44f5-9efa-0bd22e64b7bc",
                  "ce0480a1-5ef1-4c3e-9861-3743b5610795"
              ],
              "modification": "not_changed"
            },
            {
              "label": {
                "nature": "End User",
                "party": "Exclusion"
              },
              "provenance_ids": [
                  "85f5981a-ba91-44f5-9efa-0bd22e64b7bc",
                  "ce0480a1-5ef1-4c3e-9861-3743b5610795"
              ],
              "modification": "removed"
            }
          ],
          "categories": [
            {
              "label": "Responsibilities",
              "provenance_ids": [],
              "modification": "not_changed"
            },
            {
              "label": "Amendments",
              "provenance_ids": [],
              "modification": "removed"
            }
          ]
        },
        "location": {
          "begin": 214,
          "end": 237
        },
        "text": "1. IBM will provide a Senior Managing Consultant / expert resource, for up to 80 hours, to assist Florida Power & Light (FPL) with the creation of an IT infrastructure unit cost model for existing infrastructure.",
        "updated_labels": {
          "types": [
            {
              "label": {
                "nature": "Obligation",
                "party": "FPL"
              },
              "modification": "not_changed"
            },
            {
              "label": {
                "nature": "Disclaimer",
                "party": "Buyer"
              },
              "modification": "added"
            }
          ],
          "categories": [
            {
              "label": "Responsibilities",
              "modification": "not_changed"
            },
            {
              "label": "Audits",
              "modification": "added"
            }
          ]
        }
      }
    }
  ],
  "pagination" : {
      "refresh_cursor" : string,
      "next_cursor" : string,
      "refresh_url" : string,
      "next_url": string,
      "total": integer
  }
}
```
{: screen}

## Getting specific feedback
{: #get_spec_feedback}

You can retrieve specific feedback from a document by using the `GET /v1/feedback/{feedback_id}` method. The method takes the following input parameters.

  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
  - `feedback_id` (**required** `string`): The feedback ID for which the service is to return details. You can obtain the `feedback_id` from the output of the `GET /v1/feedback` method.

An example command is:

```bash
curl -X GET -u "apikey:{apikey}" https://{cluster_CA_domain}/{deployment_name}/compare-and-comply/api/v1/feedback/9730b437-cb86-4d40-9a84-ff6948bb3dd1?version=2018-10-15
```
{: codeblock}

The command returns output similar to the following:

```
{
  "feedback_id": "9730b437-cb86-4d40-9a84-ff6948bb3dd1",
  "user_id": "8049c229-4409-4dfe-bfb0-6577c9b361ab",
  "created": "2018-10-08T22:17:45+0000",
  "comment": "",
  "feedback_data": {
    "feedback_type": "element_classification",
    "document": {
      "title": "Legal Approval SOW",
      "hash": "4492935afd3673e04082591d163ad68b"
    },
    "model_id": "contracts",
    "model_version": "10.00",
    "location": {
      "begin": 214,
      "end": 237
    },
    "text": "1. IBM will provide a Senior Managing Consultant / expert resource, for up to 80 hours, to assist Florida Power & Light (FPL) with the creation of an IT infrastructure unit cost model for existing infrastructure.",
    "original_labels": {
      "types": [
        {
          "label": {
            "nature": "Obligation",
            "party": "IBM"
          },
          "modification": "not_changed",
          "provenance_ids": [
              "85f5981a-ba91-44f5-9efa-0bd22e64b7bc",
              "ce0480a1-5ef1-4c3e-9861-3743b5610795"
          ]
        }
      ],
      "categories": [
        {
          "label": "obligation",
          "modification": "removed",
          "provenance_ids": [
              "85f5981a-ba91-44f5-9efa-0bd22e64b7bc",
              "ce0480a1-5ef1-4c3e-9861-3743b5610795"
          ]
        }
      ]
    },
    "updated_labels": {
      "types": [
        {
          "label": {
            "nature": "Obligation",
            "party": "IBM"
          },
          "modification": "not_changed"
        }
      ],
      "categories": [
        {
          "label": "Responsibilities",
          "modification": "added"
        }
      ]
    }
  }
}
```
{: screen}

<!--
###Deleting all feedback
{: #delete_feedback}

You can delete all feedback in a document collection by using the `DELETE /v1/feedback` method. The method requires only the `version` date parameter.

```bash
curl -X DELETE -u "apikey:{apikey}" https://{cluster_CA_domain}/{deployment_name}/compare-and-comply/api/v1/feedback?version=2018-10-15
```
{: pre}

Alternatively, you can delete all feedback from documents with a specific customer ID by specifying the ID in the call headers:

```bash
curl -X DELETE -u "apikey:{apikey}" -H 'x-watson-metadata: customer_id=3910' https://{cluster_CA_domain}/{deployment_name}/compare-and-comply/api/v1/feedback?version=2018-10-15
```
{: pre}

The service returns the following output on success:

```
{
  "status": "200",
  "message": "Successfully deleted the feedback"
}
```
{: codeblock}
-->

## Deleting specific feedback
{: #delete_spec_feedback}

You can delete specific feedback from a document by using the `DELETE /v1/feedback/{feedback_id}` method. The method takes the following input parameters.

  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
  - `feedback_id` (**required** `string`): The feedback ID the service is to delete. You can obtain the `feedback_id` from the output of the `GET /v1/feedback/{feedback_id}` method.

An example command is:

```bash
curl -X DELETE -u "apikey:{apikey}" https://{cluster_CA_domain}/{deployment_name}/compare-and-comply/api/v1/feedback/5206038a-5ea0-4f48-bee1-0780c56c53c9?version=2018-10-15
```
{: codeblock}

The service returns the following output on success:

```
{
  "status": "200",
  "message": "Successfully deleted the feedback with id  - 5206038a-5ea0-4f48-bee1-0780c56c53c9"
}
```
{: screen}
