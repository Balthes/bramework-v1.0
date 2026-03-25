Here is the **exact same text**, translated to **English**, keeping the **structure, formatting, and meaning 100% identical**.

---

# 🇬🇧 Bramework v1.0 – Developer Documentation

## 1. System Philosophy and Architecture

**Bramework** is a lightweight, custom‑built PHP framework based on the **Front Controller** design pattern.  
Every client‑side request (HTTP request) passes through a single central entry point: the `public/index.php` file.

This approach enables:

- the use of **Clean URLs**  
- robust **routing**  
- centralized handling of global configurations and security checks (e.g., session initialization)  

---

## 2. Folder Structure and Organization

The framework strictly separates:

- business logic  
- presentation  
- publicly accessible browser‑side files  

```
/bramework
├── /docs
│   ├── documentation_HU.md     # Detailed Hungarian documentation
│   ├── documentation_EN.md     # Detailed English documentation
│   └── database_setup.sql      # Database table schema (SQL export)
├── /config                     # Global settings (DB, URL, Debug)
│   └── config.php              # The only file a developer needs to modify for deployment
├── /app                        # The application's presentation layer
│   ├── /components             # Built‑in, fundamental building blocks
│   │   ├── header.php          # HTML head, meta tags, CSS linking
│   │   ├── footer.php          # Footer, JS script loading
│   │   ├── nav.php             # Responsive navigation bar
│   │   ├── start.php           # Default landing page
│   │   ├── login.php           # Built‑in login form
│   │   └── register.php        # Built‑in registration form
│   └── /views                  # Project‑specific subpages (e.g., about.php)
├── /core                       # The framework engine (Core API)
│   ├── Auth.php                # User session and permission handling
│   ├── Database.php            # Singleton PDO database connection
│   ├── Router.php              # URL processing and execution engine
│   └── View.php                # Template rendering class
├── /public                     # The Document Root
│   ├── /css                    # Stylesheets
│   ├── /js                     # Client‑side scripts
│   ├── .htaccess               # Apache rewrite rules for the Front Controller
│   └── index.php               # Central entry point and route registration
└── README.md                   # Quick start guide
```

**Important design decision:**  
The `start.php`, `login.php`, and `register.php` files are located in the `/components` folder because Bramework is designed with built‑in user management and instant startup in mind. These files are part of the “core,” not project‑specific views.

---

## 3. Operation of the Core Classes

### 3.1. Routing (Router.php and index.php)

The heart of the system is the router.  
The `.htaccess` file redirects all requests to the following format:

```
index.php?url=...
```

Using the `Router::add()` method, you can assign an anonymous function (closure) to a specific URL.  
The `dispatch()` method executes the appropriate code block or triggers a 404 error.

**Example of loading the default landing page:**

```php
$router->add('/', function() {
    View::render('header', ['title' => 'Welcome to Bramework!'], true);
    View::render('nav', [], true);
    View::render('start', [], true); // Loaded from the components folder
    View::render('footer', [], true);
});
```

---

### 3.2. View Handling (View.php)

The `View::render()` static method is responsible for loading HTML files.

Parameters:

- **$name (string):** the name of the file to load (e.g., `start`)  
- **$data (array):** associative array whose keys become local variables via `extract()`  
- **$isComponent (bool):**  
  - `false` → loads from `/views`  
  - `true` → loads from `/components`  

---

### 3.3. Configuration and Database (Portability)

System portability is managed by the `/config/config.php` file. When migrating to any other server, you **only** need to modify the data within **this specific file**.

**Installation Steps:**
1. Copy the files to your server.
2. Import the `/docs/database_setup.sql` file into your MySQL database.
3. Open `/config/config.php` and provide the following:
   - Your database name, username, and password.
   - The `BASE_URL` value (e.g., `http://yourdomain.com/public`).

---


### 3.4. Authentication and Security (Auth.php)

The built‑in authentication module manages user sessions.

Available functions:

- `Auth::init()` – start session  
- `Auth::login()` – log in  
- `Auth::logout()` – log out  
- `Auth::check()` – check login status  

**Password handling:**

- passwords are **never stored in plain text**  
- during registration: `password_hash()`  
- during login: `password_verify()`  

---

If you'd like, I can also create a **fully bilingual HU–EN version**, or format it as a **GitHub‑ready README**.
