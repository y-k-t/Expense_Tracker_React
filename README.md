# Expense Tracker (ReactJS)
## Date: 24/05/2025

## AIM
To develop a simple Expense Tracker application using React that allows users to manage their personal finances by adding, viewing, and deleting income and expense transactions, while dynamically calculating the current balance, total income, and total expenses.

## ALGORITHM
### STEP 1: Initialize the Project
Create a new React app using:

npx create-react-app expense-tracker
or

npm create vite@latest expense-tracker --template react

Open the project in a code editor like VS Code.

### Step 2: Setup State
Define a state variable to store transactions:

Example: const [transactions, setTransactions] = useState([])

Define state variables for the form inputs:

const [text, setText] = useState("")

const [amount, setAmount] = useState("")

### Step 3: Add a New Transaction
Create a form with two inputs:

Text input for description

Number input for amount

On form submit:

Validate input

Create a new transaction object

Add the object to the transactions array using setTransactions.

### Step 4: Display Transaction List

Use map() to render each transaction in a list.

Conditionally style each item based on amount > 0 (income) or amount < 0 (expense).

Add a delete button next to each transaction.

### Step 5: Calculate and Display Summary

Use reduce() to calculate:

Total Balance: sum of all amounts

Total Income: sum of all positive amounts

Total Expenses: sum of all negative amounts

Display these values at the top of the UI.

### Step 6: Delete a Transaction

When delete is clicked:

Use filter() to remove the transaction from the array by id.

Update the state using setTransactions.

### Step 7: Style the Application

Use basic CSS to style:

Balance summary

Income/expense totals

Form inputs

Transaction list (with color coding)

## PROGRAM
# AddTransaction.js
```js
import React, { useState } from 'react';

function AddTransaction({ addTransaction }) {
  const [text, setText] = useState('');
  const [amount, setAmount] = useState(0);

  const onSubmit = e => {
    e.preventDefault();

    const newTransaction = {
      id: Math.floor(Math.random() * 1000000),
      text,
      amount: +amount
    };

    addTransaction(newTransaction);
    setText('');
    setAmount(0);
  };

  return (
    <>
      <h3>Add New Transaction</h3>
      <form onSubmit={onSubmit}>
        <div className="form-control">
          <label>Description</label>
          <input type="text" value={text} onChange={(e) => setText(e.target.value)} placeholder="Enter description" required />
        </div>
        <div className="form-control">
          <label>Amount (positive = income, negative = expense)</label>
          <input type="number" value={amount} onChange={(e) => setAmount(e.target.value)} required />
        </div>
        <button className="btn">Add Transaction</button>
      </form>
    </>
  );
}

export default AddTransaction;
```
# Balance.js
```js
import React from 'react';

function Balance({ transactions }) {
  const amounts = transactions.map(t => t.amount);
  const total = amounts.reduce((acc, val) => acc + val, 0).toFixed(2);

  return (
    <div className="balance">
      <h4>Your Balance</h4>
      <h1>${total}</h1>
    </div>
  );
}

export default Balance;
```
# IncomeExpenses.js
```js
import React from 'react';

function IncomeExpenses({ transactions }) {
  const amounts = transactions.map(t => t.amount);
  const income = amounts.filter(a => a > 0).reduce((acc, a) => acc + a, 0).toFixed(2);
  const expense = (amounts.filter(a => a < 0).reduce((acc, a) => acc + a, 0) * -1).toFixed(2);

  return (
    <div className="inc-exp-container">
      <div>
        <h4>Income</h4>
        <p className="money plus">+${income}</p>
      </div>
      <div>
        <h4>Expense</h4>
        <p className="money minus">-${expense}</p>
      </div>
    </div>
  );
}

export default IncomeExpenses;
```
Transaction.js
```js
import React from 'react';

function Transaction({ transaction, deleteTransaction }) {
  const sign = transaction.amount < 0 ? '-' : '+';

  return (
    <div className={`transaction ${transaction.amount < 0 ? 'minus' : 'plus'}`}>
  <span>{transaction.text}</span>
  <span>{transaction.amount < 0 ? '-' : '+'}${Math.abs(transaction.amount)}</span>
  <button onClick={() => deleteTransaction(transaction.id)} className="delete-btn" title="Delete">
    &times;
  </button>
</div>

  );
}

export default Transaction;
```
# TransactionList.js
```
import React from 'react';
import Transaction from './Transaction';

function TransactionList({ transactions, deleteTransaction }) {
  return (
    <>
      <h3>History</h3>
      <ul className="list">
        {transactions.map(t => (
          <Transaction key={t.id} transaction={t} deleteTransaction={deleteTransaction} />
        ))}
      </ul>
    </>
  );
}

export default TransactionList;
```
# App.js
```
import React, { useState } from 'react';
import './App.css';
import Balance from './components/Balance';
import IncomeExpenses from './components/IncomeExpenses';
import TransactionList from './components/TransactionList';
import AddTransaction from './components/AddTransaction';

function App() {
  const [transactions, setTransactions] = useState([]);

  const addTransaction = (transaction) => {
    setTransactions([transaction, ...transactions]);
  };

  const deleteTransaction = (id) => {
    setTransactions(transactions.filter(t => t.id !== id));
  };

  return (
  <>
    <div className="container">
      <h2>Expense Tracker</h2>
      <Balance transactions={transactions} />
      <IncomeExpenses transactions={transactions} />
      <TransactionList
        transactions={transactions}
        deleteTransaction={deleteTransaction}
      />
      <AddTransaction addTransaction={addTransaction} />
    </div>

    <footer className="footer">
      <p>© {new Date().getFullYear()} Expense Tracker | Made by Yogesh | 21222040185</p>
    </footer>
  </>
);
}

export default App;
```
## OUTPUT
![image](https://github.com/user-attachments/assets/1cdd735f-48ca-446b-b7ed-71b0a67f3d72)
![image](https://github.com/user-attachments/assets/814eed3e-3187-41c7-94c5-68c8f196b4df)


## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
