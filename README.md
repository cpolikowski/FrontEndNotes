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

app.get('/', (req, res) => {
  res.send('Hello World!')
});
const PORT = process.env.PORT || 8000;
app.listen(PORT, () => {
  console.log('Server ready and running on ${PORT}');
});
```

## Lab 6
# labWork.html
``` javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src = "./script.js" defer></script>
</head>

<style>
    .error {
        color: "red;"
    }
</style>

<body>

    <h1>Email Validation</h1>

    <form id = "emailForm" name = "emailAddress">
        <label for="emailAddress">Only Siena Email address:</label><br>
        <input type = "text" id = "emailAddress">
        <span id = "emailError" class = "error"></span>
        <button type = "submit" id = "btn">Submit</button>
    </form>
</body>
</html>
```
# script.js
``` javascript
'use strict'
function validateEmail() {
    let isValid = false;
    const emailAddress = document.getElementById("emailAddress").value;
    // Get the emailAddress's input value
    document.getElementById("emailError").textContent = "";
    //set the textContent of emailError as empty string.
    
    if (emailAddress.endsWith("@siena.edu")) {
        isValid = true;
        console.log("Email is valid", emailAddress);
    } else {
        document.getElementById("emailError").textContent = "Please enter a valid email address";
    }

    if (isValid) {
        makeRequest();
    }
}
document.getElementById("emailForm").addEventListener("submit", function (e) {
    e.preventDefault();
    validateEmail();
});

function makeRequest() {
    let url = "https://macpro.csis410.com/submit";

    // creating formData object
    let params = new FormData();
    params.append("email:", document.getElementById("emailAddress").value);

    // fetching with a POST method and the param data in the body
    fetch(url, { method: "POST", body: params })
        .then(statusCheck)
        // .then((response) => response.json())
        .then(response => response.text())
        .then((data) => updateResults(data))
        .catch(handleError);
}

function updateResults(data) {
    console.log(data);
}

function handleError(error) {
    console.error(error);
}


async function statusCheck(res) {
    if (!res.ok) {
        throw new Error(await res.text());
    }
    return res;
}
```

# index.html 
``` javascript
<!DOCTYPE html>
<html>
  <head>
    <title>Fetch Dog Pics</title>
    <meta charset="UTF-8" />
    <style>
      table,
      th,
      tr,
      td {
        border: 2px solid black;
      }
    </style>
  </head>

  <body>
    <h1>I ❤️ Dogs</h1>
    <main>
      <section id="app">
        <table>
          <thead>
            <tr>
              <td>ID</td>
              <td>url</td>
              <td>Image</td>
            </tr>
          </thead>
          <tbody id="myTableBody"></tbody>
        </table>
      </section>
    </main>

    <script src="./main.js" defer></script>
  </body>
</html>
```

# main.js
``` javascript
'use strict';
function createRow(id, url) {
  const tr = document.createElement('tr');
  const td_id = document.createElement('td');
  td_id.appendChild(document.createTextNode(id));

  const td_url = document.createElement('td');
  td_url.appendChild(document.createTextNode(url));

  const td_img = document.createElement('td');

  const img = document.createElement('img');
  img.src = url;
  td_img.appendChild(img);

  tr.appendChild(td_id);
  tr.appendChild(td_url);
  tr.appendChild(td_img);
  tableBody.appendChild(tr);
}
const app = document.getElementById('app');
// Creating a new Element
const newElement = document.createTextNode('Hello, World!');
// Adding it as a Child
app.appendChild(newElement);

const tableBody = document.querySelector('tbody');
const sampleObj = {
  id: 1,
  url: 'https://images.dog.ceo/breeds/maltese/n02085936_3391.jpg',
};

createRow(1, 'https://images.dog.ceo/breeds/maltese/n02085936_3391.jpg');

// -- fetch here --
async function fetchDogPicsAsync() {
  const url = 'https://dog.ceo/api/breeds/image/random';
  const response = await fetch(url);
  let data = await response.json();
  console.log(data);
  app.appendChild(document.createTextNode(JSON.stringify(data)));
  createRow(data.id, data.message);
}
//document.addEventListener("DOMContentLoaded", fetchDogPicsAsync);

