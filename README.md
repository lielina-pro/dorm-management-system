#  Dormitory Management System

<div align="center">

![Status](https://img.shields.io/badge/Status-In%20Development-blue?style=flat-square)
![PHP](https://img.shields.io/badge/Backend-PHP-777BB4?style=flat-square&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/Database-MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![HTML](https://img.shields.io/badge/Frontend-HTML%20%7C%20CSS%20%7C%20JS-E34F26?style=flat-square&logo=html5&logoColor=white)
![Railway](https://img.shields.io/badge/Backend%20Host-Railway-0B0D0E?style=flat-square&logo=railway&logoColor=white)
![Vercel](https://img.shields.io/badge/Frontend%20Host-Vercel-000000?style=flat-square&logo=vercel&logoColor=white)

**A web-based dormitory management platform developed for Hawassa University IOT Campus.**  
Digitalizing room assignments, student management, proctor operations, and complaint handling across 7 dormitory blocks.

</div>

---

##  Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [System Architecture](#-system-architecture)
- [Getting Started](#-getting-started)
- [Deployment](#-deployment)
- [Database Setup](#-database-setup)
- [User Roles](#-user-roles)
- [Project Structure](#-project-structure)
- [Team](#-team)
- [License](#-license)

---

##  Overview

The Dormitory Management System is an academic project built for **Hawassa University IOT Campus** to replace manual dormitory processes with a fully digital platform.

The system manages **7 dormitory blocks** with a total of **2,975 rooms**, supporting up to **5 students per room**. It serves three types of users — students, proctors, and an admin — each with their own dedicated dashboard and feature set.

| Blocks | Gender | Rooms per Block | Total Rooms |
|--------|--------|-----------------|-------------|
| 1, 2, 5, 6, 7 | Male | 425 | 2,125 |
| 3, 4 | Female | 425 | 850 |
| **Total** | | | **2,975** |

---

##  Features

###  Student
- Login using Full Name and Student ID (e.g. `2715/16`)
- View assigned dorm block, room number, and roommates
- Submit complaints or petitions with category selection and description

###  Proctor
- Dashboard showing total, assigned, and free rooms in assigned block
- Look up dorm members by block and room number
- Search a student's dorm by name
- Manage annual dorm key checklist per room
- View and respond to assigned tasks with solved / unsolved status
- Send comments directly to admin

###  Admin
- Full system overview dashboard with live statistics
- Assign, move, and remove students from rooms
- Assign and reassign proctors to blocks (gender-matched, max 4 per block)
- Manage rooms — create, update, and delete (only when empty)
- View all student complaints and reply directly
- Assign complaints as tasks to proctors with instructions and deadlines
- Receive task completion or failure notifications from proctors and reassign if needed
- In-app notification center with unread badge counter
- Export reports (room assignments, proctor assignments, complaint and task history) as PDF or Excel

---

##  Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | HTML5, CSS3, JavaScript |
| Backend | PHP |
| Database | MySQL |
| Frontend Hosting | Vercel |
| Backend & DB Hosting | Railway |
| PDF Export | mPDF / TCPDF |
| Excel Export | PhpSpreadsheet |

---

##  System Architecture

```
┌─────────────────────────────────────────┐
│              Client Browser             │
│        HTML + CSS + JavaScript          │
│           (Hosted on Vercel)            │
└────────────────────┬────────────────────┘
                     │ HTTP Requests
┌────────────────────▼────────────────────┐
│              PHP Backend                │
│         REST API Endpoints              │
│          (Hosted on Railway)            │
└────────────────────┬────────────────────┘
                     │
┌────────────────────▼────────────────────┐
│             MySQL Database              │
│     12 Tables · 2,975 Rooms Seeded      │
│          (Hosted on Railway)            │
└─────────────────────────────────────────┘
```

---

##  Getting Started

### Prerequisites

Make sure you have the following installed locally for development:

- PHP >= 7.4
- MySQL >= 5.7
- A local server: [XAMPP](https://www.apachefriends.org/) or [Laragon](https://laragon.org/)
- [Composer](https://getcomposer.org/) (for PhpSpreadsheet and mPDF)
- Git

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-org/dorm-management-system.git

# 2. Navigate into the project
cd dorm-management-system

# 3. Install PHP dependencies
composer install

# 4. Set up your database config
cp config/db.example.php config/db.php
# Then open config/db.php and fill in your local DB credentials

# 5. Import the database schema
mysql -u root -p your_database_name < database/schema.sql

# 6. Seed the database (blocks, rooms, default admin)
mysql -u root -p your_database_name < database/seed.sql

# 7. Start your local server and open the project in your browser
```

---

##  Deployment

This project is deployed using two platforms:

### Frontend — Vercel
- Connect your GitHub repository to [Vercel](https://vercel.com)
- Vercel auto-deploys on every push to `main`

### Backend & Database — Railway
- Create a new project on [Railway](https://railway.app)
- Add a **PHP** service and a **MySQL** plugin
- Set your environment variables in Railway's dashboard (see `.env.example`)
- Import `database/schema.sql` and `database/seed.sql` via Railway's MySQL shell

### Environment Variables

Create a `.env` file locally based on `.env.example`:

```env
DB_HOST=your_db_host
DB_PORT=3306
DB_NAME=dorm_management
DB_USER=your_db_user
DB_PASS=your_db_password
APP_URL=https://your-vercel-url.vercel.app
```

> ⚠️ Never commit your `.env` or `config/db.php` files. They are listed in `.gitignore`.

---

## 🗄 Database Setup

The database contains **12 tables**:

| Table | Description |
|-------|-------------|
| `blocks` | 7 dorm blocks with gender designation |
| `rooms` | 2,975 rooms across all blocks |
| `students` | Student records with room and block assignment |
| `admin` | Single admin account |
| `proctors` | Proctor accounts with block assignment |
| `complaints` | Student-submitted complaints |
| `complaint_replies` | Admin replies to complaints |
| `tasks` | Tasks assigned by admin to proctors |
| `notifications` | In-app notifications for admin and proctors |
| `key_checklist` | Annual key distribution record per student |
| `proctor_comments` | Comments sent from proctors to admin |
| `password_resets` | Password reset tokens for admin and proctors |

To reset the full database:

```bash
mysql -u root -p your_database_name < database/schema.sql
mysql -u root -p your_database_name < database/seed.sql
```

---

##  User Roles

### Student Login
- Uses **Full Name + Student ID** (e.g. `Abebe Kebede` + `2715/16`)
- No registration — accounts are created by the admin

### Proctor Login
- Uses **Username + Password**
- Forgot password supported

### Admin Login
- Uses **Username + Password**
- Single admin account seeded in `seed.sql`
- Forgot password supported

---

##  Project Structure

```
dorm-management-system/
│
├── index.php
├── README.md
├── .gitignore
├── .env.example
├── composer.json
│
├── config/
│   ├── db.example.php
│   ├── db.php              ← (gitignored — add your own)
│   └── config.php
│
├── assets/
│   ├── css/
│   ├── js/
│   └── images/
│
├── pages/
│   ├── auth/
│   ├── student/
│   ├── proctor/
│   └── admin/
│
├── api/
│   ├── auth/
│   ├── students/
│   ├── rooms/
│   ├── proctors/
│   ├── complaints/
│   ├── tasks/
│   ├── notifications/
│   └── export/
│
├── includes/
│   ├── header.php
│   ├── footer.php
│   ├── navbar.php
│   └── session.php
│
└── database/
    ├── schema.sql
    └── seed.sql
```

---

##  Team

Developed by students of **Hawassa University — IOT Campus** as an academic web programming project.

| Name | Role | GitHub |
|------|------|--------|
| Firstname Lastname | Frontend Developer | [@username](https://github.com/username) |
| Firstname Lastname | Frontend Developer | [@username](https://github.com/username) |
| Firstname Lastname | Backend Developer | [@username](https://github.com/username) |
| Firstname Lastname | Backend Developer | [@username](https://github.com/username) |
| Firstname Lastname | Backend Developer | [@username](https://github.com/username) |

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).  
Developed for academic purposes at **Hawassa University IOT Campus**.

---

<div align="center">
  <sub>Built with  by the DMS Team · Hawassa University IOT Campus</sub>
</div>
