# Sistema de Suficiencia de Red IPS - Especificación Técnica

## 1. Proyecto y Contexto

### 1.1 Nombre del Proyecto
Sistema de Suficiencia de Red IPS - Módulo de Capacidad Instalada y Resolución

### 1.2 Tipo de Proyecto
Aplicación web PHP con MySQL para gestión de salud pública

### 1.3 Funcionalidad Principal
Sistema para gestionar la suficiencia de red de Instituciones Prestadoras de Servicios (IPS), incluyendo la relación jerárquica entre IPS principales y Sub-IPS (sedes), talento humano por sede, capacidad instalada, y generación de informes consolidados y desagregados.

### 1.4 Usuarios Objetivo
- Administradores de IPS
- Coordinadores de salud
- Personal de planificación
- Directores de instituciones de salud

## 2. Estructura de Datos

### 2.1 Modelo Relacional

```
┌─────────────┐       ┌─────────────┐
│    ips      │───────│  subips     │
│ (Principal) │  1:N  │ (Sedes)     │
└─────────────┘       └─────────────┘
                             │
              ┌───────────────┼───────────────┐
              │               │               │
              ▼               ▼               ▼
      ┌───────────────┐ ┌───────────────┐ ┌───────────────┐
      │ talento_      │ │ capacidad_    │ │ registro_    │
      │ humano        │ │ instalada     │ │ produccion   │
      └───────────────┘ └───────────────┘ └───────────────┘
```

### 2.2 Tablas del Sistema

#### 2.2.1 ips (Instituciones Prestadoras de Servicios)
| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT (PK) | Identificador único |
| nit | VARCHAR(20) | Número de identificación tributaria |
| nombre | VARCHAR(255) | Nombre de la IPS |
| direccion | VARCHAR(255) | Dirección principal |
| telefono | VARCHAR(50) | Teléfono de contacto |
| email | VARCHAR(100) | Correo electrónico |
| nivel | VARCHAR(20) | Nivel de atención (PRIMARIO, SECUNDARIO, TERCIARIO) |
| municipio | VARCHAR(100) | Municipio |
| departamento | VARCHAR(100) | Departamento |
| activo | TINYINT(1) | Estado (1=activo, 0=inactivo) |
| fecha_registro | TIMESTAMP | Fecha de registro |

#### 2.2.2 subips (Sub-Instituciones Prestadoras de Servicios)
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

#### 2.2.3 talento_humano (Personal de Salud)
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

#### 2.2.4 capacidad_instalada (Recursos Físicos)
| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT (PK) | Identificador único |
| subips_id | INT (FK) | Referencia a Sub-IPS |
| servicio_id | INT (FK) | Referencia a servicio |
| consultorios | INT | Número de consultorios |
| unidades | INT | Número de unidades |
| tiempo_consulta | INT | Tiempo por consulta (minutos) |
| dias_semana | VARCHAR(50) | Días de atención |
| horas_dia_lunes_viernes | INT | Horas周一 a周五 |
| horas_dia_sabado | INT | Horas sábado |
| fecha_registro | TIMESTAMP | Fecha de registro |

#### 2.2.5 servicios_maestros (Catálogo de Servicios)
| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT (PK) | Identificador único |
| codigo | VARCHAR(20) | Código del servicio |
| nombre | VARCHAR(255) | Nombre del servicio |
| grupo | ENUM | Grupo de servicio |
| complejidad | ENUM | Nivel de complejidad |
| tiempo_promedio_atencion | INT | Tiempo promedio |
| activo | TINYINT(1) | Estado |

#### 2.2.6 cargos_maestros (Catálogo de Cargos)
| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT (PK) | Identificador único |
| codigo | VARCHAR(20) | Código del cargo |
| nombre | VARCHAR(255) | Nombre del cargo |
| area | VARCHAR(50) | Área (ASISTENCIAL, ADMINISTRATIVA) |
| nivel | VARCHAR(50) | Nivel (PROFESIONAL, TÉCNICO, etc.) |
| activo | TINYINT(1) | Estado |

#### 2.2.7 registro_produccion (Producción Mensual)
| Campo | Tipo | Descripción |
|-------|------|-------------|
| id | INT (PK) | Identificador único |
| capacidad_id | INT (FK) | Referencia a capacidad instalada |
| mes_referencia | VARCHAR(7) | Mes de referencia (YYYY-MM) |
| pacientes_atendidos | INT | Pacientes atendidos |
| fecha_registro | TIMESTAMP | Fecha de registro |

## 3. Funcionalidades del Sistema

### 3.1 Gestión de IPS
- Crear, editar, eliminar IPS principales
- Listar IPS activas
- Ver detalle de IPS con sus Sub-IPS asociadas

### 3.2 Gestión de Sub-IPS
- Crear Sub-IPS asociada a una IPS padre
- Editar información de Sub-IPS
- Listar Sub-IPS por IPS
- Trasladar Sub-IPS entre IPS

### 3.3 Gestión de Talento Humano
- Registrar personal asociado a Sub-IPS
- Editar información del personal
- Listar personal por Sub-IPS
- Ver distribución de talento humano por sede

### 3.4 Gestión de Capacidad Instalada
- Configurar capacidad instalada por Sub-IPS y servicio
- Registrar número de consultorios, unidades
- Configurar horarios y días de atención
- Registrar tiempos de consulta

### 3.5 Registro de Producción
- Registrar producción mensual por capacidad instalada
- Ver historial de producción por sede
- Comparar capacidad vs producción real

