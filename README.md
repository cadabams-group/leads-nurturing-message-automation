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
videoRecordID ( fetch a video related to the lead’s disorder )

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
