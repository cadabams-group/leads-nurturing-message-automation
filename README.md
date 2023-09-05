# leads-nurturing-message-automation
message automation workflow for leads nurtuting as built on messagebird and rasayel

Note: This workflow demonstrates the message automation on Lead stage for hospital (H) service

**`Common step` Webhook Trigger at the start of the flow**
**Webhook variables config:**
- phone (user phone number, ex: 919741120672)
- firstName (user’s first name, ex: John)
- disorder (disorder service based on mapped primary tag)
- Service (Hospital / Rehab / Consultation / Emergency)
- recordID (airtable / crm record id)
- blogRecordID ( fetch a blog related to the lead’s disorder )
- videoRecordID ( fetch a video related to the lead’s disorder )

`Step 1:`
**Send first message using http request (uses graphQL in the request body)**
```
method: "post"
url: https://api.rasayel.io/api/graphql
headers: {
  content-type: application/json
  Authorization: Bearer Mzk3NzBmYzUtNmY3My00M2IxLTgxOGYtNGIxNzhkNDAwY2RkOmV5SmhiR2NpT2lKUVV6VXhNaUo5LmV5SmtZWFJoSWpwN0ltRndjRjlwWkNJNk56SXlMQ0poWTJOdmRXNTBYMmxrSWpvM01qSjlMQ0pwWVhRaU9qRTJPRFE1TWpnMU1Ua3NJbWx6Y3lJNklrMWhhMlV1WTI5dElpd2lhblJwSWpvaU16azNOekJtWXpVdE5tWTNNeTAwTTJJeExUZ3hPR1l0TkdJeE56aGtOREF3WTJSa0lpd2lZWFZrSWpwYkluSmxZV1JmZDNKcGRHVWlYWDAuc1JETUxkWlc3Rk1uUW5Xam5VT3Z6OEN6N202SFlsZWNfZUVIZHY3a1M4dkxwUnNMUkVWREFkLVVqOVdiOGc1b00xVHBBdmx5d3I2RjR6MnV6UU9XaVFkaFg4UjJIaTRoMHYtSEkyQ0R4bTVDdXd1UFMwRXN2bXlNcm9qUDE2V1p0Y3pkczNpaVg4cjVFQ0l4X0Q5a2I4UjdrTFROV3JqTVlnUWpqYjI0N3JFSmdENU9KS2dMUURxWWpaODhiRlc0U3E2RWpQTG5ZVjZJYVE3bUUxRktaTVdqbG1tMVZqV0pCMWdtNHBYQ3hwdFZEa1ZOWWFxQl9SZFVCVldPQzFPalhFbEF6WjdwSTNJTkQ2NGxUQU01dlMyYklZendobEJEUm1abnFXQ2ppdVVXZExXWjEtX05BaVZwdjNLQkFQRTBhclZuenVxTG9rSUgxb2xVZFpfUGFR
}
```
**body:**
```json
{
"query": "mutation ($input: MessageProactiveTemplateCreateInput!) {\ntemplateProactiveMessageCreate(input: $input) {\nmessage {\ncreatedAt\ncreatedAtCursor\ndeletedAt\ndeliveredAt\ndirection\nfailedAt\nforwarded\nid\nidCursor\nidempotencyKey\nmessageCategory\nmessageSubtype\nmessageType\nmodifiedAt\nreactions\nreadAt\nreceivedAt\nrepliedTo\nsentAt\nupdatedAt\nuuid\n}\n}\n}",
"variables": {
  "input": {
    "channelId": 2054,
    "receiver": "﻿phone﻿",
    "messageTemplateId": 29120,
    "components": [
      {
        "type": "BODY",
        "parameters": [
          {
            "type": "TEXT",
            "text": "﻿firstName﻿"
          }
        ]
      }
    ]
  }
}
```

## 5 step module (Text Message)
`step 1:` **wait**
wait for a certain predefined period of time

`step 2:` **Fetch Variables**
Make an HTTP Request to fetch variables from an external source. Please make sure to respond with a JSON object with string values only
example request as configured in messagebird:
```
url: "https://api.airtable.com/v0/appimsA1vyl94iRP7/tbla4RqbY2Ok55N4N/"
method: 'get'
header: {
  Authorization: Bearer patfzN3aWCFozivPW.56e0a2adb31d34515db153108d86b0d444e290def14e9e58267cd86976428b8b
}
```
fields to fetch in the response:
- leadStage
- phone
- firstName
- disorder
- service

