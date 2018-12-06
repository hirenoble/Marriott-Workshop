---
layout: default
---
# Instructions

## Step 1
Setup your environment following given steps:

https://github.com/adobeio/adobeio-documentation/blob/stage/runtime/quickstarts/setup.md

## Step 2
**Deploy your first action**
**hello.js**

```js
function main(params) {
  return { body: 'Hello ' + params.name };
}
```

**Execute below commands**

`wsk action create hello hello.js --web true`

`wsk action get hello --url`


**Append `?name=John` in the above URL**
`https://adobeioruntime.net/api/v1/web/<your_namespace>/default/hello?name=John`






## Important URLs:

* 


