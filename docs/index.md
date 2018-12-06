---
layout: default
---
# Instructions

## Step 1
Setup your environment following given steps:

`https://github.com/adobeio/adobeio-documentation/blob/stage/runtime/quickstarts/setup.md`

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


**Append `?name=John` in the above URL and hit it in the browser**

`https://adobeioruntime.net/api/v1/web/<your_namespace>/default/hello?name=John`

## Step 3
**Debug your action**

`wsk activation list`

`wsk activation get d773ba181d5a4ca4b3ba181d5abca472`