// -- fetch here --
const url = 'https://dog.ceo/api/breeds/image/random';
const response = fetch(url)
  .then((response) => response.json())
  .then(processDogData);

function processDogData(data) {
  console.log(data);
  app.appendChild(document.createTextNode(JSON.stringify(data)));

  createRow(5, data.message);
}

setInterval(fetchDogPicsAsync, 5000);
```

## Lab 7
# server.js
``` javascript
"use strict";

let categories = ['successQuotes', 'perseveranceQuotes', 'happinessQuotes'];

let successQuotes = [
  {
    'quote': 'Success is not final, failure is not fatal: It is the courage to continue that counts.',
    'author': 'Winston S. Churchill'
  },
  {
    'quote': 'The way to get started is to quit talking and begin doing.',
    'author': 'Walt Disney'
  }
];

let perseveranceQuotes = [
  {
    'quote': 'It’s not that I’m so smart, it’s just that I stay with problems longer.',
    'author': 'Albert Einstein'
  },
  {
    'quote': 'Perseverance is failing 19 times and succeeding the 20th.',
    'author': 'Julie Andrews'
  }
];

let happinessQuotes = [
  {
    'quote': 'Happiness is not something ready made. It comes from your own actions.',
    'author': 'Dalai Lama'
  },
  {
    'quote': 'For every minute you are angry you lose sixty seconds of happiness.',
    'author': 'Ralph Waldo Emerson'
  }
];


const express = require("express");
const app = express();
const port = process.env.PORT || 8000;

app.get("/", (req,res) => {
    res.send("Hello World");
});

app.get("/quotebook/categories", (req,res) => {
    res.type("text");
    let myAnswer = "";
    for(let i = 0; i < categories.length; i++) {
        let temp = "A possible category is " + categories[i] + "\n";
        myAnswer = myAnswer + temp;
    }
    res.send(myAnswer);
});

app.get("/quotebook/quote/:category", (req, res) => {
  res.type("JSON");
  let cat = req.params.category;
  const index = Math.floor(Math.random(2) * 2);
  console.log(cat);
  if (cat == "successQuotes"){
    res.send(successQuotes[index]);
  }else if(cat == "perseveranceQuotes"){
    res.send(perseveranceQuotes[index]);
  }else if(cat == "happinessQuotes"){
    res.send(happinessQuotes[index]);
  } else {
    res.type('text').status(400).send('Error, Bad Request!');
  }

});

// 1.3 skipping for nowc
app.get("/quotebook/quote/:category", (req, res) => {
  res.type("text");

});

app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
});
```

## Lab 9
# add_expense.html (public)
``` javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Add Expense</title>
</head>
<body>
    <h2>Add New Expense</h2>
    <form id="expenseForm">
        <label for="title">Title:</label><br>
        <input type="text" id="title" name="title"><br>
        <label for="category">Category:</label><br>
        <input type="text" id="category" name="category"><br>
        <label for="amount">Amount:</label><br>
        <input type="text" id="amount" name="amount"><br>
        <label for="type">Type:</label><br>
        <select id="type" name="type">
            <option value="Debit">Debit</option>
            <option value="Credit">Credit</option>
        </select><br>
        <label for="date">Date:</label><br>
        <input type="date" id="date" name="date"><br><br>
        <input type="submit" value="Submit">
    </form>

    <script>
        document.getElementById("expenseForm").addEventListener("submit", function(event) {
            event.preventDefault();
            
            const formData = new FormData(this);
            const expenseData = {};
            formData.forEach(function(value, key){
                expenseData[key] = value;
            });
            
            fetch('/add-expense', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(expenseData)
            })
            .then(response => response.json())
            .then(data => {
                alert(data.message);
                document.getElementById("expenseForm").reset();
            })
            .catch(error => console.error('Error adding expense:', error));
        });
    </script>
</body>
</html>
```

# index.html (public)
``` javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
    <title>Document</title>
</head>
<body>

    <div class="container">
        <h2>Current Finances</h2>
        <p id="updateTime"></p>
        <table class="table">
          <thead class="thead-light">
            <tr>
              <th>Title</th>
              <th>Category</th>
              <th>Amount</th>
              <th>Type</th>
              <th>Date</th>
            </tr>
          </thead>
          <tbody id="financeData">
            <!-- Finance data will be dynamically inserted here -->
          </tbody>
        </table>
      </div>
    