`step 3:` **Check the leadStage of the lead (condition check)**
```
if(leadStage === "Lead"){
  // continue with the flow ✅
} else {
  // end of flow ❌
}
```

`step 4:` **Send message using HTTP request**
```
method: "post"
url: https://api.rasayel.io/api/graphql
headers: {
  content-type: application/json
  Authorization: Bearer Mzk3NzBmYzUtNmY3My00M2IxLTgxOGYtNGIxNzhkNDAwY2RkOmV5SmhiR2NpT2lKUVV6VXhNaUo5LmV5SmtZWFJoSWpwN0ltRndjRjlwWkNJNk56SXlMQ0poWTJOdmRXNTBYMmxrSWpvM01qSjlMQ0pwWVhRaU9qRTJPRFE1TWpnMU1Ua3NJbWx6Y3lJNklrMWhhMlV1WTI5dElpd2lhblJwSWpvaU16azNOekJtWXpVdE5tWTNNeTAwTTJJeExUZ3hPR1l0TkdJeE56aGtOREF3WTJSa0lpd2lZWFZrSWpwYkluSmxZV1JmZDNKcGRHVWlYWDAuc1JETUxkWlc3Rk1uUW5Xam5VT3Z6OEN6N202SFlsZWNfZUVIZHY3a1M4dkxwUnNMUkVWREFkLVVqOVdiOGc1b00xVHBBdmx5d3I2RjR6MnV6UU9XaVFkaFg4UjJIaTRoMHYtSEkyQ0R4bTVDdXd1UFMwRXN2bXlNcm9qUDE2V1p0Y3pkczNpaVg4cjVFQ0l4X0Q5a2I4UjdrTFROV3JqTVlnUWpqYjI0N3JFSmdENU9KS2dMUURxWWpaODhiRlc0U3E2RWpQTG5ZVjZJYVE3bUUxRktaTVdqbG1tMVZqV0pCMWdtNHBYQ3hwdFZEa1ZOWWFxQl9SZFVCVldPQzFPalhFbEF6WjdwSTNJTkQ2NGxUQU01dlMyYklZendobEJEUm1abnFXQ2ppdVVXZExXWjEtX05BaVZwdjNLQkFQRTBhclZuenVxTG9rSUgxb2xVZFpfUGFR
}
```
**body:**
```json
{
"query": "mutation ($input: MessageProactiveTemplateCreateInput!) {\ntemplateProactiveMessageCreate(input: $input) {\nmessage {\ncreatedAt\ncreatedAtCursor\ndeletedAt\ndeliveredAt\ndirection\nfailedAt\nforwarded\nid\nidCursor\nidempotencyKey\nmessageCategory\nmessageSubtype\nmessageType\nmodifiedAt\nreactions\nreadAt\nreceivedAt\nrepliedTo\nsentAt\nupdatedAt\nuuid\n}\n}\n}",
"variables": {
  "input": {
    "channelId": 2054,
    "receiver": "﻿phone﻿",
    "messageTemplateId": 29120,
    "components": [
      {
        "type": "BODY",
        "parameters": [
          {
            "type": "TEXT",
            "text": "﻿firstName﻿"
          }
        ]
      }
    ]
  }
}
```

`step 5:` **Success! End of Module 🎉**


## 7 Step Module (Multimedia Message)
`step 1:` **wait**
wait for a certain predefined period of time

`step 2:` **Fetch Variables**
Make an HTTP Request to fetch variables from an external source. Please make sure to respond with a JSON object with string values only
example request as configured in messagebird:
```
url: "https://api.airtable.com/v0/appimsA1vyl94iRP7/tbla4RqbY2Ok55N4N/"
method: 'get'
header: {
  Authorization: Bearer patfzN3aWCFozivPW.56e0a2adb31d34515db153108d86b0d444e290def14e9e58267cd86976428b8b
}
```
fields to fetch in the response:
- leadStage
- phone
- firstName
- disorder
- service

`step 3:` **Check the leadStage of the lead (condition check)**
```
if(leadStage === "Lead"){
  // continue with the flow ✅
} else {
  // end of flow ❌
}
```

