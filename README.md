# FarmaYa 🏥

Proyecto Final para el curso de Integrador I - Sistema de E-commerce para Farmacia

## 📋 Descripción

FarmaYa es una plataforma de comercio electrónico completa diseñada para farmacias, que permite a los usuarios navegar por un catálogo de productos farmacéuticos, gestionar carritos de compra, realizar pedidos y realizar pagos de manera segura. El sistema incluye interfaces separadas para clientes, administradores y personal de entrega.

## ⚡ Inicio Rápido (Docker)

```bash
# 1. Clonar el repositorio
git clone https://github.com/LIUNKS/FarmaYa.git
cd FarmaYa

# 2. Crear variables de entorno locales
cp .env.example .env
# Edita .env y define contraseñas seguras

# 3. Levantar todo el sistema (DB + Backend)
docker compose up --build

# 4. Abrir en el navegador
# Backend API: http://localhost:8080
# Swagger UI: http://localhost:8080/swagger-ui.html
# Frontend: Abre frontend/farmacia-merysalud/index.html
```

¡Listo! 🎉 El backend estará corriendo en el puerto 8080 con la base de datos configurada y el script SQL ejecutado automáticamente.

## ✨ Características

### Para Clientes

- **Registro y Autenticación**: Sistema seguro de login/registro con JWT
- **Catálogo de Productos**: Navegación por categorías, búsqueda y filtros
- **Carrito de Compras**: Agregar, modificar y eliminar productos
- **Gestión de Pedidos**: Historial de pedidos y seguimiento de estado
- **Direcciones de Entrega**: Múltiples direcciones por usuario

### Para Administradores

- **Dashboard Administrativo**: Panel de control completo
- **Gestión de Productos**: CRUD completo de productos con imágenes
- **Gestión de Usuarios**: Administración de clientes y roles
- **Gestión de Pedidos**: Actualización de estados y procesamiento
- **Reportes de Ventas**: Análisis y reportes de ventas

### Para Personal de Entrega

- **Interfaz de Entrega**: Gestión de pedidos asignados
- **Actualización de Estados**: Cambio de estado de pedidos

## 🛠️ Tecnologías Utilizadas

### Backend

- **Java 21** - Lenguaje de programación
- **Spring Boot 3.5** - Framework principal
- **Spring Security** - Autenticación y autorización
- **Spring Data JPA** - Persistencia de datos
- **JWT (JSON Web Tokens)** - Autenticación stateless
- **MySQL** - Base de datos relacional
- **Swagger/OpenAPI** - Documentación de API
- **Maven** - Gestión de dependencias

### Frontend

- **HTML5** - Estructura de páginas
- **CSS3** - Estilos y diseño
- **JavaScript (ES6)** - Lógica del cliente
- **Bootstrap 5** - Framework CSS responsivo

### Base de Datos

- **MySQL 8.0+** - Sistema de gestión de base de datos
- **Hibernate** - ORM para Java

## 📋 Prerrequisitos

### Opción A: Ejecución con Docker (Recomendado)

- **Docker** y **Docker Compose**
- **Git** (para clonar el repositorio)
- Navegador web moderno (Chrome, Firefox, Edge)

### Opción B: Ejecución Local

- **Java 21** o superior
- **Maven 3.6+** (o usar el Maven Wrapper incluido: `./mvnw`)
- **MySQL 8.0+**
- **Git** (para clonar el repositorio)
- Navegador web moderno (Chrome, Firefox, Edge)

## 🚀 Instalación y Configuración

### 1. Clonar el Repositorio

```bash
git clone https://github.com/LIUNKS/FarmaYa.git
cd FarmaYa
```

---

## 🐳 Opción A: Ejecución con Docker Compose (Recomendado)

Esta es la forma más rápida de levantar todo el proyecto (Base de datos + Backend) en contenedores.

### 2A. Levantar todo el sistema

