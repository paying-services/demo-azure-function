#demo

before start read this docs

https://learn.microsoft.com/en-us/azure/logic-apps/logic-apps-securing-a-logic-app?tabs=azure-portal#authentication-types-supported-triggers-actions

![alt text](image.png)

built int connector for function app from logic app doesnt support authentication so in our case we can do something like this

* request a token from entra id  endpoint
* create a task with a post request
* parse the response from entra id 
* put the acces_token in authorization bearer header in the post



* enable authentication in the azure function paired with a app reg/entrerprise app

![alt text](image-7.png)


* create a random role in the app reg refered line above

![alt text](image-4.png)

* assign the app registration as a user of himself  with the  previous role added

![alt text](image-5.png)


```
#IF THE APP REGISTRATION USED FOR AUTHENTICATION
oidForMI="0386f90a-0bf5-48f6-bb29-3118ad4c6b51"

appName="functionappdemogabo"
serverSPOID=$(az ad sp list --filter "displayName eq '$appName'" --query '[0].id' -o tsv | tr -d '[:space:]')

roleguid1="c96e85b4-0eeb-4e0b-9017-f6b7be2c920e"

az rest -m POST -u https://graph.microsoft.com/v1.0/servicePrincipals/$oidForMI/appRoleAssignments -b "{\"principalId\": \"$oidForMI\", \"resourceId\": \"$serverSPOID\",\"appRoleId\": \"$roleguid1\"}"
```




https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-client-creds-grant-flow#get-a-token

* enSure that logic have key vault secret user in logic app´

![alt text](image-1.png)


add your secret in key vault 

![alt text](image-2.png)

dont forget to add yourself to the rbac so you can add them 

![alt text](image-3.png)


{
  "token_type": "Bearer",
  "expires_in": "3599",
  "ext_expires_in": "3599",
  "expires_on": "1709166209",
  "not_before": "1709162309",
  "resource": "00000002-0000-0000-c000-000000000000",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlhSdmtvOFA3QTNVYVdTblU3Yk05blQwTWpoQSIsImtpZCI6IlhSdmtvOFA3QTNVYVdTblU3Yk05blQwTWpoQSJ9.eyJhdWQiOiIwMDAwMDAwMi0wMDAwLTAwMDAtYzAwMC0wMDAwMDAwMDAwMDAiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC8xOGIwODAxMS0wYTY2LTRkYTAtOTdmMC1jYzNlOTU3MWM5ZTkvIiwiaWF0IjoxNzA5MTYyMzA5LCJuYmYiOjE3MDkxNjIzMDksImV4cCI6MTcwOTE2NjIwOSwiYWlvIjoiRTJOZ1lGalh2MEJyMXF1YUcwTHY1NnFMRnMzWkJ3QT0iLCJhcHBpZCI6ImIxYTE3ZGZkLWUzY2EtNDBiNi05NmY2LWYzY2ZjYTA3ZjU2YSIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzE4YjA4MDExLTBhNjYtNGRhMC05N2YwLWNjM2U5NTcxYzllOS8iLCJvaWQiOiIwMzg2ZjkwYS0wYmY1LTQ4ZjYtYmIyOS0zMTE4YWQ0YzZiNTEiLCJyaCI6IjAuQVZvQUVZQ3dHR1lLb0UyWDhNdy1sWEhKNlFJQUFBQUFBQUFBd0FBQUFBQUFBQUJhQUFBLiIsInN1YiI6IjAzODZmOTBhLTBiZjUtNDhmNi1iYjI5LTMxMThhZDRjNmI1MSIsInRlbmFudF9yZWdpb25fc2NvcGUiOiJTQSIsInRpZCI6IjE4YjA4MDExLTBhNjYtNGRhMC05N2YwLWNjM2U5NTcxYzllOSIsInV0aSI6Im9KRjBWYlZWQkVpc1ZQQU9zRk1sQUEiLCJ2ZXIiOiIxLjAifQ.PiYj0UIruZViiFNprchWQFzTQic-Un1lilIcoOvdKFu_k2Zi4HEW30sbgRjKxNuiWzYGYDss-V0IwbfwOa6SmakJxjMDwJhuOPMNa2LLmDsKWYeMzhgONXcIcpodL3LtM6l8RW5eufVIoJYlAk42mvowKV2Sw7u-jyejWbsNAaGzV3BSfqKxTs-WuNrwgG4nYCKAhfOiaRcG1TaxcvoXnHcDuH_iccQhXQd3SPAHn_Oz6Pj4bXZW_3E2DJMNK1QZO8Ebu1wa5fWrzKYsP6FgXJ2mM9gyJwyBWAaxNtXBX7YDlFJMRFhQdA7ATJIh5B9OuVfuuBLhQeLqILYMAY3rEQ"
}