---
layout: default
---
# Instructions

## Step 1
Setup your environment following given steps: https://github.com/adobeio/adobeio-documentation/blob/stage/runtime/quickstarts/setup.md

## Step 2
**Deploy your first action**
```js
function main(params) {
  return { body: 'Hello ' + params.name };
}
```

`wsk action create hello hello.js --web true`

`wsk action get hello --url`

`https://adobeioruntime.net/api/v1/web/<your_namespace>/default/hello?name=John`






## Important URLs:

*   Center of developer universe: [https://www.google.com](https://www.google.com)
*   Adobe Developer website: [https://www.adobe.io](https://www.adobe.io)
*   Adobe Developer admin console: [https://console.adobe.io](https://console.adobe.io)


