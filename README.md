# Documentación Técnica y Anexos  SIGEJOD

**Sistema de Gestión y Comunicación Escolar (SIGEJOD)**  
Trabajo Recepcional | Ingeniería de Software | Universidad Veracruzana (UV)

---

## 1. Descripción General

Este repositorio centraliza la documentación técnica, operativa, de infraestructura y de código fuente correspondiente al diseño, construcción y despliegue en producción del **Sistema de Gestión Escolar SIGEJOD**. Su estructura está orientada a garantizar la reproducibilidad del entorno en la nube, facilitar el mantenimiento evolutivo del código fuente y servir como referencia formal para la administración escolar, docentes y tutores.

---

## 2. Índice de Anexos Oficiales

| Identificador | Documento | Alcance y Contenido | Archivo |
| :--- | :--- | :--- | :--- |
| **Anexo A** | **Proceso de Despliegue** (*Deployment & DevOps*) | Topología de infraestructura en la nube, aprovisionamiento de PostgreSQL, variables de entorno en Render y Vercel, configuración DNS y servicio de correo transaccional mediante Resend. | [Anexo_A_Proceso_de_Despliegue_SIGEJOD.pdf](./Anexo_A_Proceso_de_Despliegue_SIGEJOD.pdf) |
| **Anexo B** | **Manual Técnico y Arquitectura** | Configuración del entorno de desarrollo local, arquitectura en capas, diccionario de datos relacional (9 entidades), esquema de seguridad híbrida (JWT y RTR) y catálogo de endpoints REST. | [Anexo_B_Manual_Técnico_SIGEJOD.pdf](./Anexo_B_Manual_Técnico_SIGEJOD.pdf) |
| **Anexo C** | **Manual de Operación y Usuario** | Guía operativa estructurada por perfil institucional (Administrador, Docente y Tutor) para activación de cuentas, gestión de grupos, captura de calificaciones, envío de avisos y consulta de boletas. | [Anexo_C_Manual_de_Usuario_SIGEJOD.pdf](./Anexo_C_Manual_de_Usuario_SIGEJOD.pdf) |
| **Anexo D** | **Manual de Código Fuente** (*Backend & Frontend*) | Documentación estructurada del código fuente completo del servidor API REST (Node.js, Express y PostgreSQL) y del cliente web SPA (React, Vite y Tailwind CSS). | [Anexo_D_Manual_Codigo_Fuente_SIGEJOD.pdf](./Anexo_D_Manual_Codigo_Fuente_SIGEJOD.pdf) |

---

## 3. Ecosistema de Repositorios y Entornos

El sistema opera bajo una arquitectura cliente-servidor desacoplada, distribuida en repositorios independientes bajo control de versiones Git:

| Componente | Pila Tecnológica | Repositorio Remoto | Entorno Desplegado |
| :--- | :--- | :--- | :--- |
| **Frontend (SPA)** | React 19, Vite, Tailwind CSS, Axios | `https://github.com/IvanOs20/sigeJob-client.git` | `https://sigejod.com` |
| **Backend (API REST)** | Node.js, Express 5, Sequelize, PostgreSQL | `https://github.com/IvanOs20/Alerta-Temprana.git` | `https://api.sigejod.com` |
| **Documentación** | Especificación Técnica y Manuales Oficiales | `https://github.com/IvanOs20/SIGEJOD-Documentacion-Tecnica.git` | Repositorio GitHub |

---

## 4. Resumen de Arquitectura y Seguridad

- **Autenticación Híbrida sin Persistencia Insegura:** Implementación de *Access Tokens* JWT (`HS256`, vigencia de 15 minutos) almacenados exclusivamente en memoria volátil (RAM) del cliente, en conjunto con *Refresh Tokens* opacos de 7 días transportados mediante cookies `HttpOnly`, `Secure` y `SameSite=None`.
- **Rotación de Tokens de Sesión (RTR):** Persistencia de hashes criptográficos `SHA-256` en la entidad `tb_sesiones` con invalidación atómica por uso y revocación preventiva ante detección de reutilización.
- **Control de Acceso Granular (RBAC y Mitigación BOLA/IDOR):** Segregación estricta de privilegios por rol (`admin`, `docente`, `tutor`) y validación de titularidad a nivel de registro en los controladores del servidor para expedientes académicos, calificaciones y notificaciones.
- **Infraestructura Cloud Desacoplada:** Despliegue continuo sobre **Vercel Edge Network** (aplicación cliente), **Render Cloud** (servicio API REST y base de datos PostgreSQL gestionada con cifrado SSL/TLS) y **Resend API** (mensajería transaccional sobre HTTPS).

---

## 5. Obtención del Código Fuente

Para replicar el entorno de desarrollo local siguiendo los procedimientos especificados en el **Anexo B**:

```bash
# Clonar el repositorio del servidor Backend (API REST)
git clone [https://github.com/IvanOs20/Alerta-Temprana.git](https://github.com/IvanOs20/Alerta-Temprana.git)

# Clonar el repositorio del cliente Frontend (SPA)
git clone [https://github.com/IvanOs20/sigeJob-client.git](https://github.com/IvanOs20/sigeJob-client.git)