### 3.6 Informes
- Informe consolidado por IPS (suma de todas las Sub-IPS)
- Informe detallado por Sub-IPS
- Estadísticas de capacidad resolutiva
- Distribución de talento humano
- Productividad por sede

## 4. Diseño Visual

### 4.1 Paleta de Colores
- **Primario**: #2E86AB (Azul profesional de salud)
- **Secundario**: #A23B72 (Magenta para acentos)
- **Éxito**: #28A745 (Verde para estados positivos)
- **Advertencia**: #FFC107 (Amarillo para alertas)
- **Error**: #DC3545 (Rojo para errores)
- **Fondo**: #F8F9FA (Gris claro)
- **Texto**: #343A40 (Gris oscuro)
- **Blanco**: #FFFFFF

### 4.2 Tipografía
- **Títulos**: 'Segoe UI', sans-serif - 600 weight
- **Cuerpo**: 'Segoe UI', sans-serif - 400 weight
- **Monospace**: 'Consolas', monospace (para códigos)

### 4.3 Espaciado
- Base: 8px
- Margen contenedor: 24px
- Padding cards: 16px
- Gap elementos: 16px

### 4.4 Componentes UI
- Cards con sombra sutil
- Botones primarios y secundarios
- Tablas responsive con hover
- Formularios con validación visual
- Navegación lateral (sidebar)
- Breadcrumbs para navegación jerárquica

## 5. Arquitectura Técnica

### 5.1 Estructura de Archivos
```
suficiencia-ips/
├── config/
│   └── database.php
├── models/
│   ├── Ips_model.php
│   ├── Subips_model.php
│   ├── Talento_humano_model.php
│   ├── Capacidad_instalada_model.php
│   └── Servicios_model.php
├── views/
│   ├── templates/
│   │   ├── header.php
│   │   └── footer.php
│   ├── ips/
│   │   ├── listar.php
│   │   └── crear_editar.php
│   ├── subips/
│   │   ├── listar.php
│   │   └── crear_editar.php
│   ├── talento_humano/
│   │   ├── listar.php
│   │   └── crear_editar.php
│   ├── capacidad/
│   │   ├── listar.php
│   │   └── crear_editar.php
│   ├── informes/
│   │   ├── consolidado_ips.php
│   │   └── detalle_subips.php
│   └── dashboard.php
├── controllers/
│   ├── Ips_controller.php
│   ├── Subips_controller.php
│   ├── Talento_controller.php
│   ├── Capacidad_controller.php
│   └── Informe_controller.php
├── js/
│   ├── main.js
│   └── validacion.js
├── css/
│   └── styles.css
├── index.php
└── database/
    └── esquema.sql
```

### 5.2 Tecnologías
- **Backend**: PHP 8.x
- **Base de datos**: MySQL 8.x
- **Frontend**: HTML5, CSS3, JavaScript Vanilla
- **Plantillas**: PHP include/require
- **Sin frameworks** (arquitectura ligera)

## 6. Criterios de Aceptación

### 6.1 Gestión de IPS
- [ ] Se puede crear una nueva IPS con todos los campos obligatorios
- [ ] Se puede editar una IPS existente
- [ ] Se puede eliminar una IPS (con confirmación)
- [ ] Se listan solo las IPS activas

### 6.2 Gestión de Sub-IPS
- [ ] Se puede crear una Sub-IPS seleccionando su IPS padre
- [ ] Se muestra la IPS padre en listados de Sub-IPS
- [ ] Se pueden listar Sub-IPS filtradas por IPS
- [ ] Se puede editar y eliminar Sub-IPS

### 6.3 Gestión de Talento Humano
- [ ] El personal se asocia a una Sub-IPS específica
- [ ] Se listan todos los recursos humanos con su sede
- [ ] Se puede filtrar por Sub-IPS
- [ ] Se muestran los cargos correctamente

### 6.4 Gestión de Capacidad Instalada
- [ ] La capacidad se asocia a una Sub-IPS específica
- [ ] Se listan los servicios disponibles por sede
- [ ] Se puede configurar horarios y días
- [ ] Se muestran estadísticas de capacidad

### 6.5 Informes
- [ ] El informe consolidado muestra datos de todas las Sub-IPS agrupadas por IPS
- [ ] El informe detallado muestra datos específicos de cada Sub-IPS
- [ ] Se puede seleccionar IPS y ver sus Sub-IPS
- [ ] Los informes son exportables a PDF

### 6.6 Interfaz
- [ ] El diseño es responsive
- [ ] La navegación es intuitiva
- [ ] Los formularios tienen validación
- [ ] Los mensajes de éxito/error son claros

## 7. Datos de Ejemplo

### 7.1 IPS de Ejemplo
```sql
INSERT INTO ips (nit, nombre, direccion, telefono, email, nivel, municipio, departamento) VALUES
('760010395701', 'HOSPITAL CARLOS HOLMES TRUJILLO', 'AV 3A BIS SUR 9D BIS 9 K 34', '0000000000', 'isjjas@gmail.com', 'PRIMARIO', 'Bogotá', 'Cundinamarca');
```

### 7.2 Sub-IPS de Ejemplo
```sql
INSERT INTO subips (ips_id, codigo, nombre, direccion, tipo_sede, barrio) VALUES
(1, 'CAL01', 'CALIPSO', 'Calle 1 # 2-3', 'CALIPSO', 'Calipso'),
(1, 'POB01', 'POBLADO II', 'Carrera 5 # 6-7', 'POBLADO', 'Poblado II'),
(1, 'COM01', 'COMUNEROS II', 'Avenida 8 # 9-10', 'COMUNEROS', 'Comuneros II');
```