```bash
# Construir y arrancar todos los servicios (DB + Backend)
docker compose up --build
```

Esto hará automáticamente:

- ✅ Crear el contenedor MySQL 8.0 con la base de datos `farmaya`
- ✅ Compilar tu aplicación Spring Boot con Maven Wrapper
- ✅ Ejecutar el script SQL (`farmaYa.sql`) automáticamente
- ✅ Iniciar el backend en `http://localhost:8080`

### Comandos útiles de Docker Compose

```bash
# Ver logs en tiempo real
docker compose logs -f

# Ver solo logs del backend
docker compose logs -f farmaya-app

# Ver solo logs de la base de datos
docker compose logs -f farmaya-db

# Detener todos los servicios
docker compose down

# Reiniciar solo el backend (si cambias código)
docker compose restart farmaya-app

# Reconstruir el backend (después de cambios importantes)
docker compose up --build farmaya-app

# Detener y eliminar todo (incluida la base de datos)
docker compose down -v
```

### Configuración de variables de entorno

Antes de ejecutar Docker Compose, define tus variables locales:

```bash
# Crear archivo .env en la raíz del proyecto
cp .env.example .env

# Editar el archivo .env con tus credenciales
nano .env
```

Ejemplo de `.env`:

```env
MYSQL_ROOT_PASSWORD=TuContraseñaRootSegura
MYSQL_PASSWORD=TuContraseñaAppSegura
MYSQL_DATABASE=farmaya
MYSQL_USER=farmaya_app
SPRING_SQL_INIT_MODE=always
```

**Nota de seguridad**: El archivo `.env` está ignorado en `.gitignore` para evitar subir credenciales al repositorio.

**Importante**:
- En desarrollo (Docker Compose local): `SPRING_SQL_INIT_MODE=always` para cargar el script SQL.
- En producción: usar `SPRING_SQL_INIT_MODE=never`.
- MySQL no se publica al host por defecto. Si necesitas exponer el puerto para herramientas locales, hazlo con un `docker-compose.override.yml` local (no versionado).

---

## 💻 Opción B: Ejecución Local (Sin Docker)

Si prefieres ejecutar el proyecto de forma tradicional sin Docker:

### 2B. Configurar la Base de Datos MySQL

#### Opción 1: Instalación local de MySQL

```bash
# Crear la base de datos e importar el script SQL
mysql -u root -p < backend/src/main/resources/farmaYa.sql
```

#### Opción 2: MySQL en contenedor (solo la DB)

```bash
docker run --name farmaya-db \
  -e MYSQL_ROOT_PASSWORD=TuContraseñaRootSegura \
  -e MYSQL_USER=farmaya_app \
  -e MYSQL_PASSWORD=TuContraseñaAppSegura \
  -e MYSQL_DATABASE=farmaya \
  -p 3306:3306 \
  -v farmaya_data:/var/lib/mysql \
  --restart unless-stopped \
  -d mysql:8.0
```

### 3B. Configurar el Backend

```bash
cd backend

# Editar application.properties si es necesario
nano src/main/resources/application.properties
```

**Variables recomendadas para entorno local**:

- `SPRING_DATASOURCE_USERNAME=farmaya_app`
- `SPRING_DATASOURCE_PASSWORD=<tu_password_app>`
- `MYSQL_DATABASE=farmaya`
- `SPRING_SQL_INIT_MODE=always` (solo desarrollo)

### 4B. Ejecutar el Backend

```bash
# Opción 1: Con Maven Wrapper (recomendado)
./mvnw spring-boot:run

# Opción 2: Con Maven instalado globalmente
mvn spring-boot:run
```

El backend estará disponible en: `http://localhost:8080`

---

### 5. Ejecutar el Frontend

