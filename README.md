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

No incluye:

* Notificaciones
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

### 6.2 Tabla habits

* id (PK)
* user_id (FK)
* name
* frequency
* created_at
* is_active

### 6.3 Tabla habit_logs

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

* Notificaciones
* Modo oscuro
* Aplicación móvil
* Categorías de hábitos
* Exportación de datos

## 11. Autor

Proyecto desarrollado por **Harley Mosquera & Success Technology** como software gratuito y base tecnológica.