</body>

<script>
    function updateCurrentTime() {
        // Just updates the time on webpage.
        const now = new Date();
        const currentTime =
            now.getHours() + ":" + now.getMinutes() + ":" + now.getSeconds();
        document.getElementById("updateTime").innerText =
            "Last Updated: " + currentTime;
    }

    function fetchFinanceData() {
        fetch('/finance-data')
            .then(response => response.json())
            .then(data => {
                updateFinanceTable(data);
                updateCurrentTime();
            })
            .catch(error => console.error('Error fetching finance data:', error));
    }

    function updateFinanceTable(data) {
        const financeDataElement = document.getElementById("financeData");
        financeDataElement.innerHTML = ""; // Clear existing data
        data.forEach((item) => {
            const tr = document.createElement("tr");

            const tdTitle = document.createElement("td");
            const tdCategory = document.createElement("td");
            const tdAmount = document.createElement("td");
            const tdType = document.createElement("td");
            const tdDate = document.createElement("td");

            const tnTitle = document.createTextNode(item.title);
            const tnCategory = document.createTextNode(item.category);
            const tnAmount = document.createTextNode(item.amount);
            const tnType = document.createTextNode(item.type);
            const tnDate = document.createTextNode(item.date);

            tdTitle.appendChild(tnTitle);
            tdCategory.appendChild(tnCategory);
            tdAmount.appendChild(tnAmount);
            tdType.appendChild(tnType);
            tdDate.appendChild(tnDate);

            tr.appendChild(tdTitle);
            tr.appendChild(tdCategory);
            tr.appendChild(tdAmount);
            tr.appendChild(tdType);
            tr.appendChild(tdDate);

            financeDataElement.appendChild(tr);
        });
    }

    // Run it
    setInterval(fetchFinanceData, 5000);
    fetchFinanceData(); // Fetch data initially
</script>

</html>
```

# server.js 
``` javascript
'use strict';

const express = require("express");
const sqlite3 = require("sqlite3").verbose();

const app = express();
const port = process.env.PORT || 8000;

app.use('/public', express.static('public'));
app.use(express.urlencoded({ extended: true }));
app.use(express.json());

// Connect to SQLite database
const db = new sqlite3.Database('expenses.db');

// Create expenses table if not exists
db.run(`
    CREATE TABLE IF NOT EXISTS expenses (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        title TEXT,
        category TEXT,
        amount REAL,
        type TEXT,
        date TEXT
    )
`);

// Endpoint to return all expenses from the database
app.get('/finance-data', (req, res) => {
    db.all('SELECT * FROM expenses', (err, rows) => {
        if (err) {
            console.error('Error fetching finance data:', err);
            res.status(500).json({ message: 'Error fetching finance data' });
        } else {
            res.json(rows);
        }
    });
});

// POST endpoint to add new expense to the database
app.post('/add-expense', (req, res) => {
    const newExpense = req.body;
    db.run(`
        INSERT INTO expenses (title, category, amount, type, date) 
        VALUES (?, ?, ?, ?, ?)
    `, [newExpense.title, newExpense.category, newExpense.amount, newExpense.type, newExpense.date], function(err) {
        if (err) {
            console.error('Error adding expense:', err);
            res.status(500).json({ message: 'Error adding expense' });
        } else {
            res.json({ message: 'Expense added successfully!', expense: newExpense, expenseId: this.lastID });
        }
    });
});

