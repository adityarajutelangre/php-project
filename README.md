# PHP Signup Form with MySQL Database

## 1. Introduction

This project is a simple signup form developed using **HTML, PHP, and MySQL**. It collects user information through a web form and submits the information to a PHP script. The PHP script connects to the MySQL database and attempts to insert the submitted details into the `users` table.

The form collects the user's name, email address, website, comment, and gender. After submission, the page displays a success message and the submitted information if the database query succeeds.

### Technologies Used

- **HTML:** Creates the signup form.
- **PHP:** Processes the submitted form data.
- **MySQL:** Stores the submitted information.
- **MySQLi:** Connects PHP to the MySQL database.
- **PM2 (optional):** Can manage a PHP development-server process in the background.

## 2. Architecture Diagram

```text
+---------------------------+
|       User / Browser      |
+-------------+-------------+
              |
              | Enters signup details
              v
+---------------------------+
|       signup.html         |
|  HTML Signup Form         |
+-------------+-------------+
              |
              | HTTP POST
              v
+---------------------------+
|        submit.php         |
|  Reads and processes data |
+-------------+-------------+
              |
              | MySQLi connection
              v
+---------------------------+
|       MySQL Database      |
|       Database: FCT       |
|       Table: users        |
+-------------+-------------+
              |
              | Query result
              v
+---------------------------+
| Browser Result Page       |
| Success message or error  |
+---------------------------+
```

## 3. Input

The user enters the following information in the signup form:

| Field | Description | Requirement |
|---|---|---|
| Name | User's name | Required |
| Email | User's email address | Required |
| Website | User's website URL | Optional |
| Comment | Additional message | Optional |
| Gender | Female, Male, or Other | Required |

### Example Input

```text
Name: Aditya
Email: aditya@example.com
Website: https://example.com
Comment: I am interested in this service.
Gender: Male
```

The form sends the data to `submit.php` using the HTTP `POST` method.

## 4. Background Process

### How the Application Works

1. The user opens `signup.html` in a browser through a PHP-enabled web server.
2. The user fills in the form fields.
3. The browser sends the form data to `submit.php` using `POST`.
4. PHP reads the submitted values from `$_POST`.
5. PHP connects to the MySQL database named `FCT`.
6. The script attempts to insert the data into the `users` table.
7. The browser displays a success message and the submitted information, or an error message.
8. PHP closes the database connection.

### PM2 Background Process (Optional)

PM2 is mainly designed for managing Node.js applications, but it can also manage a command such as PHP's built-in development server. Use this only for local development or testing, not as a replacement for a production web server.

After installing Node.js and PM2, and ensuring PHP is installed, run these commands from the project directory:

```bash
pm2 start "php -S 127.0.0.1:8000" --name php-signup
pm2 status
pm2 logs php-signup
pm2 stop php-signup
```

Open the application at:

```text
http://127.0.0.1:8000/signup.html
```

**Note:** PM2 must be installed separately. The PHP command must be available in your system's PATH. For production, use a suitable web server such as Apache or Nginx with PHP configured.

## 5. Output

### Successful Output

If the database connection and insert query succeed, the browser displays:

```text
✅ New record created successfully!

Submitted Information:
- Name: Aditya
- Email: aditya@example.com
- Website: https://example.com
- Comment: I am interested in this service.
- Gender: Male
```

The example above illustrates the expected output; actual values depend on the form submission.

### Error Output

If the database connection or insert query fails, the application displays an error message. Check that the MySQL server is running, the `FCT` database exists, the `users` table exists, and the connection credentials are correct.

## 6. Database Setup

The PHP script expects the following database configuration:

```text
Database name: FCT
Table name: users
Columns: name, email, website, comment, gender
```

Example table creation SQL:

```sql
CREATE DATABASE IF NOT EXISTS FCT;
USE FCT;

CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL,
    website VARCHAR(255),
    comment TEXT,
    gender VARCHAR(20) NOT NULL
);
```

Make sure your local MySQL username and password match the values configured in `submit.php`.

## 7. Important Security Notes

- Use a **prepared statement** instead of inserting raw form values directly into an SQL query. This helps prevent SQL injection.
- Validate and sanitize submitted values on the server.
- Keep database credentials out of public repositories; use environment variables or a private configuration file.
- Disable `display_errors` in production.
- The current code displays the SQL query in an error message. Avoid exposing SQL details to users in production.
- Ensure the database table and column names match the code.

## 8. Conclusion

This project demonstrates how an HTML form can send user information to PHP and how PHP can connect to MySQL to store that information. It provides a beginner-friendly example of form handling, database connectivity, and displaying a submission result. The project can be improved by adding prepared statements, stronger validation, and production-ready server configuration.
