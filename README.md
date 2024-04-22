## Postman_part_1

**Protocol:** `http` <br>
**IP:** `162.55.220.72` <br>
**Port:** `5007` <br>

### 📜 Task: EP_1

**Method:** `GET` <br>
**EndPoint:** `/get_method` <br>
**Request <u>url params</u>:** `name: str, age: int`

**Response:**

```json
[ "Str", "Str" ]
```

<details>
<summary><a><b>📋 Solution</b></a></summary>

Send GET request with Query Params

```url
http://162.55.220.72:5007/get_method?name=Mark&age=25
```

📋 Query Params

```text
?name=Mark&age=25
```

**Response:**

```json5
[ "Mark", "25" ]
```

<details>
<summary><a><b>📊 Tests</b></a></summary>

```js
const reqName = 'EP_1 - ';
const reqData = pm.request.url.query;     // Getting the request parameters
const responseData = pm.response.json();  // Getting the JSON data from the response

pm.test(`${reqName} Step - 1 - Response status code is 200`, function () {
  pm.response.to.have.status(200);        // Checking that the response status code is 200
});

pm.test(`${reqName} Step - 2 - Response matches the expected response`, function () {

  let age = String(reqData.find(param => param.key === "age").value);
  let name = String(reqData.find(param => param.key === "name").value);

  let expectedResponse = [ name, age ];

  pm.expect(responseData).to.eql(expectedResponse);
});
```
</details>
</details>

<hr>

### 📜 Task: EP_2

**Method:** `POST` <br>
**EndPoint:** `/user_info_3` <br>
**Request <u>form data</u>:** `name: str, age: int, salary: int` 

**Response:**

```json5
{
  "name": "name",                                   // name 
  "age": "age",                                     // age 
  "salary": "salary",                               // salary
  "family": {
    "children": [ [ "Alex", 24 ], [ "Kate", 12 ] ],
    "u_salary_1_5_year": "u_salary_1_5_year"        // salary * 4
  }
}
```

<details>
<summary><a><b>📋 Solution</b></a></summary>

```url
http://162.55.220.72:5007/user_info_3
```

**Body:** `form-data`

```json5
{
  mode: "formdata",
  formdata: [
    { key: "name",    value: "Mark",  type: "text" },
    { key: "age",     value: "25",    type: "text" },
    { key: "salary",  value: "12500", type: "text" }
  ],
  options: {
    raw: {
      language: "json"
    }
  }
}
```

**Response:**

```json
{
  "age": "25",
  "family": {
    "children": [ [ "Alex", 24 ], [ "Kate", 12 ] ],
    "u_salary_1_5_year": 50000
  },
  "name": "Mark",
  "salary": 12500
}
```

<details>
<summary><a><b>📊 Tests</b></a></summary>

```js
const testName = 'EP_2 - Step -';

const requestData = pm.request.body;
const responseData = pm.response.json();

pm.test(`${testName} 1 - Check if mode is formdata`, function () {
  pm.expect(requestData.mode).to.eql("formdata");
});

pm.test(`${testName} 2 - Check formdata values`, function () {
  pm.expect(requestData.formdata).to.not.be.undefined; // Checking that formData exists
  pm.expect(requestData.formdata).to.not.be.empty; // Checking that formData is not an empty object
});

pm.test(`${testName} 3 - Response status code is 200`, function () {
  pm.response.to.have.status(200);
});

pm.test(`${testName} 4 - Response properties is not undefined`, function () {
  pm.expect(responseData).to.not.be.undefined;
});

pm.test(`${testName} 5 - Response properties is not empty`, function () {
  pm.expect(responseData).to.not.be.empty;
});

pm.test(`${testName} 6 - Response matches the expected response`, function () {
  let reqData = requestData.formdata;

  let age = String(reqData.find(param => param.key === "age").value);
  let name = String(reqData.find(param => param.key === "name").value);
  let salary = Number(reqData.find(param => param.key === "salary").value);
  let u_salary_1_5_year = salary * 4;

  let expectedResponse = {
    "age": age,
    "name": name,
    "salary": salary,
    "family": {
      "children": [
        ["Alex", 24],
        ["Kate", 12]
      ],
      "u_salary_1_5_year": u_salary_1_5_year
    }
  };

  pm.expect(responseData).to.eql(expectedResponse);
});

pm.test(`${testName} 7 - Checking for required expected keys`, function () {
  pm.expect(responseData).to.have.property('age');
  pm.expect(responseData).to.have.property('family').that.is.an('object');
  pm.expect(responseData.family).to.have.property('children').that.is.an('array');
  pm.expect(responseData.family).to.have.property('u_salary_1_5_year');
  pm.expect(responseData).to.have.property('name');
  pm.expect(responseData).to.have.property('salary');
});
```

