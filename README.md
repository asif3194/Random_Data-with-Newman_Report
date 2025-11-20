# 🚀 Random Data Generator – API Automation Using Postman & Newman

> A professional API automation project using Postman + Newman that dynamically generates booking data, validates responses, and performs authenticated update operations on the Restful Booker API.

---

## 📌 Project Summary

This project automates end-to-end testing for the **Restful Booker API** by dynamically generating test data using **Postman pre-request scripts** and validating responses using **test scripts**.  

Automation is executed via **Newman**, producing a professional **HTML report** showing request status, assertions, and response time.

**Base URL:**

```text
https://restful-booker.herokuapp.com
```

---

## 📂 Project Files

```text
.
├── Random_Data_Generator.postman_collection.json
├── Random_Data_Generator.postman_environment.json
├── newman-run-report-2025-11-17-18-22-03-791-0.html
├── screenshots/
│   └── newman-report.png
└── README.md
```

---

## 🔧 API Workflow

This collection executes the following process:

```
Create Booking ➝
Store booking ID ➝
Get Booking ➝
Validate response ➝
Create Token ➝
Update Booking (Authorized)
```

Each run generates **new random data**, ensuring repeatable, realistic testing.

---

## 🧬 Dynamic Data Generation (Pre-Request Script)

```javascript
// Random first name
var firstname = pm.variables.replaceIn('{{$randomFirstName}}');
pm.environment.set('firstname', firstname);

// Random last name
var lastname = pm.variables.replaceIn('{{$randomLastName}}');
pm.environment.set('lastname', lastname);

// Random price
var price = pm.variables.replaceIn('{{$randomInt}}');
pm.environment.set('price', price);

// Random deposit paid
var paid = pm.variables.replaceIn('{{$randomBoolean}}');
pm.environment.set('paid', paid);

// Generate random dates
let randomYear = Math.floor(Math.random() * (2025 - 2018 + 1)) + 2018;
let checkin = new Date(randomYear, Math.floor(Math.random() * 12), Math.floor(Math.random() * 28) + 1);
let checkout = new Date(checkin);
checkout.setDate(checkout.getDate() + Math.floor(Math.random() * 30) + 1);

pm.environment.set('checkin', checkin.toISOString().split('T')[0]);
pm.environment.set('checkout', checkout.toISOString().split('T')[0]);

// Additional needs
var need = pm.variables.replaceIn('{{$randomWord}}');
pm.environment.set('need', need);
```

---

## ✅ Response Validation (Test Script)

```javascript
var jsondata = pm.response.json();

// Validate First Name
pm.test("First Name Validation", function() {
    pm.expect(pm.environment.get("firstname")).to.eql(jsondata.firstname);
});

// Validate Last Name
pm.test("Last Name Validation", function() {
    pm.expect(pm.environment.get("lastname")).to.eql(jsondata.lastname);
});

// Validate Price
pm.test("Total Price Validation", function() {
    pm.expect(Number(pm.environment.get("price"))).to.eql(jsondata.totalprice);
});

// Validate Deposit Status
pm.test("Deposit Paid Validation", function() {
    pm.expect(pm.environment.get("paid") === "true").to.eql(jsondata.depositpaid);
});

// Validate Dates
pm.test("Check-In Date Validation", function() {
    pm.expect(pm.environment.get("checkin")).to.eql(jsondata.bookingdates.checkin);
});

pm.test("Check-Out Date Validation", function() {
    pm.expect(pm.environment.get("checkout")).to.eql(jsondata.bookingdates.checkout);
});

// Validate Additional Needs
pm.test("Additional Needs Validation", function() {
    pm.expect(pm.environment.get("need")).to.eql(jsondata.additionalneeds);
});
```

---

## 🔐 Authentication (Create Token)

```json
{
  "username": "admin",
  "password": "password123"
}
```

Token is saved automatically to:

```text
{{token}}
```

Then used in request header:

```text
Cookie: token={{token}}
```

---

## ▶️ How to Run Using Postman

1. Open Postman
2. Click **Import**
3. Import both:
   - `Random_Data_Generator.postman_collection.json`
   - `Random_Data_Generator.postman_environment.json`
4. Select environment:
   ```text
   Random_Data_Generator
   ```
5. Run requests in this order:

```text
1. Create Booking
2. Get Booking
3. Create Token
4. Update Booking
```

---

## ⚙️ Run Using Newman (CLI)

Install Newman:

```bash
npm install -g newman
```

Run command:

```bash
newman run Random_Data_Generator.postman_collection.json \
-e Random_Data_Generator.postman_environment.json \
-r htmlextra
```

An HTML report is generated automatically.

---

## 📊 Newman Execution Report

**Screenshot path:**

```text
/screenshots/newman-report.png
```

**Add this in your README:**

```md
![Newman Report](screenshots/newman-report.png)
```

✔ Passed requests  
✔ Response time metrics  
✔ Assertion results  
✔ Visualization graphs  

---

## 🔐 Environment Variables

| Variable    | Description |
|------------|------------|
| Baseurl    | API base URL |
| id         | Booking ID |
| token      | Auth token |
| firstname  | Random name |
| lastname   | Random surname |
| price      | Booking price |
| paid       | Deposit status |
| checkin    | Start date |
| checkout   | End date |
| need       | Extra requirements |

---

## 💻 Technology Stack

```
• Postman
• Newman
• JavaScript
• REST API
• Node.js
```

<img width="1920" height="879" alt="Screenshot (32)" src="https://github.com/user-attachments/assets/63048c31-be81-4dfc-a53c-c0cefb6923a2" />

<img width="1920" height="870" alt="Screenshot (33)" src="https://github.com/user-attachments/assets/d9268d29-35c3-4479-9796-1d3d5c9af18e" />
