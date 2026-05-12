# Sistema de Suficiencia de Red IPS

## Descripción del Proyecto

Sistema web PHP/MySQL para gestionar la suficiencia de red de Instituciones Prestadoras de Servicios (IPS), con soporte para múltiples sedes (Sub-IPS), talento humano por sede, capacidad instalada y generación de informes consolidados y detallados.

## Características Principales

El sistema permite administrar la relación jerárquica entre IPS principales y sus sedes (Sub-IPS), configurando para cada sede el talento humano disponible, la capacidad instalada de servicios y la producción mensual. Los informes pueden generarse de forma consolidada por IPS o detallada por cada Sub-IPS.

### Gestión de IPS

- Crear, editar y eliminar instituciones prestadoras de servicios
- Registrar información completa incluyendo NIT, nivel de atención, ubicación geográfica
- Visualizar todas las IPS activas con sus correspondientes sub-IPS asociadas
- Ver estadísticas generales de cada institución

### Gestión de Sub-IPS (Sedes)

- Administrar las sedes o subsedes de cada IPS (CALIPSO, POBLADO II, COMUNEROS II, NUEVO MILENIO, JAMUNDÍ)
- Cada sede se vincula a su IPS padre mediante la relación uno a muchos
- Registrar información de ubicación incluyendo barrio, tipo de sede y dirección
- Listar sub-IPS por IPS padre con filtros y búsqueda

### Gestión de Talento Humano

- Registrar el personal de salud asociado a cada Sub-IPS específica
- Asignar cargos desde el catálogo maestro (médicos, enfermeras, odontólogos, etc.)
- Configurar vinculación laboral (planta, OPS, contratista)
- Especificar horas diarias de trabajo y servicio asignado
- Visualizar distribución de personal por sede y área

### Gestión de Capacidad Instalada

- Configurar servicios disponibles en cada Sub-IPS
- Registrar número de consultorios, unidades y tiempo por consulta
- Definir horarios y días de atención (lunes a viernes, sábados)
- Calcular capacidad resolutiva (slots disponibles por semana)

### Informes y Reportes

- **Informe Consolidado por IPS**: Muestra datos agregados de todas las sub-IPS de una institución
- **Informe Detallado por Sub-IPS**: Presenta información específica de cada sede
- Estadísticas de talento humano, capacidad instalada y producción mensual
- Soporte para impresión directa del navegador

## Requisitos del Sistema

### Requisitos del Servidor

- PHP 8.0 o superior
- MySQL 8.0 o superior
- Servidor web Apache o Nginx

### Requisitos del Navegador

- Chrome/Edge 80 o superior
- Firefox 75 o superior
- Safari 13 o superior

## Instalación

### Paso 1: Importar Base de Datos

Accede a MySQL y ejecuta el siguiente comando para importar el esquema:

```bash
mysql -u root -p < database/estructura.sql
```

También puedes importar el archivo `database/estructura.sql` desde phpMyAdmin o MySQL Workbench.

### Paso 2: Configurar Conexión

Edita el archivo `config/database.php` con tus credenciales de MySQL:

```php
define('DB_HOST', 'localhost');
define('DB_NAME', 'suficiencia-ips');
define('DB_USER', 'tu_usuario');
define('DB_PASS', 'tu_contraseña');
```

### Paso 3: Iniciar el Sistema

Copia todos los archivos al directorio de tu servidor web (por ejemplo, `htdocs/suficiencia-ips`).

Accede al sistema desde tu navegador: `http://localhost/suficiencia-ips/`

## Estructura de Archivos

```
suficiencia-ips/
├── config/
│   └── database.php          # Configuración de conexión a MySQL
├── models/
│   ├── Ips_model.php          # Modelo de datos para IPS
│   ├── Subips_model.php       # Modelo de datos para Sub-IPS
│   ├── Talento_model.php      # Modelo para talento humano
│   ├── Capacidad_model.php    # Modelo para capacidad instalada
│   └── Servicios_model.php    # Modelo para catálogos
├── views/
│   ├── templates/
│   │   ├── header.php         # Plantilla de encabezado
│   │   └── footer.php         # Plantilla de pie
│   ├── dashboard.php          # Panel principal
│   ├── ips/                   # Vistas de IPS
│   ├── subips/                # Vistas de Sub-IPS
│   ├── talento_humano/        # Vistas de talento humano
│   ├── capacidad/            # Vistas de capacidad
│   └── informes/             # Vistas de informes
├── database/
│   └── estructura.sql        # Esquema completo de base de datos
├── css/
│   └── styles.css            # Estilos CSS
├── js/
│   └── main.js               # JavaScript principal
└── index.php                 # Controlador principal (enrutamiento)
```

## Modelo de Datos

### Tabla: ips

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT (PK) | Identificador único |
| nit | VARCHAR(20) | Número de identificación tributaria |
| nombre | VARCHAR(255) | Nombre de la IPS |
| direccion | VARCHAR(255) | Dirección principal |
| telefono | VARCHAR(50) | Teléfono de contacto |
| email | VARCHAR(100) | Correo electrónico |
| nivel | VARCHAR(20) | Nivel (PRIMARIO, SECUNDARIO, TERCIARIO) |
| municipio | VARCHAR(100) | Municipio |
| departamento | VARCHAR(100) | Departamento |
| activo | TINYINT(1) | Estado (1=activo, 0=inactivo) |
| fecha_registro | TIMESTAMP | Fecha de registro |

