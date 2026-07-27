# Ledger LMS

A complete Learning Management System built with **HTML, CSS, JavaScript, PHP (procedural, mysqli)** and **MySQL**. Three roles — Admin, Instructor, Student — each with their own dashboard.

## Features

**Admin**
- Dashboard with platform-wide stats
- Manage users (create, suspend/activate, delete)
- Manage courses (archive, delete, moderate)

**Instructor**
- Create/publish courses (draft or published)
- Add ordered lessons (text, notes, video URL)
- Create assignments with due dates and point values
- Post announcements to enrolled students
- Grade submissions with feedback
- Track enrolled students & their progress

**Student**
- Browse and search the course catalog
- Enroll in courses
- Work through lessons in order, mark them complete
- Automatic progress-percentage tracking per course
- Submit assignments (with optional file upload)
- See grades and instructor feedback

**Platform**
- Secure authentication with `password_hash` / `password_verify` (bcrypt)
- Prepared statements (mysqli) throughout — no SQL injection
- Session-based access control per role
- Responsive layout, works down to mobile

---

## 1. Requirements

- PHP 8.0+ with the `mysqli` extension (enabled by default in XAMPP / WAMP / MAMP)
- MySQL 5.7+ / MySQL 8.x (MySQL Workbench for management)
- A local server stack: **XAMPP**, **WAMP**, **MAMP**, or `php -S` built-in server

---

## 2. Database setup (MySQL Workbench)

1. Open **MySQL Workbench** and connect to your local MySQL server.
2. Go to **File → Run SQL Script...** (or open a new SQL tab and paste the file contents).
3. Select `database/lms.sql` from this project and run it.
   - This creates the `lms_db` database, all 8 tables, and seed data:
     - 1 admin, 1 instructor, 1 student account
     - 2 sample courses with lessons, an enrollment, and 2 assignments
4. **Important:** the seed accounts in `lms.sql` have placeholder password hashes that will not work as-is. After importing, run the password-seeding script once to set a real, working password:
   - Visit `http://localhost/lms/database/seed_passwords.php` in your browser (with your server running — see step 3 below), **or**
   - Run it from the command line: `php database/seed_passwords.php`
   - This sets the password for all 3 demo accounts to: **`password123`**
   - Delete `database/seed_passwords.php` afterward (it's not needed again).

Demo accounts after seeding:

| Role       | Email                    | Password      |
|------------|---------------------------|---------------|
| Admin      | admin@ledger.edu         | password123   |
| Instructor | instructor@ledger.edu    | password123   |
| Student    | student@ledger.edu       | password123   |

---

## 3. App setup

### Option A — XAMPP / WAMP / MAMP
1. Copy the entire `lms` folder into your server's web root (e.g. `htdocs/lms` for XAMPP).
2. Open `config/db.php` and confirm the credentials match your MySQL setup:
   ```php
   define('DB_HOST', 'localhost');
   define('DB_USER', 'root');
   define('DB_PASS', '');        // your MySQL root password, if any
   define('DB_NAME', 'lms_db');
   ```
3. Start Apache and MySQL from your control panel.
4. Visit `http://localhost/lms/` in your browser.

### Option B — PHP's built-in server (quick local testing)
```bash
cd lms
php -S localhost:8000
```
Then visit `http://localhost:8000/`.

---

## 4. Folder structure

```
lms/
├── admin/                  Admin panel pages
├── instructor/             Instructor panel pages
├── student/                Student panel pages
├── includes/                Shared PHP (auth, header/footer, helpers)
├── config/db.php            Database connection settings
├── assets/css/style.css     Global stylesheet
├── assets/js/script.js      Global JS (modals, confirms, alerts)
├── uploads/                 Student submissions & course materials
├── database/lms.sql         Full schema + seed data
├── database/seed_passwords.php  One-time password seeding helper
├── index.php                 Login page
├── register.php              Public registration (student/instructor)
└── logout.php
```

## 5. Security notes for production use

This is a learning/portfolio-grade project. Before deploying publicly:
- Set a strong MySQL password and update `config/db.php`
- Serve over HTTPS
- Add file-type/size validation on assignment uploads
- Add CSRF tokens to POST forms
- Turn off PHP error display (`display_errors = Off`) in production `php.ini`
