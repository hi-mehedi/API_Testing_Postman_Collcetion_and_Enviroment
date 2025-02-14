# Random Date Generator API Testing

## Overview
This project contains a Postman collection designed to automate API testing for booking management. The collection covers various endpoints that handle booking creation, retrieval, updates, and authentication.

## Features
- Automates API testing using Postman and Newman.
- Handles dynamic data generation through scripts.
- Supports authentication with token-based security.
- Includes pre-request and post-request scripts for efficient testing.

## API Documentation
For a detailed API reference, visit: [Postman API Documentation](https://documenter.getpostman.com/view/41963861/2sAYXEDJ1d)

## Technologies Used
- **Postman**: API development and testing.
- **Newman**: Command-line tool for running Postman collections.
- **JavaScript**: Used in scripts to manipulate request data.
- **Node.js**: Required for Newman execution.

## Prerequisites
- Install **Postman** for API testing.
- Install **Node.js** (LTS version recommended).
- Ensure an active internet connection.

## How to Clone the Project
```sh
git clone https://github.com/hi-mehedi/API_Testing_Postman_Collcetion_and_Enviroment.git

```

## Usage
### Running the Collection
1. **Import Collection in Postman:**
   - Open Postman.
   - Click **Import** and select `Random_Date_Generator.postman_collection.json`.
2. **Run with Postman Environment:**
   - Set **Baseurl** in the Postman environment.
   - Use `Random_Date_Generator.postman_environment.json`.

# Generating an HTML Report using Newman with htmlextra

To generate a detailed HTML report using **htmlextra**, install the reporter package:
```sh
npm install -g newman-reporter-htmlextra
```

Then, execute the following command to run the collection and generate an enhanced HTML report:
```sh
newman run Random_Date_Generator.postman_collection.json -e Random_Date_Generator.postman_environment.json -r htmlextra --reporter-htmlextra-export report.html
```

This will create a `report.html` file, which provides an interactive and visually enhanced view of the test results.



## API Implementation

Baseurl:https://restful-booker.herokuapp.com

### 1. Create Booking (POST /booking/)
This endpoint generates a new booking with randomized user details.

**Request URL:**
```
{{Baseurl}}/booking/
```
**Pre-request Script:**
```javascript
var firstname = pm.variables.replaceIn('{{$randomFirstName}}');
var lastname = pm.variables.replaceIn('{{$randomLastName}}');
var price = pm.variables.replaceIn('{{$randomInt}}');
var paid = pm.variables.replaceIn('{{$randomBoolean}}');
var need = pm.variables.replaceIn('{{$randomWord}}');

let randomYear = Math.floor(Math.random() * (2025 - 2018 + 1)) + 2018;
let checkin = new Date(randomYear, Math.floor(Math.random() * 12), Math.floor(Math.random() * 28) + 1);
let checkout = new Date(checkin);
checkout.setDate(checkout.getDate() + Math.floor(Math.random() * 30) + 1);

pm.environment.set("firstname", firstname);
pm.environment.set("lastname", lastname);
pm.environment.set("price", price);
pm.environment.set("paid", paid);
pm.environment.set("need", need);
pm.environment.set("checkin", checkin.toISOString().split('T')[0]);
pm.environment.set("checkout", checkout.toISOString().split('T')[0]);
```

**Request Body:**
```json
{
  "firstname": "{{firstname}}",
  "lastname": "{{lastname}}",
  "totalprice": {{price}},
  "depositpaid": {{paid}},
  "bookingdates": {
    "checkin": "{{checkin}}",
    "checkout": "{{checkout}}"
  },
  "additionalneeds": "{{need}}"
}
```

**Post-request Script:**
```javascript
var jsonResponse = pm.response.json();
pm.environment.set("id", jsonResponse.bookingid);
```

**Response Example:**
```json
{
  "bookingid": 1234,
  "booking": {
    "firstname": "John",
    "lastname": "Doe",
    "totalprice": 100,
    "depositpaid": true,
    "bookingdates": {
      "checkin": "2024-06-01",
      "checkout": "2024-06-05"
    },
    "additionalneeds": "Breakfast"
  }
}
```

### 2. Get Booking (GET /booking/{id})
Fetches details of an existing booking using `id`.

**Request URL:**
```
{{Baseurl}}/booking/{{id}}
```

**Response Example:**
```json
{
  "firstname": "John",
  "lastname": "Doe",
  "totalprice": 100,
  "depositpaid": true,
  "bookingdates": {
    "checkin": "2024-06-01",
    "checkout": "2024-06-05"
  },
  "additionalneeds": "Breakfast"
}
```

### 3. Create Token (POST /auth)
Generates an authentication token.

**Request Body:**
```json
{
  "username": "admin",
  "password": "password123"
}
```

**Post-request Script:**
```javascript
var token = pm.response.json();
pm.environment.set("token", token.token);
```

**Response Example:**
```json
{
  "token": "abc123xyz"
}
```

### 4. Update Booking (PUT /booking/{id})
Updates an existing booking using authentication.

**Request URL:**
```
{{Baseurl}}/booking/{{id}}
```

**Request Headers:**
```json
{
  "Content-Type": "application/json",
  "Cookie": "token={{token}}"
}
```

**Request Body:**
```json
{
  "firstname": "Mehedi",
  "lastname": "Hasan",
  "totalprice": 150,
  "depositpaid": false,
  "bookingdates": {
    "checkin": "2025-03-01",
    "checkout": "2025-04-01"
  },
  "additionalneeds": "Lunch"
}
```

**Response Example:**
```json
{
  "firstname": "Mehedi",
  "lastname": "Hasan",
  "totalprice": 150,
  "depositpaid": false,
  "bookingdates": {
    "checkin": "2025-03-01",
    "checkout": "2025-04-01"
  },
  "additionalneeds": "Lunch"
}


```
## Newman Report Summary
![image](https://github.com/user-attachments/assets/a1477f0e-b921-4cb9-8949-03e6cb2c9fc5)
![image](https://github.com/user-attachments/assets/34d99d29-548e-4cd2-ba02-82810792619c)
![image](https://github.com/user-attachments/assets/d9020261-5377-4830-afc4-109de00e909b)
![image](https://github.com/user-attachments/assets/1cf0c0db-e246-4d4a-85b6-ae8a85e54277)
![image](https://github.com/user-attachments/assets/a6672bac-0e61-48f7-b9eb-d5e224b28bcc)


## Author
**Mehedi Hasan Soumik**

