<div align="center">

# REST API Testing — Postman Project

**A hands-on API testing project built with Postman, covering GET/POST/PUT/DELETE and a good chunk of negative testing.**

![Postman](https://img.shields.io/badge/Postman-FF6C37?style=flat-square&logo=postman&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Status](https://img.shields.io/badge/status-completed-brightgreen?style=flat-square)
![Pass Rate](https://img.shields.io/badge/pass%20rate-100%25-success?style=flat-square)

</div>

---

## What this is

I built this to get real practice with API testing instead of just reading about it. I picked [JSONPlaceholder](https://jsonplaceholder.typicode.com) — a free mock REST API — and tested it properly: GET, POST, PUT, DELETE, each one with its own set of assertions written in Postman using `pm.test()`.

The part I actually spent the most time on was negative testing. Anyone can check that a 200 comes back when everything's fine. I wanted to see what happens when you send garbage — missing fields, null values, wrong data types, IDs that don't exist — and write assertions that catch what actually comes back, not what you'd assume comes back.

---

## Project layout

```
REST-API-Testing-Project/
│
├── postman/
│   ├── REST_API_Testing.postman_collection.json
│   └── REST_API_QA_Environment.postman_environment.json
│
├── documentation/
│   ├── Test_Execution_Report.md
│   └── Test_Execution_Report.pdf
│
└── README.md
```

The collection's split into 5 folders so you can run the whole thing or just one method at a time:

| Folder | What's inside |
|---|---|
| `01 - GET Requests` | Get all posts · get a single post · invalid post ID |
| `02 - POST Requests` | Create a post · missing required fields · invalid data types |
| `03 - PUT Requests` | Update a post · update with an invalid ID |
| `04 - DELETE Requests` | Delete a post · delete with an invalid ID |
| `05 - Negative Tests` | Invalid endpoint · invalid ID format · empty body · null values |

---

## What the negative testing actually turned up

This is the part worth reading. JSONPlaceholder is a mock API, so it doesn't validate input the way a real production API should — and that mismatch is more interesting than a clean pass:

- Sending an empty `{}` as the request body still returned `201 Created`
- `null` values for `title`, `body`, and `userId` were accepted without any complaint
- Wrong data types (a number where a string should be, etc.) got waved through too
- Trying to update a post with an ID that doesn't exist gave back a `500 Internal Server Error` instead of a proper `404`
- Deleting a post with a bad ID returned `200 OK`, as if it succeeded

None of these count as test failures — my assertions were written to match what the mock actually does, not what I wished it did. But they're exactly the kind of gap you'd flag before shipping a real API to production.

---

## Running it yourself

```bash
git clone https://github.com/Santhoshkumar-Test/REST-API-Testing-Project.git
```

1. Open Postman → **Import** → grab both files from the `postman/` folder (collection + environment)
2. Switch to the **REST API - QA Environment** in the environment dropdown
3. Check `baseUrl` is set to `https://jsonplaceholder.typicode.com`
4. Open the Collection Runner, pick the collection, hit **Run**

All 14 requests, 51 assertions, done in under 8 seconds.

---

## Results

<div align="center">

| Metric | Result |
|---|:---:|
| Requests | 14 |
| Assertions | 51 |
| Passed | 51 |
| Failed | 0 |
| Pass rate | 100% |
| Time taken | 7.99 sec |
| Avg response time | 381 ms |

</div>

The full breakdown — every request, every assertion, the whole negative-testing writeup — is in [`documentation/Test_Execution_Report.md`](documentation/Test_Execution_Report.md), or the [PDF](documentation/Test_Execution_Report.pdf) if that's easier to read.

---

## One of the actual test scripts

This is what's running on the "Create Post" request:

```javascript
pm.test("Status code should be 201", function () {
    pm.response.to.have.status(201);
});

pm.test("Response should contain generated ID", function () {
    const response = pm.response.json();
    pm.expect(response).to.have.property("id");
});

pm.test("Title should match request", function () {
    const response = pm.response.json();
    pm.expect(response.title).to.eql("QA API Test");
});
```

---

<div align="center">

**Boopathi T** · part of my QA/API testing practice work
[GitHub](https://github.com/Boopathi-Test)

</div>