`step 4:` **Fetch Variables (Blog Resources)**
Make an HTTP Request to fetch variables from an external source. Please make sure to respond with a JSON object with string values only
example request as configured in messagebird:
```
url: "https://api.airtable.com/v0/appimsA1vyl94iRP7/tbllxf0Sl5qlcqPM7/﻿"
method: 'get'
header: {
  Authorization: Bearer patfzN3aWCFozivPW.56e0a2adb31d34515db153108d86b0d444e290def14e9e58267cd86976428b8b
}
```
fields to fetch in the response:
- blogTitle 1
- blogLink 1
- blogTitle 2
- blogLink 2
...

`step 5:` **Fetch Variables (Video Resources)**
Make an HTTP Request to fetch variables from an external source. Please make sure to respond with a JSON object with string values only
example request as configured in messagebird:
```
url: "https://api.airtable.com/v0/appimsA1vyl94iRP7/tblZ3NCDWiwvpgBGI/﻿"
method: 'get'
header: {
  Authorization: Bearer patfzN3aWCFozivPW.56e0a2adb31d34515db153108d86b0d444e290def14e9e58267cd86976428b8b
}
```
fields to fetch in the response:
- videoTitle 1
- videoLink 1
- videoTitle 2
- videoLink 2
...

`step 6:` **Send message using HTTP request**
```
method: "post"
url: https://api.rasayel.io/api/graphql
headers: {
  content-type: application/json
  Authorization: Bearer Mzk3NzBmYzUtNmY3My00M2IxLTgxOGYtNGIxNzhkNDAwY2RkOmV5SmhiR2NpT2lKUVV6VXhNaUo5LmV5SmtZWFJoSWpwN0ltRndjRjlwWkNJNk56SXlMQ0poWTJOdmRXNTBYMmxrSWpvM01qSjlMQ0pwWVhRaU9qRTJPRFE1TWpnMU1Ua3NJbWx6Y3lJNklrMWhhMlV1WTI5dElpd2lhblJwSWpvaU16azNOekJtWXpVdE5tWTNNeTAwTTJJeExUZ3hPR1l0TkdJeE56aGtOREF3WTJSa0lpd2lZWFZrSWpwYkluSmxZV1JmZDNKcGRHVWlYWDAuc1JETUxkWlc3Rk1uUW5Xam5VT3Z6OEN6N202SFlsZWNfZUVIZHY3a1M4dkxwUnNMUkVWREFkLVVqOVdiOGc1b00xVHBBdmx5d3I2RjR6MnV6UU9XaVFkaFg4UjJIaTRoMHYtSEkyQ0R4bTVDdXd1UFMwRXN2bXlNcm9qUDE2V1p0Y3pkczNpaVg4cjVFQ0l4X0Q5a2I4UjdrTFROV3JqTVlnUWpqYjI0N3JFSmdENU9KS2dMUURxWWpaODhiRlc0U3E2RWpQTG5ZVjZJYVE3bUUxRktaTVdqbG1tMVZqV0pCMWdtNHBYQ3hwdFZEa1ZOWWFxQl9SZFVCVldPQzFPalhFbEF6WjdwSTNJTkQ2NGxUQU01dlMyYklZendobEJEUm1abnFXQ2ppdVVXZExXWjEtX05BaVZwdjNLQkFQRTBhclZuenVxTG9rSUgxb2xVZFpfUGFR
}
```
**body:**
```json
{
  "query": "mutation ($input: MessageProactiveTemplateCreateInput!) {\ntemplateProactiveMessageCreate(input: $input) {\nmessage {\ncreatedAt\ncreatedAtCursor\ndeletedAt\ndeliveredAt\ndirection\nfailedAt\nforwarded\nid\nidCursor\nidempotencyKey\nmessageCategory\nmessageSubtype\nmessageType\nmodifiedAt\nreactions\nreadAt\nreceivedAt\nrepliedTo\nsentAt\nupdatedAt\nuuid\n}\n}\n}",
  "variables": {
    "input": {
      "channelId": 2054,
      "receiver": "﻿phone﻿",
      "messageTemplateId": 29128,
      "components": [
        {
          "type": "BODY",
          "parameters": [
            {
              "type": "TEXT",
              "text": "﻿firstName﻿"
            },
            {
              "type": "TEXT",
              "text": "*﻿blogTitle6﻿*"
            },
            {
              "type": "TEXT",
              "text": "﻿blogLink6﻿"
            },
            {
              "type": "TEXT",
              "text": "*﻿videoTitle6﻿*"
            },
            {
              "type": "TEXT",
              "text": "﻿videoLink6﻿"
            }
          ]
        }
      ]
    }
  }
}
```

`step 7:` **Success! End of Module 🎉**
