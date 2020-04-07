---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: Services
topic: Developer guide
---

# MLServices

An MLService is a published trained model that provides your organization with the ability to access and reuse previously developed models. A key feature of MLServices is the ability to automate training and scoring on a scheduled basis. Scheduled training runs can help upkeep a model's efficiency and accuracy, while scheduled scoring runs can ensure that new insights are consistently generated.

Automated training and scoring schedules are defined with a starting timestamp, ending timestamp, and a frequency represented as a <a href="https://en.wikipedia.org/wiki/Cron" target="_blank">cron expression</a>. Schedules can be defined when [creating an MLService](#create-an-mlservice) or applied by [updating an existing MLService](#update-an-mlservice).

## Create an MLService

You can create an MLService by performing a POST request and a payload that provides a name for the service and a valid MLInstance ID. The MLInstance used to create an MLService is not required to have existing training Experiments but you can choose to create the MLService with an existing trained model by providing the corresponding Experiment ID and training run ID.

**API Format**

```http
POST /mlServices
```

**Request**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/mlServices \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json; profile=mlService.v1.json' \
    -d '{
        "name": "A name for this MLService",
        "description": "A description for this MLService",
        "mlInstanceId": "{MLINSTANCE_ID}",
        "trainingDataSetId": "{DATASET_ID}",
        "trainingExperimentId": "{TRAINING_ID}",
        "trainingExperimentRunId": "{RUN_ID}",
        "trainingSchedule": {
            "startTime": "2019-01-01T00:00",
            "endTime": "2019-12-31T00:00",
            "cron": "20 * * * *"
        },
        "scoringSchedule": {
            "startTime": "2019-01-01T00:00",
            "endTime": "2019-12-31T00:00",
            "cron": "20 * * * *"
        }
    }'
```

| Property | Description |
| --- | --- |
| `name` | The desired name for the MLService. The service corresponding to this MLService will inherit this value to be displayed in the Service Gallery UI as the service's name. |
| `description` | An optional description for the MLService. The service corresponding to this MLService will inherit this value to be displayed in Service Gallery UI as the service's description. |
| `mlInstanceId` | A valid MLInstance ID. |
| `trainingDataSetId` | A training dataset ID which if provided will override the MLInstance's default dataset ID. If the MLInstance used to create the MLService does not define a training dataset, you must provide an appropriate training dataset ID. |
| `trainingExperimentId` | An Experiment ID which you can optionally provide. If this value is not provided then creating the MLService will also create a new Experiment using the MLInstance's default configurations. |
| `trainingExperimentRunId` | A training run ID which you can optionally provide. If this value is not provided then creating the MLService will also create and execute a training run using the MLInstance's default training parameters. |
| `trainingSchedule` | A schedule for automated training runs. If this property is defined, then the MLService will automatically perform training runs on a scheduled basis. |
| `trainingSchedule.startTime` | A timestamp for which scheduled training runs will begin. |
| `trainingSchedule.endTime` | A timestamp for which scheduled training runs will end. |
| `trainingSchedule.cron` | A cron expression that defines the frequency of automated training runs. |
| `scoringSchedule` | A schedule for automated scoring runs. If this property is defined, then the MLService will automatically perform scoring runs on a scheduled basis. |
| `scoringSchedule.startTime` | A timestamp for which scheduled scoring runs will begin. |
| `scoringSchedule.endTime` | A timestamp for which scheduled scoring runs will end. |
| `scoringSchedule.cron` | A cron expression that defines the frequency of automated scoring runs. |

**Response**

A successful response returns a payload containing the details of the newly created MLService including its unique identifier (`id`), Experiment ID for training (`trainingExperimentId`), Experiment ID for scoring (`scoringExperimentId`), and the input training dataset ID (`trainingDataSetId`).

```json
{
    "id": "{MLSERVICE_ID}",
    "name": "A name for this MLService",
    "description": "A description for this MLService",
    "mlInstanceId": "{MLINSTANCE_ID}",
    "trainingExperimentId": "{TRAINING_ID}",
    "trainingDataSetId": "{DATASET_ID}",
    "scoringExperimentId": "{SCORING_ID}",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "trainingSchedule": {
        "startTime": "2019-01-01T00:00",
        "endTime": "2019-12-31T00:00",
        "cron": "20 * * * *"
    },
    "scoringSchedule": {
        "startTime": "2019-01-01T00:00",
        "endTime": "2019-12-31T00:00",
        "cron": "20 * * * *"
    },
    "updated": "2019-01-01T00:00:00.000Z"
}
```

## Retrieve a list of MLServices

You can retrieve a list of MLServices by performing a single GET request. To help filter results, you can specify query parameters in the request path. For a list of available queries, refer to the appendix section on [query parameters for asset retrieval](./appendix.md#query).

**API Format**

```http
GET /mlServices
GET /mlServices?{QUERY_PARAMETER}={VALUE}
GET /mlServices?{QUERY_PARAMETER_1}={VALUE_1}&{QUERY_PARAMETER_2}={VALUE_2}
```

| Parameter | Description |
| --- | --- |
| `{QUERY_PARAMETER}` | One of the [available query parameters](./appendix.md#query) used to filter results. |
| `{VALUE}` | The value for the preceding query parameter. |

**Request**

The following request contains a query and retrieves a list of MLServices sharing the same MLInstance ID (`{MLINSTANCE_ID}`).

```shell
curl -X GET \
    'https://platform.adobe.io/data/sensei/mlServices?property=mlInstanceId=={MLINSTANCE_ID}' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

A successful response returns a list of MLServices and their details including their MLService ID (`{MLSERVICE_ID}`), Experiment ID for training (`{TRAINING_ID}`), Experiment ID for scoring (`{SCORING_ID}`), and the input training dataset ID (`{DATASET_ID}`).

```json
{
    "children": [
        {
            "id": "{MLSERVICE_ID}",
            "name": "A service created in UI",
            "mlInstanceId": "{MLINSTANCE_ID}",
            "trainingExperimentId": "{TRAINING_ID}",
            "trainingDataSetId": "{DATASET_ID}",
            "scoringExperimentId": "{SCORING_ID}",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "displayName": "Jane Doe",
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-01T00:00:00.000Z"
        }
    ],
    "_page": {
        "property": "mlInstanceId=={MLINSTANCE_ID},deleted==false",
        "count": 1
    }
}
```

## Retrieve a specific MLService

You can retrieve the details of a specific Experiment by performing a GET request that includes the desired MLService's ID in the request path.

**API Format**

```http
GET /mlServices/{MLSERVICE_ID}
```

*   `{MLSERVICE_ID}`: A valid MLService ID.

**Request**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/mlServices/{MLSERVICE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

A successful response returns a payload containing the details of the requested MLService.

```json
{
    "id": "{MLSERVICE_ID}",
    "name": "A name for this MLService",
    "description": "A description for this MLService",
    "mlInstanceId": "{MLINSTANCE_ID}",
    "trainingExperimentId": "{TRAINING_ID}",
    "trainingDataSetId": "{DATASET_ID}",
    "scoringExperimentId": "{SCORING_ID}",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z"
}
```

## Update an MLService

You can update an existing MLService by overwriting its properties through a PUT request that includes the target MLService's ID in the request path and providing a JSON payload containing updated properties.

>[!TIP] In order to ensure the success of this PUT request, it is suggested that first you perform a GET request to [retrieve the MLService by ID](#retrieve-a-specific-mlservice). Then, modify and update the returned JSON object and apply the entirety of the modified JSON object as the payload for the PUT request.

**API Format**

```http
PUT /mlServices/{MLSERVICE_ID}
```

*   `{MLSERVICE_ID}`: A valid MLService ID.

**Request**

```shell
curl -X PUT \
    https://platform.adobe.io/data/sensei/mlServices/{MLSERVICE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json; profile=mlService.v1.json' \
    -d '{
        "name": "A name for this MLService",
        "description": "A description for this MLService",
        "mlInstanceId": "{MLINSTANCE_ID}",
        "trainingExperimentId": "{TRAINING_ID}",
        "trainingDataSetId": "{DATASET_ID}",
        "scoringExperimentId": "{SCORING_ID}",
        "trainingSchedule": {
            "startTime": "2019-01-01T00:00",
            "endTime": "2019-12-31T00:00",
            "cron": "20 * * * *"
        },
        "scoringSchedule": {
            "startTime": "2019-01-01T00:00",
            "endTime": "2019-12-31T00:00",
            "cron": "20 * * * *"
        }
    }'
```

**Response**

A successful response returns a payload containing the MLService's updated details.

```json
{
    "id": "{MLSERVICE_ID}",
    "name": "A name for this MLService",
    "description": "A description for this MLService",
    "mlInstanceId": "{MLINSTANCE_ID}",
    "trainingExperimentId": "{TRAINING_ID}",
    "trainingDataSetId": "{DATASET_ID}",
    "scoringExperimentId": "{SCORING_ID}",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "trainingSchedule": {
        "startTime": "2019-01-01T00:00",
        "endTime": "2019-12-31T00:00",
        "cron": "20 * * * *"
    },
    "scoringSchedule": {
        "startTime": "2019-01-01T00:00",
        "endTime": "2019-12-31T00:00",
        "cron": "20 * * * *"
    },
    "updated": "2019-01-02T00:00:00.000Z"
}
```

## Delete an MLService

You can delete a single MLService by performing a DELETE request that includes the target MLService's ID in the request path.

**API Format**

```http
DELETE /mlServices/{MLSERVICE_ID}
```

| Parameter | Description |
| --- | --- |
| `{MLSERVICE_ID}` | A valid MLService ID. |

**Request**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/mlServices/{MLSERVICE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

```json
{
    "title": "Success",
    "status": 200,
    "detail": "MLService deletion was successful"
}
```

## Delete MLServices by MLInstance ID

You can delete all MLServices belonging to a particular MLInstance by performing a DELETE request that specifies an MLInstance ID as a query parameter.

**API Format**

```http
DELETE /mlServices?mlInstanceId={MLINSTANCE_ID}
```

| Parameter | Description |
| --- | --- |
| `{MLSERVICE_ID}` | A valid MLService ID. |

**Request**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/mlServices?mlInstanceId={MLINSTANCE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

```json
{
    "title": "Success",
    "status": 200,
    "detail": "MLServices deletion was successful"
}
```