document.getElementById('expense-form').addEventListener('submit', addExpense);

let expenses = [];

function addExpense(e) {
    e.preventDefault();
    
    const description = document.getElementById('description').value;
    const amount = parseFloat(document.getElementById('amount').value);
    const date = document.getElementById('date').value;
    
    if(description === '' || isNaN(amount) || date === '') {
        alert('Please fill in all fields');
        return;
    }

    const expense = {
        description,
        amount,
        date,
        id: new Date().getTime()
    };

    expenses.push(expense);

    document.getElementById('description').value = '';
    document.getElementById('amount').value = '';
    document.getElementById('date').value = '';

    updateExpenseList();
}

function updateExpenseList() {
    const expenseList = document.getElementById('expense-list');
    expenseList.innerHTML = '';

    let totalAmount = 0;

    expenses.forEach(expense => {
        const li = document.createElement('li');
        li.innerHTML = `
            <div class="info">
                ${expense.description} - ₹${expense.amount.toFixed(2)} - ${expense.date}
            </div>
            <button onclick="deleteExpense(${expense.id})">Delete</button>
        `;
        expenseList.appendChild(li);
        totalAmount += expense.amount;
    });

    document.getElementById('total-amount').innerText = totalAmount.toFixed(2);
}

function deleteExpense(id) {
    expenses = expenses.filter(expense => expense.id !== id);
    updateExpenseList();
}