app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});
```

## Lab 10
# invoices.html (public)
``` javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Invoice Items Display</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: #f4f4f4;
        }

        .container {
            width: 80%;
            margin: 20px auto;
        }

        .invoice-item {
            background: #fff;
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 15px;
            margin-bottom: 10px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .invoice-item h2 {
            margin-top: 0;
        }

        .invoice-item p {
            margin: 5px 0;
        }
    </style>
</head>

<body>
    <div class="container" id="invoice-items-container">
        <div class="invoice-item">
            <h2>Static Item</h2>
            <p>Invoice ID: 100</p>
            <p>Track ID: 345</p>
            <p>Unit Price: $free.99</p>
            <p>Quantity: 1</p>
        </div>
        <!-- Invoice items will be inserted here -->
    </div>

    <script>
        // Demo data array
        let invoiceItemsData = [
            {
                InvoiceItemId: 1,
                InvoiceId: 101,
                TrackId: 345,
                UnitPrice: 1.99,
                Quantity: 1,
            },
            {
                InvoiceItemId: 2,
                InvoiceId: 102,
                TrackId: 346,
                UnitPrice: 0.99,
                Quantity: 3,
            },
            {
                InvoiceItemId: 3,
                InvoiceId: 103,
                TrackId: 347,
                UnitPrice: 1.49,
                Quantity: 2,
            },
        ];

        // Function to display invoice items
        function displayInvoiceItems(item) {
            const container = document.getElementById("invoice-items-container");
            for (item of invoiceItemsData){

                // create a new div
                let invoice = document.createElement('div');
                invoice.classList.add("invoice-item")

                let header = document.createElement('h2');
                header.innerText = "InvoiceId: " + item.InvoiceId;
                invoice.appendChild(header);

                let paraAdd = document.createElement('p');
                paraAdd.innerText = "Billing Address: " + item.BillingAddress;
                invoice.appendChild(paraAdd);

                let paraCity = document.createElement('p');
                paraCity.innerText = "Billing City: " + item.BillingCity;
                invoice.appendChild(paraCity);

                let paraCountry = document.createElement('p');
                paraCountry.innerText = "Billing Country: " + item.BillingCountry;
                invoice.appendChild(paraCountry);

                let paraCode = document.createElement('p');
                paraCode.innerText = "Billing Postal Code: " + item.BillingPostalCode;
                invoice.appendChild(paraCode);

                let paraState = document.createElement('p');
                paraState.innerText = "Billing State: " + item.BillingState;
                invoice.appendChild(paraState);

                let custId = document.createElement('p');
                custId.innerText = "Customer ID: " + item.CustomerId;
                invoice.appendChild(custId);

                let invoiceDate = document.createElement('p');
                invoiceDate.innerText = "Invoice Date: " + item.InvoiceDate;
                invoice.appendChild(invoiceDate);

                let total = document.createElement('p');
                total.innerText = "Total: " + item.Total;
                invoice.appendChild(total);

                container.appendChild(invoice);
            }
        }

        // Call displayInvoiceItems on page load with demo data
        window.onload = () => {
            fetch('/invoice_items')
            .then(data => data.json())
            .then(data => {
                invoiceItemsData = data;
                displayInvoiceItems(data);
            });
        };
    </script>
</body>

</html>
```

# server.js
``` javascript
'use strict'
const express = require("express");
const sqlite = require("sqlite");
const sqlite3 = require("sqlite3");

const app = express();

app.get('/', (req, res) => {
    res.send("Hello World");
});

async function getDBConnection() {
    const db = await sqlite.open({
        filename: "./Chinook_Sqlite.sqlite",
        driver: sqlite3.Database,
    });
    return db;
}

// this is stating the objects are static
// left side path
// right side forces dirctory to be static
app.use('/static', express.static('public'));

app.get('/invoice_items', async (req, res) => {
    let db = await getDBConnection();
    const request = "SELECT * FROM Invoice"  
    let rows = await db.all(request);
    res.json(rows);
});

const PORT = process.env.PORT || 8000;
app.listen(PORT, () => {
    console.log('Server ready and running on ${PORT}');
});
```

## lab 11
# index.html (public)
``` javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
    crossorigin="anonymous"></script>
  <title>Index Page</title>
</head>

<body>

  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Navigation for Expenses</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item active">
          <a class="nav-link" href="/public/index.html">Index</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/public/add_expense.html">Add Expense</a>
          <li class="nav-item">
            <a class="nav-link" href="/public/chart.html">Chart</a>
          </li>
        </li>
      </ul>
    </div>
  </nav>

  <div class="container">
    <h2>Current Finances (Index)</h2>
    <p id="updateTime"></p>
    <table class="table">
      <thead class="thead-light">
        <tr>
          <th>Title</th>
          <th>Category</th>
          <th>Amount</th>
          <th>Type</th>
          <th>Date</th>
        </tr>
      </thead>
      <tbody id="financeData">
        <!-- Finance data will be dynamically inserted here -->
      </tbody>
    </table>
  </div>

</body>

<script>
  function updateCurrentTime() {
    // Just updates the time on webpage.
    const now = new Date();
    const currentTime =
      now.getHours() + ":" + now.getMinutes() + ":" + now.getSeconds();
    document.getElementById("updateTime").innerText =
      "Last Updated: " + currentTime;
  }

  function fetchFinanceData() {
    fetch('/finance-data')
      .then(response => response.json())
      .then(data => {
        updateFinanceTable(data);
        updateCurrentTime();
      })
      .catch(error => console.error('Error fetching finance data:', error));
  }

  function updateFinanceTable(data) {
    const financeDataElement = document.getElementById("financeData");
    financeDataElement.innerHTML = ""; // Clear existing data
    data.forEach((item) => {
      const tr = document.createElement("tr");

      const tdTitle = document.createElement("td");
      const tdCategory = document.createElement("td");
      const tdAmount = document.createElement("td");
      const tdType = document.createElement("td");
      const tdDate = document.createElement("td");
      const tdActions = document.createElement("td");
      const tdEdit = document.createElement("td");

      const tnTitle = document.createTextNode(item.title);
      const tnCategory = document.createTextNode(item.category);
      const tnAmount = document.createTextNode(item.amount);
      const tnType = document.createTextNode(item.type);
      const tnDate = document.createTextNode(item.date);
      const deleteButton = document.createElement("button"); 
      deleteButton.textContent = "Delete";
      deleteButton.addEventListener("click", function () {
        deleteExpense(item.id); 
      });
      const btnEdit = document.createElement("button"); 
      btnEdit.textContent = "Edit"; 
      btnEdit.addEventListener("click", async () => {
        console.log("addEventListener - click edit", item)
        await editExpense(item.id, item); 
      });

      tdTitle.appendChild(tnTitle);
      tdCategory.appendChild(tnCategory);
      tdAmount.appendChild(tnAmount);
      tdType.appendChild(tnType);
      tdDate.appendChild(tnDate);
      tdActions.appendChild(deleteButton);
      tdEdit.appendChild(btnEdit);

      tr.appendChild(tdTitle);
      tr.appendChild(tdCategory);
      tr.appendChild(tdAmount);
      tr.appendChild(tdType);
      tr.appendChild(tdDate);
      tr.appendChild(tdActions);
      tr.appendChild(tdEdit);

      financeDataElement.appendChild(tr);
    });
  }

  function deleteExpense(id) {
    fetch(`/expenses/${id}`, {
      method: 'DELETE'
    })
      .then(response => {
        if (response.ok) {
          // Update table upon success
          fetchFinanceData(); // Re-fetch data to update the table
        } else {
          throw new Error('Failed to delete expense');
        }
      })
      .catch(error => console.error('Error deleting expense:', error));
  }

  async function editExpense(expenseId, data) {
    window.location.href = `/public/edit-expense.html?id=${expenseId}&title=${data.title}&category=${data.category}&amount=${data.amount}&type=${data.type}&date=${data.date}`;
  }

  // Run it
  setInterval(fetchFinanceData, 5000);
  fetchFinanceData(); // Fetch data initially
</script>

</html>
```

# edit-expense.html (public_
``` javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
    crossorigin="anonymous"></script>
    <title>Edit Expense</title>
</head>

<body>
    <h2>Edit Expense</h2>
    <form id="expenseForm">
        <label for="title">Title:</label><br>
        <input type="text" id="title" name="title"><br>
        <label for="category">Category:</label><br>
        <input type="text" id="category" name="category"><br>
        <label for="amount">Amount:</label><br>
        <input type="text" id="amount" name="amount"><br>
        <label for="type">Type:</label><br>
        <select id="type" name="type">
            <option value="Debit">Debit</option>
            <option value="Credit">Credit</option>
        </select><br>
        <label for="date">Date:</label><br>
        <input type="date" id="date" name="date"><br><br>
        <input type="submit" value="Submit">
    </form>
    
    <script>
        // Fetch expense data and populate the form fields
        const urlParams = new URLSearchParams(window.location.search);
        const id = urlParams.get('id');
        const url = `/expenses/${id}`;
        document.getElementById("title").value = urlParams.get('title');
        document.getElementById("category").value = urlParams.get('category');
        document.getElementById("amount").value = urlParams.get('amount');
        document.getElementById("type").value = urlParams.get('type');
        document.getElementById("date").value = urlParams.get('date');
       
        // Add event listener for form submission
        document.getElementById("expenseForm").addEventListener("submit", function(event) {
            event.preventDefault(); // Prevent default form submission behavior
    
            const formData = {
                title: document.getElementById("title").value,
                category: document.getElementById("category").value,
                amount: document.getElementById("amount").value,
                type: document.getElementById("type").value,
                date: document.getElementById("date").value
            };
            const url = `/expenses/${id}`;
            fetch(url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(formData)
            })
            .then(response => {
                if (response.ok) {
                    // Redirect to the index page
                    window.location.href = '/public/index.html';
                } else {
                    throw new Error('Failed to update expense');
                }
            })
            .catch(error => console.error(error));
        });
    </script>
