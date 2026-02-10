---
date: Febrero 2026
title: Documentación ERP - Módulo de Compras ![arc42](Docs/images/arc42-logo.png)
---

# Acerca de arc42

arc42, La plantilla de documentación para arquitectura de sistemas y de software.

Por Dr. Gernot Starke, Dr. Peter Hruschka y otros contribuyentes.

Revisión de la plantilla: 7.0 ES (basada en asciidoc), Enero 2017

© Reconocemos que este documento utiliza material de la plantilla de
arquitectura arc42, <https://www.arc42.org>. Creada por Dr. Peter
Hruschka y Dr. Gernot Starke.

# Introducción y Metas {#section-introduction-and-goals}

## Vista de Requerimientos {#_vista_de_requerimientos}

El Sistema ERP del Módulo de Compras tiene como objetivo centralizar y automatizar los procesos de compras de la empresa.

### Requisitos de negocio principales
- Registrar productos con información básica (nombre, descripción, unidad de medida).
- Gestionar proveedores y sus datos de contacto.
- Asociar productos a proveedores y precios.
- Crear y registrar órdenes de compra.
- Consultar historial de órdenes de compra para análisis y reportes.

## Metas de Calidad {#_metas_de_calidad}
- Sistema confiable y sin pérdida de datos.
- Interfaz amigable para los gestores.
- Procesamiento rápido de solicitudes de compra.

## Partes interesadas (Stakeholders) {#_partes_interesadas_stakeholders}

+------------------+----------------+----------------------------+
| Rol/Nombre       | Contacto       | Expectativas              |
+==================+================+============================+
| Gestor de Compras| correo@empresa | Registrar productos y proveedores fácilmente |
+------------------+----------------+----------------------------+
| Jefe de Compras  | correo@empresa | Analizar historial de compras y reportes     |
+------------------+----------------+----------------------------+
| Analista de Compras| correo@empresa | Gestionar proveedores y precios              |
+------------------+----------------+----------------------------+

# Restricciones de la Arquitectura {#section-architecture-constraints}

- Backend: Java con Spring Boot
- Frontend: SPA con React/JavaScript
- Base de datos: PostgreSQL
- Comunicación: HTTPS/JSON entre frontend y backend
- Arquitectura: Monolítica simple para el módulo de compras

# Alcance y Contexto del Sistema {#section-context-and-scope}

## Contexto de Negocio {#_contexto_de_negocio}

El ERP del Módulo de Compras interactúa con usuarios internos (gestores y jefes de compras) y sistemas externos de contabilidad para formalizar la adquisición de productos y mantener datos actualizados.

![Diagrama de Contexto](docs/images/c1_context.png)

## Contexto Técnico {#_contexto_técnico}

- SPA: Interfaz web que los usuarios utilizan para registrar productos y órdenes.
- API Monolítica: Gestiona la lógica de negocio y procesa solicitudes del frontend.
- Base de datos PostgreSQL: Almacena productos, proveedores, relaciones y órdenes de compra.

# Estrategia de solución {#section-solution-strategy}

El sistema utiliza una arquitectura monolítica simple para garantizar rapidez en el desarrollo y facilidad de mantenimiento. La SPA se comunica con la API que gestiona toda la lógica de negocio y la base de datos.

# Vista de Bloques {#section-building-block-view}

## Sistema General de Caja Blanca {#_sistema_general_de_caja_blanca}

![Diagrama de Contenedores](docs/images/c2_containers.png)

### Contenedores

- **SPA (Aplicación Web)**: Permite a los usuarios interactuar con el sistema para registrar productos, gestionar proveedores y crear órdenes de compra.
- **API Monolítica**: Procesa todas las solicitudes, valida datos y realiza operaciones sobre la base de datos.
- **Base de Datos (PostgreSQL)**: Almacena productos, proveedores, relaciones y órdenes de compra.

# Vista de Ejecución {#section-runtime-view}

## Escenario crítico: Registrar un Producto

1. El gestor completa el formulario de nuevo producto en la SPA.
2. La SPA envía los datos a la API mediante POST /api/productos.
3. La API valida los datos y los inserta en la base de datos.
4. La base de datos devuelve el ID del producto creado.
5. La API responde a la SPA con estado 201 Created.
6. La SPA muestra un mensaje de éxito y actualiza la lista de productos.

![Diagrama de Secuencia](docs/images/diagrama_secuencia.png)

# Modelo de Datos {#section-data-model}

![Diagrama Entidad-Relación](docs/images/diagrama_mer.png)

- **Producto**: id, nombre, descripción, unidad de medida.
- **Proveedor**: id, nombre, contacto.
- **Producto_Proveedor**: id_producto, id_proveedor, precio_unitario.
- Relaciones: Un producto puede tener múltiples proveedores, un proveedor puede ofrecer múltiples productos.

# Vista de Despliegue {#section-deployment-view}

![Diagrama de Despliegue](docs/images/diagrama_despliegue.png)

- La SPA se despliega en un servidor web (NGINX o similar).
- La API Monolítica corre en un servidor de aplicaciones Java/Spring Boot.
- La base de datos PostgreSQL puede estar en el mismo servidor o en un servidor dedicado.
- Todo el tráfico es seguro mediante HTTPS.

# Conceptos Transversales (Cross-cutting) {#section-concepts}

- Seguridad: Autenticación de usuarios y permisos según roles.
- Integridad de datos: Validaciones en API para evitar inconsistencias.
- Escalabilidad: Aunque es monolítico, la arquitectura permite migración a microservicios futura.

# Decisiones de Diseño {#section-design-decisions}

- Usar arquitectura monolítica para simplificar despliegue.
- Separación clara entre frontend (SPA) y backend (API).
- PostgreSQL como base de datos principal por su robustez y soporte de transacciones.

# Requerimientos de Calidad {#section-quality-scenarios}

- Alta disponibilidad: El sistema debe estar disponible > 99% del tiempo.
- Rendimiento: Registro de un producto debe completarse en <2 segundos.
- Seguridad: Datos de proveedores y productos cifrados en base de datos.

# Riesgos y deuda técnica {#section-technical-risks}

- Riesgo de dependencia de un único servidor en la versión monolítica.
- Posible dificultad para escalar verticalmente con alto volumen de pedidos.
- Necesidad futura de migrar a microservicios si la empresa crece.

# Glosario {#section-glossary}

+------------------+-------------------------------------------+
| Término          | Definición                                |
+==================+===========================================+
| Producto         | Bien o artículo que se puede comprar.    |
+------------------+-------------------------------------------+
| Proveedor        | Entidad que suministra productos.        |
+------------------+-------------------------------------------+
| Orden de Compra  | Documento que formaliza la adquisición. |
+------------------+-------------------------------------------+
| Gestor de Compras| Usuario que registra productos y órdenes.|
+------------------+-------------------------------------------+
| Jefe de Compras  | Usuario que analiza historial y reportes.|
+------------------+-------------------------------------------+


