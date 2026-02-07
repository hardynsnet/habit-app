# habit-app– Software Gratuito de Seguimiento de Hábitos

## 1. Introducción

Habit es un software web gratuito diseñado para el seguimiento y control de hábitos personales. El sistema permite a los usuarios definir hábitos con distintas frecuencias (diaria, semanal o mensual), registrar su cumplimiento y analizar su progreso a lo largo del tiempo mediante métricas y visualizaciones simples.

El proyecto se desarrolla con un enfoque funcional y estructurado, priorizando la claridad del código, la escalabilidad y la facilidad de mantenimiento.

## 2. Objetivo del Proyecto

El objetivo principal de Habit es proporcionar una herramienta sencilla pero sólida que permita:

* Registrar hábitos personales de forma estructurada.
* Llevar un control histórico del cumplimiento de cada hábito.
* Calcular métricas básicas de progreso y constancia.
* Presentar la información de manera clara para facilitar el análisis del comportamiento del usuario.

## 3. Alcance del Proyecto (MVP)

El alcance del MVP se limita a las funcionalidades esenciales necesarias para que el sistema sea usable y funcional, evitando características avanzadas que no aporten valor inmediato.

Incluye:

* Gestión de usuarios
* Gestión de hábitos
* Registro de cumplimiento
* Seguimiento y visualización básica
* Notificaciones
* Modo oscuro

No incluye:

* Integraciones externas
* Monetización
* Aplicación móvil nativa

## 4. Arquitectura General

El sistema sigue una arquitectura cliente-servidor:

* **Frontend:** Aplicación web desarrollada en React.
* **Backend:** API REST desarrollada en Node.js con Express.
* **Base de datos:** PostgreSQL, gestionada mediante Prisma ORM.

La comunicación entre frontend y backend se realiza mediante solicitudes HTTP utilizando JSON.

## 5. Módulos del Sistema

### 5.1 Módulo de Usuarios

Responsable de la gestión de autenticación y perfil de los usuarios.

**Funcionalidades:**

* Registro de usuarios
* Inicio de sesión
* Gestión de perfil básico

**Datos del usuario:**

* Nombre
* Correo electrónico
* Contraseña (almacenada de forma cifrada)
* Foto de perfil
* Fecha de creación

La autenticación se realiza mediante JWT.

### 5.2 Módulo de Hábitos

Permite la creación y administración de hábitos asociados a un usuario.

**Funcionalidades:**

* Crear hábitos
* Editar hábitos existentes
* Eliminar hábitos
* Activar o desactivar hábitos

**Propiedades del hábito:**

* Nombre
* Frecuencia (diaria, semanal o mensual)
* Fecha de creación
* Estado (activo / inactivo)

Cada hábito pertenece a un único usuario.

### 5.3 Módulo de Registro de Cumplimiento

Encargado de almacenar el cumplimiento de cada hábito en una fecha específica.

**Características:**

* Un registro por hábito y fecha
* Permite marcar si el hábito fue cumplido o no

Estos registros son la base para el cálculo de métricas.

### 5.4 Módulo de Seguimiento y Métricas

Calcula información estadística a partir de los registros históricos.

**Métricas calculadas:**

* **Racha (streak):** número de periodos consecutivos cumplidos según la frecuencia del hábito.
* **Porcentaje de cumplimiento:** relación entre registros cumplidos y registros totales.
* **Historial:** visualización de registros por día, semana o mes.

Las métricas se calculan dinámicamente y no se almacenan en base de datos.

### 5.5 Módulo de Visualización

Responsable de presentar la información al usuario de forma gráfica.

**Componentes:**

* Calendario de cumplimiento (cumplido / no cumplido)
* Gráficas básicas de progreso (barras o circulares)

## 6. Modelo de Datos

### 6.1 Tabla users

* id (PK)
* name
* email
* password_hash
* created_at

### 6.2 Tabla chequeo_habitos

* id (PK)
* user_id
* initial_date
* final_date
* is_cheked
* habit_id

### 6.3 Tabla habits

* id (PK)
* user_id (FK)
* name
* frequency
* created_at
* is_active

### 6.4 Tabla habit_logs

* id (PK)
* habit_id (FK)
* date
* completed

**Restricción:** un hábito solo puede tener un registro por fecha.

## 7. Lógica de Negocio

### 7.1 Cálculo de Rachas

* **Diario:** se cuentan días consecutivos cumplidos hacia atrás desde la fecha actual.
* **Semanal:** se evalúa el cumplimiento por semanas completas.
* **Mensual:** se evalúa el cumplimiento por meses completos.

### 7.2 Porcentaje de Cumplimiento

Se calcula dividiendo el número de registros cumplidos entre el total de registros disponibles.

## 8. Tecnologías Utilizadas

### Frontend

* React
* TailwindCSS

### Backend

* Node.js
* Express
* JWT

### Base de Datos

* PostgreSQL
* Prisma ORM

### Visualización

* Chart.js o Recharts

