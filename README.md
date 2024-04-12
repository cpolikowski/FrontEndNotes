# FrontEndNotes

## Fetch example

``` javascript
function makeRequest(){
  let url = BASE_URL + "?query0-value0";
  fetch(url)
    .then(statusCheck)
    // .then(resp ==> resp.text()) // use if data comes in text
    // .then(resp ==> resp.json()) // use if data comes in JSON
    .then(processData)
    .catch(handleError); // user friendly error-message function
}

function processData(responseData){
  // do something (build DOM, display message, etc.)
}

async function statusCheck(res)
  if (!res.ok){
    throw new Error(await res.text());
  }
  return res;
}
```

## Posting through JS/AJAX

```javascript
id("input-form").addEventListener("submit", function(e){
  e.preventDefault(); // prevent default behavior of a submit (page refresh)
  submitRequest(); // intended response function
});
```

## ExpressJS/multer example
- remember to run **npm install multer**
``` javascript
const express = require('express');
const app = express();

const multer = require(multer);
app.use(multer().none());

app.get('/', (req, res) => res.send('Hello World!'));
app.listen(3000, () => console.log('Server ready'));
```

## Node gitignore link

- https://github.com/github/gitignore/blob/main/Node.gitignore
