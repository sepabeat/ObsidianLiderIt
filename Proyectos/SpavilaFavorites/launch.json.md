```json
{

    "version": "0.2.0",

    "configurations": [

        {

            "name": "CDX TenantSalva",

            "type": "al",

            "request": "launch",

            "environmentName": "DesarrolloArruzafa",

            "server": "https://businesscentral.dynamics.com",

            //"serverInstance": "sandbox",

            "tenant": "CRMbc727119.onmicrosoft.com",

            "launchBrowser": true,

            "authentication": "AAD",

            "startupObjectId": 22,

            "startupObjectType": "Page",

            "breakOnError": true,

            "schemaUpdateMode": "ForceSync"

        },

        /* {

            "name": "DesarrolloVS",

            "type": "al",

            "request": "launch",

            "environmentName": "DesarrolloVS",

            "startupObjectId": 27,

            "breakOnError": "All",

            "breakOnRecordWrite": "None",

            "launchBrowser": true,

            "enableSqlInformationDebugger": true,

            "tenant": "c9666b57-edd4-450d-b07c-a7140590aaca",

            "enableLongRunningSqlStatements": true,

            "longRunningSqlStatementsThreshold": 500,

            "numberOfSqlStatements": 10,

            "schemaUpdateMode": "ForceSync"

        }, */

    ]

}
```
