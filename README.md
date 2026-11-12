# S-R-K--motors--management-system
complete S R K Motors multi-Branded car service centre management system for managing total revenue expenses, customers,vehicles,service records employee details,billing,spare parts inventory ,business documents and financial reports in one secure application
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SRK Motors Management System</title>

<style>
* { box-sizing: border-box; }

body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f1f4f9;
    color: #172033;
}

header {
    background: #101c32;
    color: white;
    padding: 25px;
    text-align: center;
}

header h1 { margin: 0; font-size: 30px; }

.container {
    max-width: 1100px;
    margin: auto;
    padding: 20px;
}

.cards {
    display: grid;
    grid-template-columns: repeat(auto-fit,minmax(200px,1fr));
    gap: 15px;
}

.card, .section {
    background: white;
    padding: 22px;
    border-radius: 12px;
    box-shadow: 0 3px 12px #00000010;
}

.card h3 { color: #64748b; }

.amount {
    font-size: 25px;
    font-weight: bold;
    color: #087443;
}

.expense { color: #dc3545; }
.profit { color: #155eef; }

.section { margin-top: 22px; }

input, select, button {
    width: 100%;
    padding: 12px;
    margin: 7px 0;
    border: 1px solid #cbd5e1;
    border-radius: 7px;
    font-size: 15px;
}

button {
    background: #101c32;
    color: white;
    cursor: pointer;
    border: none;
}

.table-wrap { overflow-x: auto; }

table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 15px;
}

th, td {
    padding: 12px;
    border-bottom: 1px solid #ddd;
    text-align: left;
}

th { background: #edf2f7; }

.delete { background: #dc3545; }

footer {
    text-align: center;
    padding: 25px;
    color: #64748b;
}
</style>
</head>

<body>

<header>
<h1>SRK MOTORS</h1>
<p>Multi-Branded Car Service Centre</p>
<h2>Business Management Dashboard</h2>
</header>

<div class="container">

<div class="cards">

<div class="card">
<h3>Total Revenue</h3>
<div class="amount" id="revenue">₹0</div>
</div>

<div class="card">
<h3>Total Expenses</h3>
<div class="amount expense" id="expenses">₹0</div>
</div>

<div class="card">
<h3>Net Balance</h3>
<div class="amount profit" id="balance">₹0</div>
</div>

<div class="card">
<h3>Total Transactions</h3>
<div class="amount" id="count">0</div>
</div>

</div>

<div class="section">

<h2>Add Revenue / Expense</h2>

<form id="entryForm">

<label>Transaction Type</label>
<select id="type" required>
<option value="Revenue">Revenue / Income</option>
<option value="Expense">Business Expense</option>
</select>

<label>Date</label>
<input type="date" id="date" required>

<label>Customer / Supplier Name</label>
<input type="text" id="person" placeholder="Enter name" required>

<label>Vehicle / Description</label>
<input type="text" id="description"
placeholder="BMW General Service" required>

<label>Amount (₹)</label>
<input type="number" id="amount" min="0.01"
step="0.01" placeholder="Enter amount" required>

<button type="submit">Save Transaction</button>

</form>
</div>

<div class="section">

<h2>Financial Records</h2>

<div class="table-wrap">
<table>
<thead>
<tr>
<th>Date</th>
<th>Type</th>
<th>Name</th>
<th>Description</th>
<th>Amount</th>
<th>Action</th>
</tr>
</thead>
<tbody id="records"></tbody>
</table>
</div>

</div>
</div>

<footer>
© 2026 SRK MOTORS | Business Management System
</footer>

<script>

let transactions = [];

try {
transactions = JSON.parse(
localStorage.getItem("srkTransactions") || "[]"
);
if (!Array.isArray(transactions)) transactions = [];
} catch {
transactions = [];
}

document.getElementById("date").value =
new Date().toISOString().split("T")[0];

function money(value) {
return "₹" + value.toLocaleString("en-IN", {
maximumFractionDigits: 2
});
}

function saveData() {
localStorage.setItem(
"srkTransactions",
JSON.stringify(transactions)
);
}

function updateDashboard() {

let revenue = 0;
let expenses = 0;

transactions.forEach(item => {
if (item.type === "Revenue") {
revenue += item.amount;
} else {
expenses += item.amount;
}
});

document.getElementById("revenue").textContent =
money(revenue);

document.getElementById("expenses").textContent =
money(expenses);

document.getElementById("balance").textContent =
money(revenue - expenses);

document.getElementById("count").textContent =
transactions.length;

let table = document.getElementById("records");
table.innerHTML = "";

transactions.forEach((item, index) => {

let row = document.createElement("tr");

[
item.date,
item.type,
item.person,
item.description,
money(item.amount)
].forEach(value => {
let cell = document.createElement("td");
cell.textContent = value;
row.appendChild(cell);
});

let actionCell = document.createElement("td");
let deleteButton = document.createElement("button");

deleteButton.textContent = "Delete";
deleteButton.className = "delete";

deleteButton.onclick = function() {
if (confirm("Delete this transaction?")) {
transactions.splice(index, 1);
saveData();
updateDashboard();
}
};

actionCell.appendChild(deleteButton);
row.appendChild(actionCell);
table.appendChild(row);

});
}

document.getElementById("entryForm")
.addEventListener("submit", function(event) {

event.preventDefault();

let item = {
type: document.getElementById("type").value,
date: document.getElementById("date").value,
person: document.getElementById("person").value,
description: document.getElementById("description").value,
amount: Number(document.getElementById("amount").value)
};

if (!Number.isFinite(item.amount) || item.amount <= 0) {
alert("Enter a valid amount");
return;
}

transactions.push(item);
saveData();
updateDashboard();

this.reset();

document.getElementById("date").value =
new Date().toISOString().split("T")[0];

alert("Transaction saved successfully!");

});

updateDashboard();

</script>

</body>
</html>
