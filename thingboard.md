# ThingBoard

{provision key, secret, \[device name, credentials]}

device provisioning request (Request) { "deviceName": "DEVICE\_NAME", "provisionDeviceKey": "YOUR\_PROVISION\_KEY\_HERE", "provisionDeviceSecret": "YOUR\_PROVISION\_SECRET\_HERE" }

device provisioning response (Response) { "provisionDeviceStatus":"SUCCESS",\
"credentialsType":"ACCESS\_TOKEN", "accessToken":"sLzc0gDAZPkGMzFVTyUY" }

jweSlmwDTNMwUOFxVjf5

{ "provisionStatus": "SUCCESS" }

px7QTV7TyXvETDdLA0es

a635e320-fcbd-11eb-8d1e-6b0bbeca40ba

active lastActivityTime createdTime name type label

{ "cmdId": 1, "data": null, "update": \[ { "entityId": { "entityType": "DEVICE", "id": "a635e320-fcbd-11eb-8d1e-6b0bbeca40ba" }, "latest": { "TIME\_SERIES": { "temperature": { "ts": 1628919534445, "value": "30" } } }, "timeseries": null } ], "errorCode": 0, "errorMsg": null, "allowedEntities": 10000, "cmdUpdateType": "ENTITY\_DATA" }

Value type String Numeric Boolean Datetime

ThingsBoard support this:

HTTP:

curl -v -X POST -d "{"temperature": 25}" $THINGSBOARD\_HOST\_NAME/api/v1/$ACCESS\_TOKEN/telemetry --header "Content-Type:application/json"

MQTT:

mosquitto\_pub -d -q 1 -h "$THINGSBOARD\_HOST\_NAME" -p "1883" -t "v1/devices/me/telemetry" -u "$ACCESS\_TOKEN" -m {"temperature":25}

v1/devices/me/telemetry

v1/devices/me/attributes

v1/devices/me/attributes/request/$request\_id



v1/devices/me/rpc/request/+

v1/devices/me/rpc/response/$request\_id



v1/devices/me/rpc/request/$request\_id