```

# chart.html (public)
``` javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <title>my expenses over time</title>
    <!-- Script to load Chart.js library -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

</head>

<body>

    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="#">Navigation for Expenses</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav"
            aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item">
                    <a class="nav-link" href="/public/index.html">Index </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="/public/add_expense.html">Add Expense</a>
                </li>
                <li class="nav-item active">
                    <a class="nav-link" href="/public/chart.html">Chart <span class="sr-only">(current)</span></a>
                </li>
            </ul>
        </div>
    </nav>


    <!-- Container for the content -->
    <div class="container">
        <div class="row">
            <div class="col">
                <!-- Canvas element where the chart will be drawn -->
                <canvas id="myChart"></canvas>
            </div>
        </div>
    </div>

    <!-- Script to create the chart -->
    <script>
        // Fetch expenses from the Express endpoint
        fetch('/finance-data')
            .then(response => response.json())
            .then(data => {
                const transactionSummary = data.reduce((acc, { date, amount, type }) => {
                    if (!acc[date]) {
                        acc[date] = { credit: 0, debit: 0 };
                    }
                    if (type === "Credit") {
                        acc[date].credit += amount;
                    } else {
                        acc[date].debit += amount;
                    }
                    return acc;
                }, {});

                const dates = Object.keys(transactionSummary).sort();
                const credits = dates.map(date => transactionSummary[date].credit);
                const debits = dates.map(date => -transactionSummary[date].debit);

                const ctx = document.getElementById('myChart').getContext('2d');

                const transactionChart = new Chart(ctx, {
                    type: 'line',
                    data: {
                        labels: dates,
                        datasets: [
                            {
                                label: 'Credits',
                                data: credits,
                                borderColor: 'rgba(75, 192, 192, 1)',
                                backgroundColor: 'rgba(75, 192, 192, 0.5)',
                                fill: false,
                            },
                            {
                                label: 'Debits',
                                data: debits,
                                borderColor: 'rgba(255, 99, 132, 1)',
                                backgroundColor: 'rgba(255, 99, 132, 0.5)',
                                fill: false,
                            }
                        ]
                    },
                    options: {
                        scales: {
                            y: {
                                beginAtZero: true,
                                ticks: {
                                    callback: function (value) {
                                        return '$' + Math.abs(value);
                                    }
                                }
                            }
                        },
                        tooltips: {
                            callbacks: {
                                label: function (tooltipItem, data) {
                                    let label = data.datasets[tooltipItem.datasetIndex].label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    label += '$' + Math.abs(tooltipItem.yLabel);
                                    return label;
                                }
                            }
                        }
                    }
                });
            })
            .catch(error => {
                console.error('Error fetching expenses:', error);
            });
    </script>

