![halfway there!](/Journey/050/halfway-there.jpg)

# Cloud Resume Challenge - JavaScript Counter

## Introduction

Picking up from last weekend, we're finishing the front end aspect of the project with a JavaScript visitor counter.

## Prerequisite

- Knowledge of APIs
- Experience with JavaScript

## Use Case

- Now that the CloudFront cache has been invalidated, this will allow a visitor counter to increment (and be displayed to the client) in real-time.

## Cloud Research

[what is countAPI?](/Journey/050/count-api.png)

- [This](https://countapi.xyz/), looked promising. Other, simpler solutions are available

## Try yourself

- Read the docs and choose a way of implementing
- Create a new feature branch in the source code

### Step 1 — Set up new JS file

- Create a new JavaScript file, `counter.js`
- At the bottom of `index.html`,, just before the closing `</body>` tag: `<script src="/js/counter.js"></script>`

### Step 2 — Set up counter

- Wherever you want to display the counter:

```html
<p>You are visitor #<span id="visits">0</span></p>
```

- In `counter.js`:

```js
const count = document.getElementById('visits');

updateVisitCount();

function updateVisitCount() {
  fetch('https://api.countapi.xyz/update/domainname/?amount=1')
    .then(res => res.json())
    .then(res => {
      count.innerHTML = res.value;
    });
}

// The empty string below will link to a future DynamoDB database, via API Gateway
// xhttp.open(
//   'GET',
//   '',
//   true
// );
// xhttp.send();
```

### Step 3 — Alternative counter (AWS API only)

Until I've actually created the backend with Lambda and DynamoDB I have no idea which solution works best. However, I'd like to avoid as many superfluous API calls as possible, and this seems like a better solutuion (if I can get it to work...). I'm also concerned that the API above acts like a database for the total visitor count, circumnavigating the whole backend?

```js
const count = document.getElementById('visits');

updateVisitCount();

function updateVisitCount() {
  fetch('put api gateway link here', {
    mode: 'cors',
    headers: {
      'Content-Type': 'application/json',
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET',
    },
  })
    .then(res => res.json())
    .then(res => {
      count.innerHTML = res.value;
    });
}
```

## ☁️ Cloud Outcome

- Front end is displaying a hardcoded version of the visitor counter (no database to store a total in yet)

- I have a couple of ways to link this to the future back end in `counter.js`. Hope the difference between the two makes more sense by then...

## Next Steps

It's Christmas, it's time to build the back end with SAM CLI!

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1340368336409014273?s=20)