```bash
cd frontend/farmacia-merysalud

# Opción 1: Abrir directamente en el navegador
# Abre el archivo index.html con tu navegador

# Opción 2: Usar un servidor local (recomendado)
python -m http.server 3000
# o con Python 2
python -m SimpleHTTPServer 3000
# o con Node.js
npx serve -p 3000
```

El frontend estará disponible en: `http://localhost:3000` (o directamente abriendo los archivos HTML)

## 📖 Uso

### Acceso al Sistema

1. **Cliente**: Registrarse o iniciar sesión en `/login.html`
2. **Administrador**: Usar credenciales de admin para acceder al dashboard
3. **Repartidor**: Usar credenciales de delivery para gestión de pedidos

### API Documentation

La documentación completa de la API está disponible en Swagger UI:

```
http://localhost:8080/swagger-ui.html
```

### Endpoints Principales

- `POST /api/auth/login` - Autenticación
- `GET /api/products` - Listar productos
- `POST /api/cart/add` - Agregar al carrito
- `POST /api/orders` - Crear pedido
- `GET /api/orders` - Listar pedidos del usuario

## 🏗️ Arquitectura del Proyecto

```
FarmaYa/
├── docker-compose.yml       # Orquestación de servicios (DB + Backend)
├── .env.example            # Ejemplo de variables de entorno
├── .gitignore              # Archivos ignorados por Git
├── backend/                # API REST con Spring Boot
│   ├── Dockerfile          # Imagen Docker del backend
│   ├── mvnw / mvnw.cmd     # Maven Wrapper
│   ├── pom.xml             # Dependencias Maven
│   ├── src/main/java/com/farma_ya/
│   │   ├── controller/     # Controladores REST
│   │   ├── model/          # Entidades JPA
│   │   ├── repository/     # Repositorios de datos
│   │   ├── service/        # Lógica de negocio
│   │   ├── security/       # Configuración de seguridad
│   │   └── config/         # Configuraciones generales
│   └── src/main/resources/
│       ├── application.properties
│       └── farmaYa.sql     # Script de inicialización
├── frontend/               # Interfaz de usuario
│   └── farmacia-merysalud/
│       ├── assets/         # CSS, JS, imágenes
│       ├── components/     # Componentes HTML reutilizables
│       ├── admin/          # Páginas de administración
│       └── delivery/       # Páginas de entrega
└── DOCKER_SETUP.md         # Documentación detallada de Docker
```

## 📚 Documentación Técnica

### Documentación de Infraestructura

- **[Configuración de Docker](DOCKER_SETUP.md)**: Guía completa de Docker, troubleshooting y mejores prácticas

### Documentación del Informe Académico

- **[Principios de Diseño](principios-diseno.md)**: Aplicación de MVC, TDD, DAO y SOLID
- **[Librerías y Dependencias](librerias-dependencias.md)**: Recursos Java utilizados
- **[Porcentaje del Proyecto](porcentaje-proyecto.md)**: Distribución del código fuente
- **[Control de Versiones](control-versiones.md)**: Análisis del repositorio Git

## 🔧 Solución de Problemas

### El backend no conecta a la base de datos

```bash
# Verificar que MySQL esté listo
docker compose logs farmaya-db | grep "ready for connections"

# Verificar conectividad entre contenedores
docker compose exec farmaya-app ping farmaya-db
```

### El script SQL no se ejecuta

```bash
# Verificar que el archivo existe en el contenedor
docker compose exec farmaya-app ls -la /app/src/main/resources/

# Ver logs de Spring Boot
docker compose logs farmaya-app | grep "farmaYa.sql"
```

### Cambié el código pero no se refleja

```bash
# Reconstruir la imagen sin caché
docker compose build --no-cache farmaya-app
docker compose up farmaya-app
```

### Puerto 3306 o 8080 ya está en uso

```bash
# Ver qué proceso está usando el puerto
lsof -i :3306
lsof -i :8080

# O detener contenedores previos
docker stop $(docker ps -aq)
```

## 📝 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para más detalles.