</body>

</html>
```

# add_expense.html (public)
``` javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
    crossorigin="anonymous"></script>
    <title>Add Expense</title>
</head>

<body>

    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="#">Navigation for Expenses</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav"
            aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item">
                    <a class="nav-link" href="/public/index.html">Index </a>
                </li>
                <li class="nav-item active">
                    <a class="nav-link" href="/public/add_expense.html">Add Expense</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="/public/chart.html">Chart</a>
                </li>
            </ul>
        </div>
    </nav>

    <h2>Add New Expense</h2>
    <form id="expenseForm">
        <label for="title">Title:</label><br>
        <input type="text" id="title" name="title"><br>
        <label for="category">Category:</label><br>
        <input type="text" id="category" name="category"><br>
        <label for="amount">Amount:</label><br>
        <input type="text" id="amount" name="amount"><br>
        <label for="type">Type:</label><br>
        <select id="type" name="type">
            <option value="Debit">Debit</option>
            <option value="Credit">Credit</option>
        </select><br>
        <label for="date">Date:</label><br>
        <input type="date" id="date" name="date"><br><br>
        <input type="submit" value="Submit">
    </form>

    <script>
        document.getElementById("expenseForm").addEventListener("submit", function (event) {
            event.preventDefault();

            const formData = new FormData(this);
            const expenseData = {};
            formData.forEach(function (value, key) {
                expenseData[key] = value;
            });

            fetch('/add-expense', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(expenseData)
            })
                .then(response => response.json())
                .then(data => {
                    alert(data.message);
                    document.getElementById("expenseForm").reset();
                })
                .catch(error => console.error('Error adding expense:', error));
        });
    </script>
