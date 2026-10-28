# Auth API
## Description
This API is built with `FastAPI` and is a demonstation of how to create API endpoints to manage secure registration of users and associated authentication for other endpoints.

Registration creates a user record in the database and stores the hashed password.
Login authenticates the user and generates a `JWT Token` on success. This token can be used to authenticate the user when using other secure endpoints.

`SQLite` is used and `auth.db` is created on start of the app(if it does not exist already)

## Python Version
`3.14.4`

## Get Started
### Create virtual environment
`python3 -m venv .venv`

### Install Packages
`python3 -m pip install -r requirements.txt`

### Create a secret key
`python -c "import secrets; print(secrets.token_hex(32))"`

### Set the JWT_SECRET before you run the app
`export JWT_SECRET="paste-your-generated-value-here"`

**Note: This secret is used in your running session. If you change it, restart your app and login again to generate a new token**

### Run the API
`uvicorn main:app --reload`

## Registration and Login
### Navigate to the API documentation
`http://127.0.0.1:8000/docs`

### Register
1. Navigate to `/auth/register`
2. Click "Try it out"
3. Use the below for the request body:
```
{
  "username": "Shane",
  "email": "shane@example.com",
  "password": "secure_password_123"
}
```
Expected Response Code: `201`

### Login
1. Navigate to `auth/login`
2. Click "Try it out"
3. Use the below for the request body:
```
{
  "email": "shane@example.com",
  "password": "secure_password_123"
}
```
Expected Response Code: `200`
Expected Response Body:
```
{
  "access_token": "generated-secret-token",
  "token_type": "bearer"
}
```

### Test a secure endpoint
1. Click "Authorize on the top right of the page
2. Paste the value for the access token from the login response
3. Click Authorize. You should see "Authorized" and a Lgout Button.
4. Click "Close"
4. Navigate to `profile/me`
5. Click "try it out"
6. Click execute

Expected response code: `200`
Expected response body:
```
{
  "id": "id-of-you-user",
  "username": "Shane",
  "email": "shane@example.com"
}
```

## Tests
### Run automated tests
`python -m pytest -v`

Expected Results:
```
tests/test_auth.py::test_password_hash PASSED                                                                                           [ 16%]
tests/test_auth.py::test_login_and_profile PASSED                                                                                       [ 33%]
tests/test_auth.py::test_profile_requires_token PASSED                                                                                  [ 50%]
tests/test_auth.py::test_duplicate_registration PASSED                                                                                  [ 66%]
tests/test_auth.py::test_invalid_registration_data PASSED                                                                               [ 83%]
tests/test_auth.py::test_login_wrong_password PASSED                                                                                    [100%]

============================================================== 6 passed in 0.58s ==============================================================
```

### Test Database
Test databases are created in a temporarily directory and does not affect the live application database in anyway.