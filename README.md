# Barbados Hotel Management System

A comprehensive web-based hotel management system designed to streamline hotel operations, supplier management, and administrative tasks.

## 📋 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [System Architecture](#system-architecture)
- [Technology Stack](#technology-stack)
- [Database Schema](#database-schema)
- [Installation & Setup](#installation--setup)
- [How the System Works](#how-the-system-works)
- [User Roles](#user-roles)
- [Directory Structure](#directory-structure)
- [Usage Guide](#usage-guide)

## 🎯 Overview

Barbados Hotel Management System is a web application that facilitates hotel management operations including room bookings, supplier management, product procurement, and administrative oversight. The system supports multiple user roles with specific functionalities for each user type.

## ✨ Features

### For Visitors/Customers
- Browse hotel information and facilities
- View available rooms and amenities
- Contact hotel management
- Access booking information
- View hotel's social media presence

### For Suppliers
- Register and create supplier accounts
- Submit product applications
- View product postings from hotel
- Track application status (accepted/rejected)
- Manage supplier profile
- Upload supporting documents (PDF files)

### For Administrators
- Manage supplier applications
- Accept or reject supplier proposals
- Manage product listings
- View and manage all suppliers
- Monitor system operations
- Admin authentication and authorization

## 🏗️ System Architecture

The system follows a **3-tier architecture**:

```
┌─────────────────────────────────────┐
│     Presentation Layer (Frontend)   │
│   HTML, CSS, JavaScript              │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│     Application Layer (Backend)     │
│   PHP, Session Management            │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│     Data Layer (Database)           │
│   MySQL (barbados database)          │
└─────────────────────────────────────┘
```

### Data Flow Diagram

```
User → Frontend (HTML/CSS) → Backend (PHP) → Database (MySQL) → Backend → Frontend → User
```

## 💻 Technology Stack

- **Frontend:**
  - HTML5
  - CSS3
  - JavaScript

- **Backend:**
  - PHP 5.6+
  - Session Management

- **Database:**
  - MySQL/MariaDB

- **Development Environment:**
  - XAMPP/WAMP/LAMP Stack
  - phpMyAdmin

## 🗄️ Database Schema

The system uses the **barbados** database with the following tables:

### 1. **admin** Table
Stores administrator credentials and information.
```sql
- admin_id (INT, Primary Key)
- Names (VARCHAR)
- Email (VARCHAR)
- Username (VARCHAR)
- Password (VARCHAR)
```

### 2. **supplier** Table
Contains supplier registration data.
```sql
- sup_id (INT, Primary Key)
- sup_name (VARCHAR)
- national_id (VARCHAR)
- Address (VARCHAR)
- Phone (VARCHAR)
- Username (VARCHAR)
- Password (VARCHAR)
```

### 3. **product** Table
Manages product/supply items.
```sql
- pro_id (INT, Primary Key)
- pro_Name (VARCHAR)
- Quality (VARCHAR)
```

### 4. **application** Table
Tracks supplier applications for products.
```sql
- app_id (INT, Primary Key)
- file (VARCHAR) - Path to uploaded document
- sup_id (INT, Foreign Key)
- pro_id (INT, Foreign Key)
```

### 5. **accept_reject** Table
Records application approval/rejection decisions.
```sql
- id (INT, Primary Key)
- app_id (INT, Foreign Key)
- file (VARCHAR)
- sup_id (VARCHAR)
- pro_id (VARCHAR)
- status (VARCHAR) - 'accepted' or 'rejected'
```

## 🚀 Installation & Setup

### Prerequisites
- XAMPP/WAMP/LAMP server
- PHP 5.6 or higher
- MySQL/MariaDB
- Web browser

### Step-by-Step Installation

1. **Install XAMPP/WAMP**
   - Download and install XAMPP from [https://www.apachefriends.org](https://www.apachefriends.org)
   - Start Apache and MySQL services

2. **Clone/Download Project**
   ```bash
   # Place the project folder in htdocs directory
   C:\xampp\htdocs\Hotel-Management-System
   ```

3. **Import Database**
   - Open phpMyAdmin (http://localhost/phpmyadmin)
   - Create a new database named `barbados`
   - Import the `barbados.sql` file from the project root

4. **Configure Database Connection**
   - Open `pages/backend/connection.php`
   - Verify database credentials:
   ```php
   $conn = mysqli_connect("localhost", "root", "", "barbados");
   ```
   - Modify if your MySQL credentials differ

5. **Access the Application**
   - Open web browser
   - Navigate to: `http://localhost/Hotel-Management-System/index.html`

## 🔄 How the System Works

### System Workflow

#### 1. **Homepage Flow**
```
User visits index.html
    ↓
Views hotel information, mission, and social media
    ↓
Navigates to: About | Rooms | Contact | Admin | Supplier portals
```

#### 2. **Admin Workflow**
```
Admin → admin_home_page.php
    ↓
Login (admin_login.php)
    ↓
Dashboard Access
    ├── Manage Suppliers (manage_suppliers.php)
    ├── Manage Applications (manage_Applications.php)
    │       ├── Accept Application (accept_app.php → insert_accept.php)
    │       └── Reject Application (reject_app.php → insert_reject.php)
    ├── Manage Products (manage_product.php)
    ├── View Rejected Applications (rejected_app.php)
    └── Logout (admin_logout.php)
```

#### 3. **Supplier Workflow**
```
Supplier → sup_home_page.php
    ↓
Signup (sup_insert.php) OR Login
    ↓
Supplier Dashboard
    ├── View Product Posts (view_product_post.php)
    ├── Apply for Products (apply.php)
    │       └── Upload Documents (files/)
    ├── View Application Status (accept_reject.php)
    └── Logout (logout.php)
```

#### 4. **Application Processing Flow**
```
Supplier submits application (apply.php)
    ↓
Application stored in 'application' table
    ↓
Admin reviews (manage_Applications.php)
    ↓
Decision Made
    ├── Accept → accept_app.php → insert_accept.php → 'accept_reject' table
    └── Reject → reject_app.php → insert_reject.php → 'accept_reject' table
    ↓
Supplier views status (accept_reject.php)
```

### Session Management

The system uses PHP sessions to maintain user authentication:
- Admin sessions managed throughout admin pages
- Supplier sessions tracked across supplier modules
- Session data includes user credentials and access permissions

## 👥 User Roles

### 1. **Guest/Visitor**
- **Access Level:** Public
- **Pages:** Home, About, Rooms, Contact
- **Actions:** View information, browse content

### 2. **Supplier**
- **Access Level:** Authenticated
- **Registration:** Required via signup form
- **Capabilities:**
  - Submit product applications
  - Upload supporting documents
  - Track application status
  - View product requirements
  - Manage profile

### 3. **Administrator**
- **Access Level:** Highest
- **Authentication:** Username and password
- **Capabilities:**
  - Full system access
  - Manage suppliers
  - Review and process applications
  - Manage product listings
  - Accept/reject supplier proposals
  - Generate reports
  - System configuration

## 📁 Directory Structure

```
Hotel-Management-System/
│
├── index.html                 # Homepage
├── barbados.sql              # Database schema and data
├── readMe.txt                # Basic readme
├── README.md                 # This comprehensive documentation
│
├── pages/                    # Main pages directory
│   ├── About.html           # About us page
│   ├── booking.html         # Room booking interface
│   ├── booking.css          # Booking page styles
│   ├── contact.html         # Contact page
│   │
│   ├── css/                 # Stylesheets directory
│   │   ├── index.css        # Homepage styles
│   │   ├── about.css        # About page styles
│   │   ├── contact.css      # Contact page styles
│   │   ├── admin_home_page.css    # Admin dashboard styles
│   │   ├── longin_signup.css      # Authentication styles
│   │   ├── manage_suppliers.css   # Supplier management styles
│   │   └── Apply.css        # Application form styles
│   │
│   ├── images/              # Image assets directory
│   │   └── [hotel images]
│   │
│   └── backend/             # Backend PHP scripts
│       ├── connection.php   # Database connection
│       ├── admin_login.php  # Admin authentication
│       ├── admin_logout.php # Admin logout
│       ├── Admin_signup.php # Admin registration
│       ├── admin_home_page.php    # Admin dashboard
│       ├── login.php        # General login handler
│       ├── signup.php       # General signup handler
│       ├── insert_product.php     # Product insertion
│       ├── Ad_insert.php    # Admin insertion
│       │
│       ├── Admin/           # Admin-specific modules
│       │   ├── manage_Applications.php   # Application management
│       │   ├── manage_product.php        # Product management
│       │   ├── manage_suppliers.php      # Supplier management
│       │   ├── accept_app.php            # Accept application view
│       │   ├── reject_app.php            # Reject application view
│       │   ├── rejected_app.php          # Rejected applications list
│       │   ├── accept_rejected.php       # Status view
│       │   ├── display_pdf.php           # Document viewer
│       │   ├── update_sup.php            # Supplier update
│       │   │
│       │   └── insert_accept_reject/     # Decision processing
│       │       ├── insert_accept.php     # Process acceptance
│       │       └── insert_reject.php     # Process rejection
│       │
│       └── Supply/          # Supplier-specific modules
│           ├── sup_home_page.php         # Supplier dashboard
│           ├── sup_insert.php            # Supplier registration
│           ├── apply.php                 # Application submission
│           ├── accept_reject.php         # View application status
│           ├── view_product_post.php     # View posted products
│           ├── logout.php                # Supplier logout
│           └── files/                    # Uploaded documents storage
```

### Key Directories Explained

#### `/pages/backend/Admin/`
Contains all administrative functionalities including supplier management, application processing, and product management.

#### `/pages/backend/Supply/`
Houses supplier-related features such as registration, application submission, and status tracking.

#### `/pages/backend/Supply/files/`
Stores uploaded documents (PDF files) from suppliers during application process.

#### `/pages/css/`
Contains all cascading style sheets for consistent UI/UX across the application.

## 📖 Usage Guide

### For New Suppliers

1. **Registration:**
   - Navigate to Supplier portal
   - Click on signup
   - Fill in required details (name, national ID, address, phone, credentials)
   - Submit registration

2. **Submitting Applications:**
   - Login with credentials
   - View available product posts
   - Click "Apply" on desired products
   - Upload required documents (PDF format)
   - Submit application

3. **Tracking Status:**
   - Login to supplier dashboard
   - Navigate to "Application Status"
   - View current status (pending/accepted/rejected)

### For Administrators

1. **Login:**
   - Navigate to Admin portal
   - Enter admin credentials
   - Access admin dashboard

2. **Managing Applications:**
   - View all pending applications
   - Review supplier details and documents
   - Click "View PDF" to examine uploaded files
   - Accept or reject applications
   - System automatically updates status

3. **Managing Suppliers:**
   - View all registered suppliers
   - Update supplier information
   - Monitor supplier activity

4. **Managing Products:**
   - Add new product requirements
   - Update product specifications
   - View product application history

## 🔐 Security Considerations

### Current Implementation
- PHP session-based authentication
- Login/logout functionality for both admin and suppliers
- Separate access levels for different user types

### Recommended Enhancements
- Implement password hashing (bcrypt/password_hash)
- Add SQL injection prevention (prepared statements)
- Implement CSRF token protection
- Add input validation and sanitization
- Enable HTTPS for secure data transmission
- Implement rate limiting for login attempts
- Add password strength requirements
- Implement password reset functionality

## 🐛 Known Issues & Future Enhancements

### Potential Improvements
1. **User Authentication:**
   - Implement secure password hashing
   - Add "remember me" functionality
   - Email verification for new registrations

2. **Booking System:**
   - Complete booking.html functionality
   - Integrate payment gateway
   - Add booking confirmation emails

3. **Supplier Features:**
   - Direct messaging with admin
   - Notification system for status updates
   - Document versioning

4. **Admin Features:**
   - Analytics dashboard
   - Report generation (PDF/Excel)
   - Bulk operations for suppliers/products

5. **UI/UX:**
   - Responsive design improvements
   - Mobile-first approach
   - Accessibility enhancements

## 📞 Support & Contact

For issues, questions, or contributions related to this system:
- Email: fabricembarush5@gmail.com
- Review the code documentation in respective PHP files

## 📄 License

This project is developed for educational and hotel management purposes.

## 👨‍💻 Developer Notes

### Database Connection
The system uses MySQLi for database operations. Connection parameters are defined in `connection.php`:
- Host: localhost
- Username: root
- Password: (empty by default)
- Database: barbados

### File Upload Configuration
- Uploaded files are stored in `/pages/backend/Supply/files/`
- Supported format: PDF
- Ensure proper file permissions for upload directory

### Session Configuration
Sessions are used throughout the application. Ensure PHP session configuration allows:
- Session start on each authenticated page
- Proper session timeout settings
- Secure session cookie flags

---

**Last Updated:** March 2026  
**Version:** 1.0  
**System Name:** Barbados Hotel Management System