</details>
</details>

<hr>

### 📜 Task: EP_3

**Method:** `GET` <br>
**EndPoint:** `/object_info_1` <br>
**Request <u>url params</u>:** `name: str, age: int ,weight: int`

**Response:**

```json5
{
    "name": "name",               // name
    "age": "age",                 // age
    "daily_food": "daily_food",   // weight * 0.012
    "daily_sleep": "daily_sleep"  // weight * 2.5
}
```

<details>
<summary><a><b>📋 Solution</b></a></summary>

Send GET request with Query Params
```url
http://162.55.220.72:5007/object_info_1?name=Mark&weight=45
```

Query Params
```text
?name=Mark&age=25&weight=45
```

**Response:**
```json
{
  "age": 25,
  "daily_food": 0.54,
  "daily_sleep": 112.5,
  "name": "Mark"
}
```

<details>
<summary><a><b>📊 Tests</b></a></summary>

```js
const testName = 'EP_3 - ';
const reqData = pm.request.url.query;     // Getting the request parameters
const responseData = pm.response.json();  // Getting the JSON data from the response
console.log(reqData)

pm.test(`${testName} Step - 1 - Response status code is 200`, function () {
    pm.response.to.have.status(200);    // Checking that the response status code is 200
});

pm.test(`${testName} Step - 2 - Response matches the expected response`, function () {

    let age = Number(reqData.find(param => param.key === "age").value);
    let name = String(reqData.find(param => param.key === "name").value);
    let weight = Number(reqData.find(param => param.key === "weight").value);
    let daily_food = weight * 0.012;
    let daily_sleep = weight * 2.5;

    let expectedResponse = {
        "name": name,
        "age": age,
        "daily_food": daily_food,
        "daily_sleep": daily_sleep
    };

    pm.expect(responseData).to.eql(expectedResponse);
});
```

</details>
</details>

<hr>

### 📜 Task: EP_4

**Method:** `GET` <br>
**EndPoint:** `/object_info_2` <br>
**Request <u>url params</u>:** `name: str, age: int, salary: int`

**Response:**
```json5
{
  "start_qa_salary": "start_qa_salary",                       // salary,
  "qa_salary_after_6_months": "qa_salary_after_6_months",     // salary * 2
  "qa_salary_after_12_months": "qa_salary_after_12_months",   // salary * 2.7
  "qa_salary_after_1.5_year": "qa_salary_after_1.5_year",     // salary * 3.3
  "qa_salary_after_3.5_years": "qa_salary_after_3.5_years",   // salary * 3.8
  "person": {
    "u_name": ["user_name", "salary", "age"],                 // user_name, salary, age
    "u_age": "age",                                           // age
    "u_salary_5_years": "u_salary_5_years",                   // salary * 4.2
  }
}
```

<details>
<summary><a><b>📋 Solution</b></a></summary>

Send GET request with Query Params
```url
http://162.55.220.72:5007/object_info_2?name=Mark&age=25&salary=12500
```

Query Params
```text
?name=Mark&age=25&salary=12500
```

**Response:**
```json5
{
  "person": {
    "u_age": 25,
    "u_name": [
      "Mark",
      12500,
      25
    ],
    "u_salary_5_years": 52500.0
  },
  "qa_salary_after_1.5_year": 41250.0,
  "qa_salary_after_12_months": 33750.0,
  "qa_salary_after_3.5_years": 47500.0,
  "qa_salary_after_6_months": 25000,
  "start_qa_salary": 12500
}
```

<details>
<summary><a><b>📊 Tests</b></a></summary>

