# postman_tests

## Create requests in Postman

**Protocol:** `http` <br>
**IP:** `162.55.220.72` <br>
**Port:** `5007` <br>

### Task: EP_1

**Method:** `GET` <br>
**EndPoint:** `/get_method` <br>
**Request <u>url params</u>:** `name: str, age: int`

**Response:**

```json
[ "Str", "Str" ]
```

<details><summary><b>Solution</b></summary>

Send GET request with Query Params

```http request
http://162.55.220.72:5007/get_method?name=Mark&age=25
```
Query Params
```text
?name=Mark&age=25
```

**Response:**

```json
[ "Mark", "25" ]
```

<details><summary><b>Tests</b></summary>

```js
const testName = 'EP_1 -';
const reqData = pm.request.url.query;     // Getting the request parameters
const responseData = pm.response.json();  // Getting the JSON data from the response

pm.test(`${testName} Step - 1 - Check request parameters`, function () {
  pm.expect(reqData.get('name')).to.be.a('string');            // Checking that the 'name' parameter is a string
  pm.expect(parseInt(reqData.get('age'))).to.be.a('number');   // Checking that the 'age' parameter is a number
});

pm.test(`${testName} Step - 2 - Response status code is 200`, function () {
  pm.response.to.have.status(200);    // Checking that the response status code is 200
});

pm.test(`${testName} Step - 3 - The array in the response is equal to [ 'str', 'str' ]`, function () {
  pm.expect(responseData).to.be.an('array');      // Checking that the response data is an array
  pm.expect(responseData).to.have.lengthOf(2);    // Checking that the array has a length of 2
  pm.expect(responseData[0]).to.be.a('string');   // Checking that the first element of the array is a string
  pm.expect(responseData[1]).to.be.a('string');   // Checking that the second element of the array is a string
});

pm.test(`${testName} Step - 4 - Check for empty values in an array`, function () {
  for (let i = 0; i < responseData.length; i++) {     // Checking that each element of the array is not empty
    pm.expect(responseData[i]).to.not.be.empty;
  }
});
```
</details>
</details>


==================

### Task: EP_2

**Method:** `POST` <br>
**EndPoint:** `/user_info_3` <br>
**Request <u>form data</u>:** `name: str, age: int, salary: int` 

**Response:**

```json
{
  "name": "name",
  "age": "age",
  "salary": "salary",
  "family": {
    "children": [ [ "Alex", 24 ], [ "Kate", 12 ] ],
    "u_salary_1_5_year": salary * 4
  }
}
```

<details><summary><b>Solution</b></summary>

```http request
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

<details><summary><b>Tests</b></summary>

```js
const testName = 'EP_2 - Step -';

const requestData = pm.request.body;
const responseData = pm.response.json();

pm.test(`${testName} 1 - Check if mode is formdata`, function () {
  pm.expect(requestData.mode).to.eql("formdata");
});

pm.test(`${testName} 2 - Check formdata values`, function () {
  pm.expect(requestData.formdata).to.not.be.undefined;    // Checking that formData exists
  pm.expect(requestData.formdata).to.not.be.empty;        // Checking that formData is not an empty object
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

  let expectedTypes = {
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

  pm.expect(responseData).to.eql(expectedTypes);
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

==================

### Task: EP_3

**Method:** `GET` <br>
**EndPoint:** `/object_info_1` <br>
**Request <u>url params</u>:** `name: str, age: int ,weight: int`

**Response:**

```json
{
    "name": name,
    "age": age,
    "daily_food": weight * 0.012,
    "daily_sleep": weight * 2.5
}
```

<details><summary><b>Solution</b></summary>

Send GET request with Query Params
```http request
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

<details><summary><b>Tests</b></summary>

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

    let expectedTypes = {
        "name": name,
        "age": age,
        "daily_food": daily_food,
        "daily_sleep": daily_sleep
    };

    pm.expect(responseData).to.eql(expectedTypes);
});
```

</details>
</details>

==================

### Task: EP_4
**Method:** `GET` <br>
**EndPoint:** `/object_info_2` <br>
**Request <u>url params</u>:** `name: str, age: int, salary: int`

**Response:**
```json
{
  "start_qa_salary": salary,
  "qa_salary_after_6_months": salary * 2,
  "qa_salary_after_12_months": salary * 2.7,
  "qa_salary_after_1.5_year": salary * 3.3,
  "qa_salary_after_3.5_years": salary * 3.8,
  "person": {
    "u_name": [user_name, salary, age],
    "u_age": age,
    "u_salary_5_years": salary * 4.2
  }
}
```

<details><summary><b>Solution</b></summary>

Send GET request with Query Params
```http request
http://162.55.220.72:5007/object_info_2?name=Mark&age=25&salary=12500
```

Query Params
```text
?name=Mark&age=25&salary=12500
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

<details><summary><b>Tests</b></summary>

```js

```

</details>
</details>

==================

### Task: EP_5
**Method:** `GET` <br>
**EndPoint:** `/object_info_3` <br>
**Request <u>url params</u>:** `name: str, age: int, salary: int`

**Response:**
```json
{
  "name": name,
  "age": age,
  "salary": salary,
  "family": {
    "children": [["Alex", 24], ["Kate", 12]],
    "pets": {
      "cat": { "name": "Sunny", "age": 3 },
      "dog": { "name": "Luky",  "age": 4 }
    },
    "u_salary_1_5_year": salary * 4
  }
}
```

<details><summary><b>Solution</b></summary>

Send GET request with Query Params
```http request
http://162.55.220.72:5007/object_info_3?name=Mark&age=25&salary=12500
```

Query Params
```text
?name=Mark&age=25&salary=12500
```

**Response:**
```json
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

<details><summary><b>Tests</b></summary>

```js

```

</details></details>

==================

### Task: EP_6
**Method:** `GET` <br>
**EndPoint:** `/object_info_4` <br>
**Request <u>url params</u>:** `name: str, age: int, salary: int`

**Response:**
```json
{
  "name": name,
  "age": int(age),
  "salary": [salary, str(salary * 2), str(salary * 3)]
}
```

<details><summary><b>Solution</b></summary>

Send GET request with Query Params
```http request
http://162.55.220.72:5007/object_info_4?name=Mark&age=25&salary=12500
```

Query Params
```text
?name=Mark&age=25&salary=12500
```

**Response:**
```json
{
  "age": 25,
  "name": "Mark",
  "salary": [ 12500, "25000", "37500" ]
}
```

<details><summary><b>Tests</b></summary>

```js

```

</details></details>

==================

### Task: EP_7
**Method:** `POST` <br>
**EndPoint:** `/user_info_2` <br>
**Request <u>form data</u>:** `name: str, age: int, salary: int`

**Response:**

```json
{
  "start_qa_salary": salary,
  "qa_salary_after_6_months": salary * 2,
  "qa_salary_after_12_months": salary * 2.7,
  "qa_salary_after_1.5_year": salary * 3.3,
  "qa_salary_after_3.5_years": salary * 3.8,
  "person": {
    "u_name": [user_name, salary, age],
    "u_age": age,
    "u_salary_5_years": salary * 4.2
  }
}
```

<details><summary><b>Solution</b></summary>

```http request
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

```json
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

<details><summary><b>Tests</b></summary>

```js

```

</details></details>
