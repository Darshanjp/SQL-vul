## SQL Injection Vulnerability Lab (Django) - Fixed Version

This repository contains a simple Django-based web application where a previously existing SQL Injection vulnerability has been fixed. The application now securely handles user credentials during login by using Django's authenticate() method and parameterized queries.

## Overview

The application includes a secure login feature that verifies user credentials using Django's built-in authentication system. This eliminates the SQL Injection vulnerability that allowed attackers to manipulate the SQL query through user inputs.

## Features:

    1.Secure Login Form: Only allows users with valid credentials to log in.
    2.SQL Injection Prevention: User inputs are sanitized and processed using Django's ORM and authenticate() method.

## Key Changes and Fix
Vulnerability Fixed:

The application no longer accepts malicious SQL code in the login form. All user inputs are now validated and securely processed.
## Before Fix (Vulnerable Code):

The old implementation used raw SQL queries to verify credentials:

query = f"SELECT * FROM auth_user WHERE username='{username}' AND password='{password}'"
with connection.cursor() as cursor:
    cursor.execute(query)
    user = cursor.fetchone()

This approach allowed attackers to inject SQL commands, bypass authentication, and access the system.

## After Fix (Safe Code):

The new implementation uses Django's authenticate() method:

from django.contrib.auth import authenticate, login

def login_view(request):
    error = None
    if request.method == "POST":
        username = request.POST['username']
        password = request.POST['password']

        # Authenticate using Django's built-in method
        user = authenticate(request, username=username, password=password)

        if user is not None:
            # Log the user in and redirect to the blog list page
            login(request, user)
            return redirect('blog_list')
        else:
            # Display an error message for invalid credentials
            error = "Invalid username or password!"
    
    return render(request, 'app1/login.html', {'error': error})

    i)authenticate(): Safely checks user credentials, preventing SQL Injection by escaping inputs.
    ii)Error Messages: Provides a generic error message to avoid giving clues about existing usernames.

## Running the Application
Follow these steps to run the application locally:

### Prerequisites:
- Python 3 and pip installed on your system.
- Django installed (pip install django).
- SQLite database used for storage.

### Steps:
1. Clone the repository:
```bash
git clone https://github.com/Darshanjp/SQL-XSS-vul.git
cd sql-injection-lab
```
2. Create a virtual environment (optional but recommended):
```bash
 python3 -m venv venv
 source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
```
3. Install dependencies(Django):
```bash
pip install django
```
5. Run the Django application:
```bash
python manage.py runserver
```
The application should be running at http://127.0.0.1:8000/

### Access the Application:
Open your browser and navigate to http://127.0.0.1:8000/. You can now attempt the SQL Injection attack using the login form.

## How to Demonstrate the Vulnerability:
1. Navigate to the login page at http://127.0.0.1:8000/login/.
2. Enter the following in the Username field:
```sql
' OR 1=1 --
```
3. Enter any value in the Password field (e.g., anypassword).
4. Click "Login".
5. You will be logged in without proper credentials, demonstrating the SQL Injection vulnerability.
   
## Fixing the Vulnerability (Solution)
1.Open the application in your browser: http://127.0.0.1:8000/login/.
2.Log in using valid credentials (you can create users through the Django admin panel or database setup).
3.Invalid credentials will result in an error message: "Invalid username or password!"

### Example of Fix:
In the vulnerable view, replace the raw SQL query with Django's built-in authenticate() method that uses the ORM to safely query the database.

#### Before Fix (Vulnerable Code):

```python
query = f"SELECT * FROM auth_user WHERE username='{username}' AND password='{password}'"
with connection.cursor() as cursor:
    cursor.execute(query)
    user = cursor.fetchone()
```
#### After Fix (Safe Code):

```python
from django.contrib.auth import authenticate

user = authenticate(request, username=username, password=password)
if user is not None:
    # Proceed with login
    login(request, user)
Using the authenticate() method ensures that user input is properly escaped and prevents SQL Injection attacks.
```
**safecode.py** have the safe method to do the application without sql injection vulnerability. Copy that file and paste in views.py to domonstrate the mitigation

## Contributing
Feel free to open issues or submit pull requests for improvements. If you want to help fix the vulnerability or add additional features, your contributions are welcome!