```js
const reqName = 'EP_4 - ';
const reqData = pm.request.url.query;     // Getting the request parameters
const responseData = pm.response.json();  // Getting the JSON data from the response
console.log(reqData)

pm.test(`${reqName} Step - 1 - Response status code is 200`, function () {
    pm.response.to.have.status(200);    // Checking that the response status code is 200
});

pm.test(`${reqName} Step - 2 - Response matches the expected response`, function () {

    let age = Number(reqData.find(param => param.key === "age").value);
    let name = String(reqData.find(param => param.key === "name").value);
    let salary = Number(reqData.find(param => param.key === "salary").value);
    
    let u_salary_5_years = Number((salary * 4.2).toFixed(1));
    let qa_salary_after_1_5_year = Number((salary * 3.3).toFixed(1));
    let qa_salary_after_12_months = Number((salary * 2.7).toFixed(1));
    let qa_salary_after_3_5_years = Number((salary * 3.8).toFixed(1));
    let qa_salary_after_6_months = Number(salary * 2);

    let expectedResponse = {
        "person": {
            "u_age": age,
            "u_name": [ 
                name, 
                salary, 
                age 
            ],
            "u_salary_5_years": u_salary_5_years
        },
        "qa_salary_after_1.5_year": qa_salary_after_1_5_year,
        "qa_salary_after_12_months": qa_salary_after_12_months,
        "qa_salary_after_3.5_years": qa_salary_after_3_5_years,
        "qa_salary_after_6_months": qa_salary_after_6_months,
        "start_qa_salary": salary
    };

    pm.expect(responseData).to.deep.eql(expectedResponse);
});
```

</details>
</details>

<hr>

### 📜 Task: EP_5

**Method:** `GET` <br>
**EndPoint:** `/object_info_3` <br>
**Request <u>url params</u>:** `name: str, age: int, salary: int`

**Response:**
```json5
{
  "name": "name",                             // name
  "age": "age",                               // age
  "salary": "salary",                         // salary
  "family": {
    "children": [["Alex", 24], ["Kate", 12]],
    "pets": {
      "cat": { "name": "Sunny", "age": 3 },
      "dog": { "name": "Luky",  "age": 4 }
    },
    "u_salary_1_5_year": "u_salary_1_5_year"  // salary * 4
  }
}
```

<details>
<summary><a><b>📋 Solution</b></a></summary>

Send GET request with Query Params
```url
http://162.55.220.72:5007/object_info_3?name=Mark&age=25&salary=12500
```

Query Params
```text
?name=Mark&age=25&salary=12500
```

**Response:**
```json5
{
  "age": "25",
  "family": {
    "children": [
      [ "Alex", 24 ],
      [ "Kate", 12 ]
    ],
    "pets": {
      "cat": { "age": 3, "name": "Sunny" },
      "dog": { "age": 4, "name": "Luky" }
    },
    "u_salary_1_5_year": 50000
  },
  "name": "Mark",
  "salary": 12500
}
```

<details>
<summary><a><b>📊 Tests</b></a></summary>

```js
const reqName = 'EP_5 - ';
const reqData = pm.request.url.query;     // Getting the request parameters
const responseData = pm.response.json();  // Getting the JSON data from the response

pm.test(`${reqName} Step - 1 - Response status code is 200`, function () {
    pm.response.to.have.status(200);    // Checking that the response status code is 200
});

pm.test(`${reqName} Step - 2 - Response matches the expected response`, function () {

    let age = String(reqData.find(param => param.key === "age").value);
    let name = String(reqData.find(param => param.key === "name").value);
    let salary = Number(reqData.find(param => param.key === "salary").value);

    let u_salary_1_5_year = Number(salary * 4);

    let expectedResponse = {
        "age": age,
        "family": {
            "children": [ [ "Alex", 24 ], [ "Kate", 12 ] ],
            "pets": {
                "cat": { "age": 3, "name": "Sunny" },
                "dog": { "age": 4, "name": "Luky" }
            },
            "u_salary_1_5_year": u_salary_1_5_year
        },
        "name": name,
        "salary": salary
    };

    pm.expect(responseData).to.eql(expectedResponse);
});
```

</details>
</details>

<hr>

### 📜 Task: EP_6

**Method:** `GET` <br>
**EndPoint:** `/object_info_4` <br>
**Request <u>url params</u>:** `name: str, age: int, salary: int`

**Response:**
```json5
{
  "name": "name",   // name
  "age": "age",     // int(age)
  "salary": [
    "salary", 
    "salary_1", 
    "salary_2"]     // salary, salary_1 = str(salary * 2), salary_2 = str(salary * 3)
}
```

<details>
<summary><a><b>📋 Solution</b></a></summary>

Send GET request with Query Params
```url
http://162.55.220.72:5007/object_info_4?name=Mark&age=25&salary=12500
```

Query Params
```text
?name=Mark&age=25&salary=12500
```

**Response:**
```json5
{
  "age": 25,
  "name": "Mark",
  "salary": [ 12500, "25000", "37500" ]
}
```

<details>
<summary><a><b>📊 Tests</b></a></summary>

