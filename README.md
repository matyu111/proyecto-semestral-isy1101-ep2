# Proyecto Semestral ISY1101 - Evaluación Parcial N°2

## Descripción del proyecto

Este proyecto corresponde a la Evaluación Parcial N°2 de la asignatura Introducción a Herramientas DevOps. La solución implementa una aplicación de despacho compuesta por un Frontend desarrollado en React/Vite y dos microservicios Backend desarrollados con Spring Boot.

El objetivo principal es aplicar prácticas DevOps mediante contenedorización con Docker, orquestación con Docker Compose, persistencia de datos con volúmenes y preparación para despliegue automatizado en AWS EC2 mediante GitHub Actions.

## Arquitectura de la solución

La solución está compuesta por los siguientes servicios:

- Frontend: aplicación React/Vite servida mediante Nginx.
- Backend Ventas: microservicio Spring Boot encargado de gestionar órdenes de compra.
- Backend Despachos: microservicio Spring Boot encargado de gestionar órdenes de despacho.
- Base de datos MySQL: servicio encargado de almacenar la información persistente.

## Tecnologías utilizadas

- Docker
- Docker Compose
- Java 17
- Spring Boot
- Maven
- React
- Vite
- Nginx
- MySQL 8.0
- Git
- GitHub
- AWS EC2
- GitHub Actions

## Contenedorización

Cada componente cuenta con su propio Dockerfile:

- `front_despacho/Dockerfile`
- `back-Ventas_SpringBoot/Springboot-API-REST/Dockerfile`
- `back-Despachos_SpringBoot/Springboot-API-REST-DESPACHO/Dockerfile`

Los Dockerfile de Backend utilizan multi-stage build con Maven y Java 17. Además, ejecutan la aplicación con un usuario no root para mejorar la seguridad del contenedor.

El Frontend utiliza una primera etapa con Node para construir la aplicación y una segunda etapa con Nginx para servir los archivos estáticos optimizados.

## Docker Compose

El archivo `docker-compose.yml` permite levantar todo el stack de servicios:

- `mysql`
- `backend-ventas`
- `backend-despachos`
- `frontend`

El stack utiliza una red interna llamada `tienda-net`, lo que permite la comunicación entre servicios mediante nombres de contenedor.

## Puertos utilizados

| Servicio | Puerto local | Puerto contenedor |
|---|---:|---:|
| Frontend | 8080 | 80 |
| Backend Ventas | 8081 | 8080 |
| Backend Despachos | 8082 | 8081 |
| MySQL | 3306 | 3306 |

## Persistencia de datos

La base de datos MySQL utiliza un volumen Docker llamado `dbdata`, montado en:

```text
/var/lib/mysql
```

Esto permite que la información almacenada no se pierda al detener o reiniciar los contenedores.

## Ejecución local

Desde la raíz del proyecto ejecutar:

```bash
docker compose build
docker compose up -d
docker compose ps
```

Para detener los servicios:

```bash
docker compose down
```

## URLs de prueba local

Frontend:

```text
http://localhost:8080
```

Backend Ventas:

```text
http://localhost:8081/api/v1/ventas
```

Backend Despachos:

```text
http://localhost:8082/api/v1/despachos
```

## Evidencias funcionales

Durante las pruebas locales se validó:

- Construcción correcta de imágenes Docker.
- Ejecución de contenedores con Docker Compose.
- Persistencia mediante volumen Docker.
- Frontend accesible en navegador.
- Backend Ventas respondiendo correctamente.
- Backend Despachos respondiendo correctamente.
- Comunicación Frontend hacia Backend mediante peticiones HTTP con estado 200.

## Preparación para AWS EC2

La solución queda preparada para ser desplegada en instancias EC2, considerando:

- Frontend accesible desde Internet mediante IP pública o dominio.
- Backends comunicándose con la base de datos mediante red privada.
- Uso de Security Groups para restringir accesos.
- Publicación de imágenes en Docker Hub o Amazon ECR.
- Automatización del despliegue mediante GitHub Actions sobre la rama deploy.

## Pipeline CI/CD esperado

El flujo de integración y despliegue continuo considera:

- Push sobre la rama deploy.
- Construcción de imagen Docker.
- Publicación de imagen en Docker Hub o Amazon ECR.
- Conexión a EC2.
- Descarga de la nueva imagen.
- Reinicio del contenedor actualizado.

## Principios DevOps aplicados

- Contenedorización de aplicaciones.
- Separación de responsabilidades por servicio.
- Automatización del despliegue.
- Control de versiones con Git.
- Uso de infraestructura cloud en AWS.
- Persistencia de datos con volúmenes.
- Reproducibilidad del entorno mediante Docker Compose.
