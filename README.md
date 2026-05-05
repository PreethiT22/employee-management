# 🏢 Employee Management System

A Vue.js + Axios + MockAPI CRUD application for managing employee records.

**Assignment II — Web Programming (WP) | BE/BTech CSE3, Sem IV**

---

## 📁 Project Structure

```
employee-management/
├── src/
│   ├── App.vue                    ← Root component
│   ├── main.js                    ← App entry point (Bootstrap imported here)
│   └── components/
│       └── EmployeeApp.vue        ← Full CRUD component
├── public/
│   └── index.html
├── package.json
└── README.md
```

---

## 🚀 Setup Instructions

### Step 1 — Create MockAPI

1. Go to [https://mockapi.io](https://mockapi.io) and login
2. Create a project: `EmployeeAPI`, prefix: `api`
3. Create resource: `employees` with fields:
   - `id` → auto
   - `name` → String
   - `designation` → String
   - `department` → String
   - `salary` → Number
4. Copy your endpoint URL: `https://XXXXXXXX.mockapi.io/api/employees`

### Step 2 — Create Vue Project

```bash
npm install -g @vue/cli
vue create employee-management
# Choose: Default (Vue 3)
cd employee-management
npm install axios bootstrap
```

### Step 3 — Add the Files

Replace the contents of these files with the provided code:
- `src/main.js`
- `src/App.vue`
- `src/components/EmployeeApp.vue` ← **Create this file**

### Step 4 — Set Your API URL

In `src/components/EmployeeApp.vue`, find this line and replace with your URL:

```js
const API_URL = 'https://YOUR_MOCKAPI_ID.mockapi.io/api/employees';
```

### Step 5 — Run the App

```bash
npm run serve
```

Open: [http://localhost:8080](http://localhost:8080)

---

## 📦 Deploy to GitHub

```bash
# Inside your project folder
git init
git add .
git commit -m "Initial commit - Employee Management System"

# Create repo on GitHub, then:
git remote add origin https://github.com/YOUR_USERNAME/employee-management.git
git branch -M main
git push -u origin main
```

---

## ✅ Features

- **Add** new employee records
- **View** all employees in a responsive table
- **Edit** any employee record (pre-filled form)
- **Delete** employees with confirmation
- Alert messages for all operations
- Bootstrap responsive design
- Loading spinner while fetching data

---

## 🔧 Tech Stack

| Technology | Purpose |
|------------|---------|
| Vue.js 3 | Frontend framework |
| Axios | HTTP requests (CRUD) |
| MockAPI | Backend REST API (mock) |
| Bootstrap 5 | UI styling |

---

## 📸 Screenshots

> Add screenshots of your running app here before submission.

---

## VIVA Quick Reference

| Question | Answer |
|----------|--------|
| What is Vue.js? | Progressive JS framework for building UIs |
| What is Axios? | Promise-based HTTP client for API calls |
| What is CRUD? | Create, Read, Update, Delete |
| What is mounted()? | Lifecycle hook that runs after component loads into DOM |
| What is async/await? | Syntax for handling asynchronous operations cleanly |