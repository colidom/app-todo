# 📋 IT Admin Dashboard - App TODO

Este proyecto es una aplicación web que forma parte del **IT Admin Dashboard**, desarrollada para ayudarte a gestionar tus tareas diarias de forma rápida y eficiente. La solución está dividida en dos partes:

-   **Backend**: Laravel 11
-   **Frontend**: React + Vite + TailwindCSS

---

## 🚀 Funcionalidades

-   ✅ Añadir, completar y eliminar tareas (TODOs)
-   📁 Organización clara en secciones mediante un sistema de acordeón
-   🔒 Almacenamiento de accesos (próximamente)
-   📦 Separación total de frontend y backend para facilitar la escalabilidad

---

## 📂 Estructura del proyecto

```bash
app-todo/
├── app-back/       # Laravel 11 API (Laravel Breeze + Sanctum)
└── app-front/      # React (Vite + TailwindCSS)
```

🛠️ Requisitos
PHP >= 8.2
Composer
Node.js >= 18
MySQL o PostgreSQL
Laravel Sail (opcional)

⚙️ Instalación
Backend (Laravel)

cd backend
cp .env.example .env
composer install
php artisan key:generate
php artisan migrate
php artisan serve

⚙️ Instalación
Backend (Laravel)

Por defecto, la API espera que esté corriendo en http://localhost:8000 y el frontend en http://localhost:5173

🔐 Autenticación
El backend utiliza Laravel Sanctum para autenticar las peticiones desde el frontend. Puedes registrar y loguearte desde el frontend, y se mantendrá la sesión mediante cookies.

✨ Tecnologías utilizadas
Laravel 11

React 18

Vite

Tailwind CSS

Laravel Breeze

Laravel Sanctum

📌 Próximamente
Sección de accesos (usuarios, contraseñas y URLs)

Compartición de tareas entre usuarios

Tags y filtros

Sistema de notificaciones

🤝 Autor
Carlos Oliva
GitHub: @colidom
