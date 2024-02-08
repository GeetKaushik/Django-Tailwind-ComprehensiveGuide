# Setting up Tailwind in Django with Hot Reloads

This guide provides steps to seamlessly integrate Tailwind CSS with hot reloads in a Django project.

## I. Enabling Hot Reloads

### Step 1: Install django-browser-reload

```bash
python -m pip install django-browser-reload
```

### Step 2: Add django-browser-reload to your INSTALLED_APPS in settings.py

```bash
INSTALLED_APPS = [
   ...,
   "django_browser_reload", # for hot-reload
   ...,
]

```

### Step 3: Include the app in your urls.py

```bash
# django_project/urls.py

urlpatterns = [
   ...,
   path("__reload__/", include("django_browser_reload.urls")), # This hot-reloads the page
]

```

### Step 4: Edit middleware

```bash
MIDDLEWARE = [
   "django.middleware.security.SecurityMiddleware",
   "django.contrib.sessions.middleware.SessionMiddleware",
   "django.middleware.common.CommonMiddleware",
   "django.middleware.csrf.CsrfViewMiddleware",
   "django.contrib.auth.middleware.AuthenticationMiddleware",
   "django.contrib.messages.middleware.MessageMiddleware",
   "django.middleware.clickjacking.XFrameOptionsMiddleware",
   "django_browser_reload.middleware.BrowserReloadMiddleware",  # hot-reload middleware
]
```

## II. Setting up Tailwind with Django

### Step 1: Install and initialize TailwindCSS

```bash
npm install -D tailwindcss
npx tailwindcss init
```

### Step 2: Add HTML source files to tailwind.config.js

```bash
// tailwind.config.js

    module.exports = {
        content: [
            './templates/**/*.html' // path where all your source files are located
        ],
    theme: {
        extend: {},
        },
    plugins: [],
    }
```

### Step 3: Create a static folder in the project root and add src/input.css

```bash
    |-- your django-project name
        |-- settings.py
        |-- ...
    |-- your django-app name
    |-- templates
    |-- manage.py
    |-- static
    └── src
        └── input.css
```

Edit input.css:

```bash
    /* static/src/input.css */

    @tailwind base;
    @tailwind components;
    @tailwind utilities;
```

### Step 4: Edit package.json

```bash
    // package.json
    {
        "scripts": {
        "dev": "npx tailwindcss -i ./static/src/input.css -o ./static/src/styles.css --watch"
        //here ./static/src/input.css is the path input.css you just created, ./static/src/styles.css is the path for optimized output styles provided by tailwind (don't worry the file will be created on its own by tailwind)
        },
        "devDependencies": {
        "tailwindcss": "^3.4.1" // don’t worry if version differs a bit
        }
    }
```

### Step 5: Open the terminal and run the following command to check TailwindCSS

```bash
    npm run dev
```

### Step 6: Create a base.html file to load TailwindCSS styles

```bash

    <!-- templates/base.html -->
    {% load static %} <!-- to load static files in django-html -->

    <!DOCTYPE html>
    <html lang="en">

    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Django + Tailwind CSS</title>
    <link rel="stylesheet" href="{% static 'src/styles.css' %}"> <-! To load Tailwind CSS styles in django-html -->

    </head>

    <body class="bg-green-50">
    <div class="container mx-auto mt-4">
        {% block content %}
        {% endblock content %}
    </div>
    </body>

    </html>

```

### Step 7: Run the server

```bash

python manage.py runserver

# Open in browser 127.0.0.0:8000
# ProTip: you can change the port number by specifying it at the end
# (e.g., python manage.py runserver 5000, open in browser 127.0.0.0:5000)

```

## III. Bonus Steps: Integrate TailwindCSS watcher and Django server in one terminal

### Step 1: Install concurrently
```bash
npm i -D concurrently

```
### Step 2: Setup Concurrently for TailwindCSS watcher and Django server by updating package.json
```bash
    {
    "scripts": {
        "tailwind": "npx tailwindcss -i ./static/src/input.css -o ./static/src/styles.css --watch",
        "django": "python manage.py runserver",
        "start": "concurrently --names \"tailwind,django\" -c \"bgBlue.bold,bgGreen.bold\"  \"npm run tailwind\" \"npm run django\""
    },
    "devDependencies": {
    "Concurrently": "^8.2.2" // don’t worry if version differs a bit
    "tailwindcss": "^3.4.1" // don’t worry if version differs a bit
    }
    }

```
### Step 3: Run the following command in the terminal
```bash
npm start
```
# Now you can enjoy a seamless developer experience with Django and TailwindCSS! 😄 
##Please provide your feedback. Your input is valuable. Additionally, I would appreciate it if you could consider giving a star to the repo. Thank you! ⭐

