Communication with the outside
==============

Konnectors communicate with the stack via its stdout. Each line is parsed by the stack as JSON.

When an error is thrown by the konnector, it is catched and translated to JSON.

### Message types

This is the list of error codes that your konnector can `throw` and which will be translated by the `collect` application.

Example :

```
const login = function () {
  throw new Error('LOGIN_FAILED')
}
```

`Collect` will then signal to the user that the credentials used are not correct.

|Error code|Meaning|
|---|---|
|`LOGIN_OK`|The konnector has logged in|
|`LOGIN_FAILED`|The konnector could not login|
|`NOT_EXISTING_DIRECTORY`|The folder specified as folder_to_save does not exist (checked by BaseKonnector)|
|`VENDOR_DOWN`|The vendor's website is down|
|`USER_ACTION_NEEDED`|The user needs to go to the vendor's website to fix something
|`UNKNOWN_ERROR`|There was an unexpected error, please take a look at the logs to know what happened|

Sentry
======

If `process.env.SENTRY_DSN` is set :

* Raven will be configured to send exception to this address
* the BaseKonnector will have its `run` method wrapped into a `Raven.context` so that when it fails, it sends the exception to Raven before exiting.

The idea being that the `process.env.SENTRY_DSN` is only set up for people having opted in for exception handling. Self-hosted instances of the cozy-stack will be free to use or not our SENTRY_DSN.