```js
const reqName = 'EP_6 - ';
const reqData = pm.request.url.query;     // Getting the request parameters
const responseData = pm.response.json();  // Getting the JSON data from the response

pm.test(`${reqName} Step - 1 - Response status code is 200`, function () {
    pm.response.to.have.status(200);    // Checking that the response status code is 200
});

pm.test(`${reqName} Step - 2 - Response matches the expected response`, function () {

    let age = Number(reqData.find(param => param.key === "age").value);
    let name = String(reqData.find(param => param.key === "name").value);
    let salary = Number(reqData.find(param => param.key === "salary").value);

    let salary_prop_1 = String(salary * 2);
    let salary_prop_2 = String(salary * 3);

    let expectedResponse = {
        "age": age,
        "name": name,
        "salary": [ salary, salary_prop_1, salary_prop_2 ]
    };

    pm.expect(responseData).to.eql(expectedResponse);
});
```

</details>
</details>

<hr>

### 📜 Task: EP_7

**Method:** `POST` <br>
**EndPoint:** `/user_info_2` <br>
**Request <u>form data</u>:** `name: str, age: int, salary: int`

**Response:**

```json5
{
  "start_qa_salary": "start_qa_salary",                       // salary,
  "qa_salary_after_6_months": "qa_salary_after_6_months",     // salary * 2
  "qa_salary_after_12_months": "qa_salary_after_12_months",   // salary * 2.7
  "qa_salary_after_1.5_year": "qa_salary_after_1.5_year",     // salary * 3.3
  "qa_salary_after_3.5_years": "qa_salary_after_3.5_years",   // salary * 3.8
  "person": {
    "u_name": ["name", "salary", "age"],                      // name, salary, age
    "u_age": "age",                                           // age
    "u_salary_5_years": "u_salary_5_years",                   // salary * 4.2
  }
}
```

<details>
<summary><a><b>📋 Solution</b></a></summary>

```url
http://162.55.220.72:5007/user_info_2
```
**Body:** `form-data`

```json5
{
  mode: "formdata",
  formdata: [
    { key: "name",    value: "Mark",  type: "text" },
    { key: "age",     value: "25",    type: "text" },
    { key: "salary",  value: "12500", type: "text" }
  ],
  options: {
    raw: {
      language: "json"
    }
  }
}
```

**Response:**

```json5
{
    "person": {
        "u_age": 25,
        "u_name": [ "Mark", 12500, 25 ],
        "u_salary_5_years": 52500.0
    },
    "qa_salary_after_1.5_year": 41250.0,
    "qa_salary_after_12_months": 33750.0,
    "qa_salary_after_3.5_years": 47500.0,
    "qa_salary_after_6_months": 25000,
    "start_qa_salary": 12500
}
```

<details>
<summary><a><b>📊 Tests</b></a></summary>

```js
const reqName = 'EP_7 - ';
const reqData = pm.request.body.formdata;     // Getting the request parameters
const responseData = pm.response.json();      // Getting the JSON data from the response

pm.test(`${reqName} Step - 1 - Response status code is 200`, function () {
    pm.response.to.have.status(200);          // Checking that the response status code is 200
});

pm.test(`${reqName} Step - 2 - Response matches the expected response`, function () {

    let age = Number(reqData.find(param => param.key === "age").value);
    let name = String(reqData.find(param => param.key === "name").value);
    let salary = Number(reqData.find(param => param.key === "salary").value);
    
    let u_salary_5_years = Number((salary * 4.2).toFixed(1));
    let qa_salary_after_1_5_year = Number((salary * 3.3).toFixed(1));
    let qa_salary_after_12_months = Number((salary * 2.7).toFixed(1));
    let qa_salary_after_3_5_years = Number((salary * 3.8).toFixed(1));
    let qa_salary_after_6_months = Number(salary * 2);

    let expectedResponse = {
        "person": {
            "u_age": age,
            "u_name": [ 
                name, 
                salary, 
                age 
            ],
            "u_salary_5_years": u_salary_5_years
        },
        "qa_salary_after_1.5_year": qa_salary_after_1_5_year,
        "qa_salary_after_12_months": qa_salary_after_12_months,
        "qa_salary_after_3.5_years": qa_salary_after_3_5_years,
        "qa_salary_after_6_months": qa_salary_after_6_months,
        "start_qa_salary": salary
    };

    pm.expect(responseData).to.eql(expectedResponse);
});
```

</details>
</details>