</body>

</html>
```

# server.js
``` javascript
'use strict';

const express = require("express");
const sqlite3 = require("sqlite3").verbose();

const app = express();
const port = process.env.PORT || 8000;

app.use('/public', express.static('public'));
app.use(express.urlencoded({ extended: true }));
app.use(express.json());

// Connect to SQLite database
const db = new sqlite3.Database('expenses.db');

function getDBConnection() {
    return new Promise((resolve, reject) => {
        const db = new sqlite3.Database('expenses.db', (err) => {
            if (err) {
                reject(err); // Reject if there's an error
            } else {
                resolve(db); // Resolve with the database instance
            }
        });
    });
}

// Create expenses table if not exists
db.run(`
    CREATE TABLE IF NOT EXISTS expenses (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        title TEXT,
        category TEXT,
        amount REAL,
        type TEXT,
        date TEXT
    )
`);

// Endpoint to return all expenses from the database
app.get('/finance-data', (req, res) => {
    db.all('SELECT * FROM expenses', (err, rows) => {
        if (err) {
            console.error('Error fetching finance data:', err);
            res.status(500).json({ message: 'Error fetching finance data' });
        } else {
            res.json(rows);
        }
    });
});

// POST endpoint to add new expense to the database
app.post('/add-expense', (req, res) => {
    const newExpense = req.body;
    db.run(`
        INSERT INTO expenses (title, category, amount, type, date) 
        VALUES (?, ?, ?, ?, ?)
    `, [newExpense.title, newExpense.category, newExpense.amount, newExpense.type, newExpense.date], function (err) {
        if (err) {
            console.error('Error adding expense:', err);
            res.status(500).json({ message: 'Error adding expense' });
        } else {
            res.json({ message: 'Expense added successfully!', expense: newExpense, expenseId: this.lastID });
        }
    });
});

app.delete("/expenses/:id", async function (req, res) {
    let db = await getDBConnection();
    const id = req.params.id;
    const deleteQuery = "DELETE FROM expenses WHERE id = ?";
    try {
        const delOut = await db.run(deleteQuery, id);
        await db.run("DELETE FROM sqlite_sequence WHERE name='expenses'");
        res.status(200).send("Expense deleted successfully");
    } catch (error) {
        res.status(404).send("Expense id not found");
    }
});

// Endpoint to update expense data
app.post('/expenses/:id', (req, res) => {
    const { id } = req.params;
    const updatedExpense = req.body;

    db.run(`
        UPDATE expenses 
        SET title = ?, category = ?, amount = ?, type = ?, date = ? 
        WHERE id = ?
    `, [updatedExpense.title, updatedExpense.category, updatedExpense.amount, updatedExpense.type, updatedExpense.date, id], function (err) {
        if (err) {
            console.error('Error updating expense:', err);
            res.status(500).json({ message: 'Error updating expense' });
        } else {
            // Send a success response
            res.json({ message: 'Expense updated successfully!', expense: updatedExpense });

        }
    });
});


app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});
```

## Node gitignore link

- https://github.com/github/gitignore/blob/main/Node.gitignore
