{
    "name": "pipeline1",
    "properties": {
        "activities": [
            {
                "name": "Copy data1",
                "type": "Copy",
                "dependsOn": [],
                "policy": {
                    "timeout": "0.12:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "userProperties": [],
                "typeProperties": {
                    "source": {
                        "type": "DelimitedTextSource",
                        "storeSettings": {
                            "type": "AzureBlobStorageReadSettings",
                            "recursive": true,
                            "enablePartitionDiscovery": false
                        },
                        "formatSettings": {
                            "type": "DelimitedTextReadSettings"
                        }
                    },
                    "sink": {
                        "type": "DelimitedTextSink",
                        "storeSettings": {
                            "type": "AzureBlobStorageWriteSettings"
                        },
                        "formatSettings": {
                            "type": "DelimitedTextWriteSettings",
                            "quoteAllText": true,
                            "fileExtension": ".txt"
                        }
                    },
                    "enableStaging": false,
                    "translator": {
                        "type": "TabularTranslator",
                        "typeConversion": true,
                        "typeConversionSettings": {
                            "allowDataTruncation": true,
                            "treatBooleanAsNumber": false
                        }
                    }
                },
                "inputs": [
                    {
                        "referenceName": "blobstorage25",
                        "type": "DatasetReference"
                    }
                ],
                "outputs": [
                    {
                        "referenceName": "blobstorage25",
                        "type": "DatasetReference"
                    }
                ]
            },
            {
                "name": "Script1",
                "type": "Script",
                "dependsOn": [
                    {
                        "activity": "Copy data1",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "policy": {
                    "timeout": "0.12:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "userProperties": [],
                "typeProperties": {
                    "scripts": [],
                    "scriptBlockExecutionTimeout": "02:00:00"
                }
            },
            {
                "name": "Validation1",
                "type": "Validation",
                "dependsOn": [
                    {
                        "activity": "Script1",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "userProperties": [],
                "typeProperties": {
                    "timeout": "0.12:00:00",
                    "sleep": 10
                }
            },
            {
                "name": "Azure Data Explorer Command1",
                "type": "AzureDataExplorerCommand",
                "dependsOn": [
                    {
                        "activity": "Validation1",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "policy": {
                    "timeout": "0.12:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "userProperties": [],
                "typeProperties": {
                    "commandTimeout": "00:20:00"
                }
            }
        ],
        "annotations": [],
        "lastPublishTime": "2025-01-28T03:34:25Z"
    },
    "type": "Microsoft.DataFactory/factories/pipelines"
}
