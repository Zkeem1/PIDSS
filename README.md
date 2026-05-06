<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

<p align="center">
<a href="https://github.com/laravel/framework/actions"><img src="https://github.com/laravel/framework/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

# piDSS — Photovoltaic Installation Decision Support System

> A full-stack Laravel web application that helps registered customers evaluate, configure, and select photovoltaic (PV) solar system components through an intelligent decision-support interface.

---

## Table of Contents
- [Project Overview](#project-overview)
- [Key Features](#key-features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Database Design](#database-design)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Testing](#testing)
- [Project Structure](#project-structure)
- [Screenshots](#screenshots)
- [Future Enhancements](#future-enhancements)
- [License](#license)

---

## Project Overview

**piDSS** (Photovoltaic Installation Decision Support System) is a web-based decision support platform built with the Laravel framework. The system enables registered customers to:

- Browse and compare PV system items (solar panels, inverters, batteries, etc.)
- View detailed product specifications, descriptions, and pricing
- Configure complete PV system installations tailored to their property
- Schedule consultations and receive automated quotes
- Manage house information, appliance loads, and energy requirements

The application follows the **MVC (Model-View-Controller)** architectural pattern and implements **RESTful API design principles** with service-oriented architecture (SOA).

---

## Key Features

### Core Functionality
| Feature | Description |
|---------|-------------|
| **Item Catalog** | List and browse PV system components with name, description, and pricing |
| **User Authentication** | Secure registration, login, email verification, and password reset via Laravel Breeze |
| **Product Management** | CRUD operations for products and product attributes (admin/manager roles) |
| **PV System Configuration** | Build custom solar system configurations with building, room, and appliance data |
| **Automated Quoting** | Generate investment quotes based on system configuration |
| **Scheduling** | Book consultation appointments with automated time-slot management |
| **Search & Filter** | Advanced search across products, configurations, and user data |
| **Role-Based Access** | Multi-level permissions (Admin, Manager, Customer) |

### Additional Features
- **Responsive UI** built with Tailwind CSS and Blade templating engine
- **Dark mode support** via Laravel Breeze
- **Form validation** with custom request classes and reusable validation rules
- **Database seeding** for rapid development and testing environments
- **Unit & Feature Testing** with PHPUnit for authentication, product management, and search

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| **Backend Framework** | Laravel 10.x (PHP 8.1+) |
| **Frontend** | Blade Templating, Tailwind CSS, Alpine.js |
| **Authentication** | Laravel Breeze (Laravel Sanctum) |
| **Database** | MySQL / MariaDB |
| **ORM** | Eloquent |
| **Migration & Seeding** | Laravel Migrations, Factories, Seeders |
| **Testing** | PHPUnit |
| **Build Tool** | Vite |
| **Package Manager** | Composer, npm |
| **Version Control** | Git |

---

## Architecture

### MVC Pattern Implementation
```
Request → Route → Controller → Model → Database
                ↓
              View (Blade)
```

### System Components
- **Routes** (`routes/web.php`) — Define URI-to-controller mappings
- **Controllers** (`app/Http/Controllers/`) — Handle HTTP requests and business logic
- **Models** (`app/Models/`) — Eloquent ORM for database interactions
- **Views** (`resources/views/`) — Blade templates for UI rendering
- **Migrations** (`database/migrations/`) — Version-controlled database schema
- **Seeders** (`database/seeders/`) — Populate database with test/production data

### RESTful Resource Routes
| HTTP Verb | URI | Controller Method | Action |
|-----------|-----|-------------------|--------|
| GET | `/item` | `index()` | List all PV items |
| GET | `/product` | `index()` | List products |
| GET | `/product/{id}` | `show()` | View product details |
| GET/POST | `/schedule` | `index/store()` | Manage appointments |
| GET/POST | `/quote` | `index/store()` | Generate quotes |

---

## Database Design

### Key Entities
| Table | Purpose |
|-------|---------|
| `users` | Registered customers and administrators |
| `items` | PV system components (panels, inverters, batteries) |
| `products` | Product catalog with attributes |
| `product_attributes` | Specifications linked to products |
| `configurations` | User-defined PV system setups |
| `buildings` | Property information per user |
| `rooms` | Rooms within buildings |
| `appliances` | Electrical loads per room |
| `pv_systems` | Complete solar system configurations |
| `schedules` | Consultation appointments |
| `faqs` | Frequently asked questions |

### Eloquent Relationships
- `User` → has many `Building`, `Schedule`, `Configuration`
- `Building` → has many `Room`
- `Room` → has many `Appliance`
- `Product` → has many `ProductAttribute`
- `PVSystem` → belongs to `Configuration`, has many `PVSystemProduct`

---

## Installation & Setup

### Prerequisites
- PHP >= 8.1
- Composer
- Node.js & npm
- MySQL/MariaDB
- Git

### Step-by-Step Setup

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/SEOLS-Iteration-4.git
cd SEOLS-Iteration-4

# 2. Install PHP dependencies
composer install

# 3. Install frontend dependencies
npm install
npm run build

# 4. Environment configuration
cp .env.example .env
php artisan key:generate

# 5. Database setup
# Update .env with your database credentials:
# DB_DATABASE=pidss
# DB_USERNAME=root
# DB_PASSWORD=your_password

# 6. Run migrations and seeders
php artisan migrate:fresh --seed

# 7. Start the development server
php artisan serve
```

Access the application at: `http://localhost:8000`

---

## Usage

### Default Login Credentials (Seeded)
After running `php artisan db:seed`, use these accounts:

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@example.com | password |
| Customer | customer@example.com | password |

### Key URLs
| URL | Description |
|-----|-------------|
| `/` | Landing/Welcome page |
| `/register` | User registration |
| `/login` | User login |
| `/dashboard` | Customer home page |
| `/item` | List PV items |
| `/product` | Product catalog |
| `/schedule` | Book consultation |
| `/quote` | Generate quote |

---

## Testing

### Test Coverage
- **Authentication Tests** — Registration, login, password reset, email verification
- **Product Admin Tests** — CRUD operations, authorization
- **Product Customer Tests** — Catalog browsing, search
- **Profile Tests** — Update personal information, password changes
- **Search Tests** — Query functionality, result accuracy

### Run Tests
```bash
php artisan test
```

### Manual Test Scenarios
1. Register a new user and verify email
2. Login with correct/incorrect credentials
3. Access `/item` while authenticated vs. unauthenticated
4. Create, read, update, and delete products (admin only)
5. Generate a PV system configuration and quote
6. Schedule a consultation appointment

---

## Project Structure

```
SEOLS-Iteration-4/
├── app/
│   ├── Http/
│   │   ├── Controllers/      # Business logic controllers
│   │   ├── Middleware/         # Authentication, authorization
│   │   ├── Requests/           # Form request validation
│   │   └── Kernel.php
│   ├── Models/                 # Eloquent models
│   ├── Policies/               # Authorization policies
│   ├── Providers/              # Service providers
│   ├── Rules/                  # Custom validation rules
│   └── Traits/                 # Reusable code traits
├── config/                     # Laravel configuration
├── database/
│   ├── factories/              # Model factories
│   ├── migrations/             # Schema migrations
│   └── seeders/                # Database seeders
├── resources/
│   ├── views/                  # Blade templates
│   ├── css/                    # Stylesheets
│   └── js/                     # JavaScript
├── routes/
│   ├── web.php                 # Web routes
│   └── auth.php                # Authentication routes
├── tests/
│   ├── Feature/                # Feature tests
│   └── Unit/                   # Unit tests
├── .env.example
├── composer.json
├── package.json
└── README.md
```

---

## Screenshots

> *[Add screenshots of key pages: Landing page, Dashboard, Product Catalog, Schedule Form, Quote Generator]*

---

## Future Enhancements

- [ ] API endpoints for mobile app integration
- [ ] Real-time solar production estimation using weather APIs
- [ ] PDF quote generation and email delivery
- [ ] Payment gateway integration for deposits
- [ ] Admin analytics dashboard with charts and reports
- [ ] Multi-language support (i18n)
- [ ] Docker containerization for deployment

---

## License

This project was developed for academic purposes at Fanshawe College as part of the SEOLS (Software Engineering for Open-Source and Linux Systems) course.

---

## Contact

**Developer:** Temar Lewis  
**GitHub:** [@Zkeem1](https://github.com/Zkeem1)  
**Email:** temarlewis@yahoo.com

---

> *"Making your photovoltaic systems decisions easier."* — piDSS

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