## 9. Estado del Proyecto

El proyecto se encuentra en fase de desarrollo del MVP. La estructura está pensada para permitir futuras ampliaciones sin afectar la base del sistema.

## 10. Posibles Extensiones Futuras

* Aplicación móvil
* Categorías de hábitos
* Exportación de datos

## 11. Autor

Proyecto desarrollado por **Harley Mosquera & Success Technology** como software gratuito y base tecnológica.



------------------------------------------------------------------------------------------------------------------------------------------------------------------------



###📜 Documentación Técnica de Backend - Habit (MVP)

Este documento detalla la arquitectura, lógica de negocio y especificaciones de la API desarrollada para la aplicación de seguimiento de hábitos.

1. Arquitectura del Sistema
El backend está construido sobre una arquitectura RESTful utilizando el stack PERN (PostgreSQL, Express, React, Node.js).

Entorno de ejecución: Node.js

Framework: Express.js

ORM: Prisma (Type-safe)

Base de Datos: PostgreSQL

Autenticación: JWT (JSON Web Tokens)

2. Modelo de Datos (Esquema Prisma)
La base de datos se ha normalizado para garantizar integridad y evitar duplicados. La "Fuente de Verdad" para cualquier métrica es la tabla HabitLog.

2.1 Tablas Principales
Tabla,Función,Constraints Clave
User,Almacena credenciales y perfil.,email (Unique)
Habit,Define los hábitos de cada usuario.,userId (FK)
HabitLog,Registro diario de cumplimiento.,"UNIQUE(habitId, date)"
ChequeoHabito,Registro de periodos cerrados (Semanales/Mensuales).,"initial_date, final_date"

Nota Senior: Se definió el campo date en HabitLog como tipo @db.Date para evitar conflictos de horas/minutos en el cálculo de rachas.

3. Lógica de Negocio: Algoritmo de Rachas (Streaks)
El sistema no guarda las rachas en la base de datos para evitar desincronización. Se calculan dinámicamente mediante SQL avanzado (CTEs).

3.1 Racha Diaria (Daily Streak)
Se utiliza una Función de Ventana (ROW_NUMBER) para detectar la continuidad.

Lógica: Se comparan los días registrados con una serie aritmética generada en tiempo real. Si el día N atrás no coincide con la fecha del registro, la racha se rompe.

Regla de Oro: El cálculo se detiene en el primer día donde completed = false o donde no exista registro.

3.2 Racha Semanal/Mensual
Se utiliza DATE_TRUNC para agrupar los logs. Una semana o mes se considera "cumplido" si existe al menos un registro con completed = true en dicho periodo.

4. Especificaciones de la API (Endpoints)
4.1 Módulo de Autenticación
POST /auth/register: Recibe name, email, password. Cifra la clave con BcryptJS (10 salt rounds).

POST /auth/login: Valida credenciales y retorna un JWT con una validez de 24h.

4.2 Módulo de Hábitos
GET /habits: Retorna la lista de hábitos del usuario autenticado. Incluye el cálculo de racha actual mediante $queryRaw.

POST /habits: Crea un nuevo hábito (daily, weekly, monthly).

PUT /habits/:id: Edición de nombre o frecuencia.

DELETE /habits/:id: Eliminación lógica (cambio de estado a is_active: false).

4.3 Módulo de Cumplimiento (Check-in)
POST /habits/:id/check:

Utiliza la operación upsert de Prisma (si existe el log, lo actualiza; si no, lo crea).

Payload: { "date": "YYYY-MM-DD", "completed": boolean }.

Response: Retorna el log actualizado y la racha recalculada.

4.4 Módulo de Visualización y Perfil
GET /habits/:id/calendar?month=YYYY-MM: Filtra los logs entre el primer y último día del mes solicitado. Formatea la salida a ISO Strings para consistencia en el Frontend.

GET /users/me: Retorna la información del token decodificado y datos de perfil.

PUT /users/me: Permite actualizar nombre, correo o contraseña de forma segura.

5. Seguridad e Integridad
Protección de Rutas: Middleware authenticateToken que verifica el Bearer Token en cada petición privada.

Aislamiento de Datos: Todas las consultas SQL y de Prisma incluyen obligatoriamente el filtro where: { userId: req.user.id } para evitar que un usuario acceda a datos de otro.

Prevención de Inyecciones: Uso de consultas parametrizadas en $queryRaw para evitar SQL Injection.

6. Próximos Pasos (Frontend)
El backend está configurado para manejar CORS. La estructura de respuesta JSON está optimizada para ser consumida por:

Axios como cliente HTTP.

Context API o Zustand para manejar el estado global del usuario y el token.

Chart.js / Recharts para consumir los datos del endpoint de calendario y métricas.

Comandos de Configuración Rápida
Bash
# Instalar dependencias
npm install

# Generar cliente de Prisma
npx prisma generate

# Aplicar migraciones a la base de datos
npx prisma migrate dev --name init

# Iniciar servidor
npm run dev
