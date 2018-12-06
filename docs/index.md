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