### Tabla: subips

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT (PK) | Identificador único |
| ips_id | INT (FK) | Referencia a IPS padre |
| codigo | VARCHAR(20) | Código interno de la sede |
| nombre | VARCHAR(255) | Nombre de la Sub-IPS |
| direccion | VARCHAR(255) | Dirección de la sede |
| telefono | VARCHAR(50) | Teléfono de contacto |
| email | VARCHAR(100) | Correo electrónico |
| tipo_sede | VARCHAR(50) | Tipo (CALIPSO, POBLADO, COMUNEROS, etc.) |
| barrio | VARCHAR(100) | Barrio o sector |
| municipio | VARCHAR(100) | Municipio |
| departamento | VARCHAR(100) | Departamento |
| activo | TINYINT(1) | Estado (1=activo, 0=inactivo) |
| fecha_registro | TIMESTAMP | Fecha de registro |

### Tabla: talento_humano

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT (PK) | Identificador único |
| subips_id | INT (FK) | Referencia a Sub-IPS |
| cedula | VARCHAR(20) | Número de cédula |
| nombre_completo | VARCHAR(255) | Nombre completo |
| cargo_id | INT (FK) | Referencia a cargo |
| horas_diarias | INT | Horas diarias de trabajo |
| tipo_vinculacion | VARCHAR(50) | Tipo de vinculación |
| servicio_id | INT (FK) | Servicio al que pertenece |
| fecha_registro | TIMESTAMP | Fecha de registro |

### Tabla: capacidad_instalada

| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT (PK) | Identificador único |
| subips_id | INT (FK) | Referencia a Sub-IPS |
| servicio_id | INT (FK) | Referencia a servicio |
| consultorios | INT | Número de consultorios |
| unidades | INT | Número de unidades |
| tiempo_consulta | INT | Tiempo por consulta (minutos) |
| dias_semana | VARCHAR(50) | Días de atención |
| horas_dia_lunes_viernes | INT | Horas lunes a viernes |
| horas_dia_sabado | INT | Horas sábado |
| fecha_registro | TIMESTAMP | Fecha de registro |

## Uso del Sistema

### Crear una IPS

1. Navega a la sección "IPS" en el menú lateral
2. Haz clic en "Nueva IPS"
3. Completa el formulario con la información de la institución
4. Haz clic en "Guardar"

### Agregar Sub-IPS

1. Desde la lista de IPS, haz clic en el botón "+ Sub-IPS" de la IPS correspondiente
2. Selecciona el tipo de sede (CALIPSO, POBLADO II, COMUNEROS II, NUEVO MILENIO, JAMUNDÍ)
3. Completa la información de la sede
4. Haz clic en "Guardar"

### Registrar Personal

1. Navega a "Talento Humano" en el menú lateral
2. Haz clic en "Nuevo Personal"
3. Selecciona la Sub-IPS donde labora el personal
4. Ingresa los datos del trabajador y su cargo
5. Haz clic en "Guardar"

### Configurar Capacidad Instalada

1. Navega a "Capacidad Instalada" en el menú lateral
2. Haz clic en "Nueva Capacidad"
3. Selecciona la Sub-IPS y el servicio
4. Configura el número de consultorios, tiempo de consulta y horarios
5. Haz clic en "Guardar"

### Generar Informes

1. Navega a "Informes" en el menú lateral
2. Selecciona "Consolidado IPS" o "Detalle Sub-IPS"
3. Elige la IPS o Sub-IPS del listado
4. Visualiza el informe o impnímelo directamente

## Tipos de Sede Disponibles

El sistema incluye los siguientes tipos de sede predefinidos:

- **CALIPSO**: Sede de tipo CALIPSO
- **POBLADO**: Sede de tipo Poblado (POBLADO I, POBLADO II, etc.)
- **COMUNEROS**: Sede de tipo Comuneros (COMUNEROS I, COMUNEROS II, etc.)
- **NUEVO MILENIO**: Sede Nuevo Milenio
- **JAMUNDI**: Sede Jamundí
- **CENTRO, NORTE, SUR, ESTE, OESTE**: Sedes por ubicación cardinal

## Capacidades de Cálculo

### Slots Disponibles

El sistema calcula automáticamente los "slots" o cupos disponibles según la fórmula:

```
Slots Semanales = (Horas Totales × 60 ÷ Tiempo por Consulta) × Número de Consultorios
```

Donde las horas totales se calculan considerando las horas de lunes a viernes y los días de atención configurados.

### Capacidad Resolutiva

La capacidad resolutiva representa la cantidad máxima teórica de pacientes que pueden ser atendidos en un periodo de tiempo, considerando los recursos físicos (consultorios) y el tiempo promedio de atención.

## Soporte y Contacto

Para soporte técnico o reporte de errores, contacta al administrador del sistema.

## Licencia

Sistema de uso interno para gestión de suficiencia de red IPS.
