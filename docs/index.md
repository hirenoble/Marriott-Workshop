---
layout: default
---
# Excercise-1 (Hello World)

## Step 1
Setup your environment following given steps:

`https://github.com/adobeio/adobeio-documentation/blob/stage/runtime/quickstarts/setup.md`

## Step 2
**Deploy your first action**

* Copy the below code in a text editor and save it as `hello.js`

**hello.js**

```js
function main(params) {
  return { body: 'Hello ' + params.name };
}
```

**Execute below commands**

`wsk action create hello hello.js --web true`

`wsk action get hello --url`


**Append `?name=John` in the above URL and hit it in the browser**

`https://adobeioruntime.net/api/v1/web/<your_namespace>/default/hello?name=John`

## Step 3
**Debug your action**

`wsk activation list`

`wsk activation get d773ba181d5a4ca4b3ba181d5abca472`

# Excercise-2 (Adobe I/O Events Webhook)

## Step 1
**Deploy your action**

* Copy the below code in a text editor and save it as `webhook.js`

```js
function main(args) {

    if (args.challenge)
        return {
            statusCode: 200,
            body: args.challenge
        };

    console.log("Trigger message payload: "+JSON.stringify(args));
    var mcId = args.event["com.adobe.mcloud.pipeline.pipelineMessage"]["com.adobe.mcloud.protocol.trigger"].mcId;
    
    return {body:mcId};
    
}
```

**Execute below commands**

`wsk action create webhook webhook.js --web true`

`wsk action get webhook --url`

* Copy and paste the above URL in Adobe I/O Console integration webhook registration for Adobe I/O Events.

## Step 2
**Debug your action**

`wsk activation list`

`wsk activation get d773ba181d5a4ca4b3ba181d5abca472`


# Excercise 3 (Data Source in Voyager)

## Step 1

**Deploy your action**
* Copy the below code in a text editor and save it as `config.js`

```js
module.exports = {
    credentials: {
        api_key: <api_key>,
        technical_account_id: "<your_id>@techacct.adobe.com",
        client_secret: "<secret>",
        org_id: "<your_org>@AdobeOrg",
        private_key: <your_private_key>,
        meta_scopes: "https://ims-na1.adobelogin.com/s/ent_campaign_sdk",
        tenant: "adobeiosolutionsdemo",
        transactionalAPI: "mcadobeiosolutionsdemo",
        eventID: "EVTAAMValue"
    }
}

```

* Copy the below code in a text editor and save it as `package.json`
```json
{
  "name": "Adobe Runtime action",
  "version": "1.0.0",
  "main": "./app.js",
  "scripts": {
    "start": "node app.js"
  }
}

```

* Copy the below code in a text editor and save it as `app.js`
```js
var request = require('request');
var jwt = require('jsonwebtoken');
var _config = require('./config.js');

var api_key;

function main(params) {


    var firstName = params.body.xdmEntity.person.name.firstName,
        lastName = params.body.xdmEntity.person.name.lastName,
        email = params.body.xdmEntity.personalEmail.address;

    if (firstName !== undefined && lastName !== undefined && email !== undefined) {
        return new Promise(function (resolve, reject) {
            generateToken(_config, function (err, token) {

                if (err)
                    reject(err);


                var tenant = _config.credentials.tenant;
                var transactionalAPI = _config.credentials.transactionalAPI;
                var eventID = _config.credentials.eventID;

                console.log("Access token generated successfully, trying to call Campaign API");

                var options = {
                    method: 'POST',
                    url: 'https://mc.adobe.io/' + tenant + '/campaign/' + transactionalAPI + '/' + eventID,
                    headers: {
                        'Content-Type': 'application/json;charset=utf-8',
                        'Cache-Control': 'no-cache',
                        'x-api-key': api_key,
                        Authorization: 'Bearer ' + token
                    },
                    body: '{\n  "email":"' + email + '",\n  "ctx":\n  {\n  \t"firstName":"' + firstName + '",\n  \t"lastName":"' + lastName + '"\n  }\n}'
                };

                request(options, function (error, response, body) {
                    if (error) reject(error);

                    resolve({
                        body: body
                    });
                });
            });
        });

    } else {
        return ({
            statusCode: 400,
            body: 'One or more parameters are empty.'
        });
    }

}

function generateToken(_config, callback) {
    api_key = _config.credentials.api_key;
    var technical_account_id = _config.credentials.technical_account_id;
    var org_id = _config.credentials.org_id;
    var client_secret = _config.credentials.client_secret;
    var private_key = _config.credentials.private_key;
    var meta_scopes = _config.credentials.meta_scopes.split(',');
    var access_token = "";

    var aud = "https://ims-na1.adobelogin.com/c/" + api_key;

    var jwtPayload = {
        "exp": Math.round(87000 + Date.now() / 1000),
        "iss": org_id,
        "sub": technical_account_id,
        "aud": aud
    };

    for (var i = 0; i < meta_scopes.length; i++) {
        if (meta_scopes[i].indexOf("https") > -1) {
            jwtPayload[meta_scopes[i]] = true;
        } else {
            jwtPayload["https://ims-na1.adobelogin.com/s/" + meta_scopes[i]] = true;
        }

    }
    jwt.sign(jwtPayload, private_key, {
        algorithm: 'RS256'
    }, function (err, token) {
        if (err)
            callback(err, undefined);

        console.log("JWT Token generated, trying to get access token");
        var accessTokenOptions = {
            uri: 'https://ims-na1.adobelogin.com/ims/exchange/jwt/',
            headers: {
                'content-type': 'multipart/form-data',
                'cache-control': 'no-cache'
            },
            formData: {
                client_id: api_key,
                client_secret: client_secret,
                jwt_token: token
            }
        };

        request.post(accessTokenOptions, function (err, res, body) {
            if (err) {
                console.log("Error:" + err);
                callback(err, undefiend);
            }

            if (JSON.parse(body).access_token) {
                access_token = JSON.parse(body).access_token;
                callback(undefined, access_token);
            }
        });
    });
}



exports.main = main;

```

**Execute below commands**

`zip -r action.zip .`
`wsk action create voyager --kind nodejs:6 action.zip --web true`
`wsk action get webhook --url`


## Step 2
**Debug your action**

`wsk activation list`

`wsk activation get d773ba181d5a4ca4b3ba181d5abca472`





