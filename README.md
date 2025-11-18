# Proyecto Final – MiniWebApp con Docker, AWS, Prometheus y Grafana

Este proyecto implementa un sistema web completo compuesto por:

- Backend Flask + MySQL
- Reverse Proxy con Nginx + HTTPS (certificado self-signed)
- Despliegue en EC2 (AWS)
- Infraestructura basada en contenedores Docker
- Sistema de Monitoreo con Prometheus + Node Exporter
- Dashboard de observabilidad con Grafana

El objetivo del proyecto es diseñar, contenerizar, desplegar y monitorear un sistema web real, siguiendo prácticas básicas de DevOps.

## Arquitectura del sistema

```
               ┌──────────────────────────┐
               │        Usuario Web        │
               └──────────────┬───────────┘
                              │  HTTPS/443
                   ┌──────────▼──────────┐
                   │       NGINX         │
                   │ Reverse Proxy + SSL │
                   └──────────┬──────────┘
                              │ proxy_pass
                     ┌────────▼──────────┐
                     │     Flask App     │
                     │   (Gunicorn)      │
                     └────────┬──────────┘
                              │
                     ┌────────▼──────────┐
                     │      MySQL DB     │
                     └────────────────────┘

     ┌──────────────────────────────────────────────┐
     │        Prometheus + Node Exporter             │
     │        Monitoreo de CPU, RAM, Disco           │
     └──────────────────────────────────────────────┘

     ┌──────────────────────────────────────────────┐
     │                  Grafana                     │
     │       Visualización de métricas en tiempo real│
     └──────────────────────────────────────────────┘
```

Sistema completamente orquestado mediante Docker Compose.

## Despliegue en AWS EC2

### 1. Configuración de la instancia

**Tipo:** t2.small (2 GB RAM, recomendado para Prometheus + Grafana)

**Puertos abiertos:**
- 22 (SSH)
- 80 (HTTP)
- 443 (HTTPS)
- 9090 (Prometheus)
- 9100 (Node Exporter)
- 3000 (Grafana)

### 2. Instalación de Docker en EC2

```bash
sudo apt update
sudo apt install docker.io docker-compose
sudo systemctl enable docker
sudo systemctl start docker
```

### 3. Clonar el repositorio

```bash
git clone https://github.com/USUARIO/REPO.git
cd REPO
```

### 4. Levantar la infraestructura

```bash
sudo docker-compose up --build -d
```

## Sistema de Monitoreo

### Node Exporter

Recolecta métricas de:
- CPU
- RAM
- Disco
- Sistema de archivos

### Prometheus

Scrapea métricas cada 15s y evalúa reglas de alerta.

### Grafana

Dashboard personalizado con:
- CPU Usage (%)
- Memory Usage (%)
- Disk Usage (%)

## Evidencias del despliegue

Agrega aquí tus capturas:

- Vista de la aplicación en HTTPS
- Prometheus Targets UP
- Node Exporter funcionando
- Grafana con dashboard de CPU/RAM/Disco
- Contenedores en docker ps
- Estructura del repositorio

(Puedes incluir imágenes o un video corto subido a YouTube o al repositorio.)

## Conclusión Técnica

Durante el desarrollo e implementación del proyecto aprendí a integrar múltiples tecnologías de infraestructura moderna:

### Integración Docker + AWS + Prometheus

Comprendí cómo contenerizar servicios, exponerlos mediante redes internas, orquestar múltiples máquinas con Docker Compose, y desplegarlos en una instancia EC2 accesible vía HTTPS.

También aprendí a integrar herramientas de observabilidad (Prometheus y Grafana) para monitorear métricas reales de un servidor productivo.

### Dificultades enfrentadas

Una de las partes más complejas fue manejar el despliegue sobre AWS desde diferentes redes. Al trabajar desde distintas casas (la de mi madre, mis abuelos y la mía), tuve que ajustar constantemente las reglas del Security Group, porque mi IP cambiaba cada vez que me conectaba desde un lugar diferente. Esto me obligó a comprender mejor cómo funciona la seguridad de AWS y cómo administrar acceso remoto correctamente.

Además, fue desafiante administrar recursos en una instancia pequeña (t2.micro). Al usar servicios como Grafana y Prometheus, el servidor se quedaba sin memoria.

En un entorno real, solucionaría esto con:
- Instancias más grandes
- Autoescalado
- Contenedores livianos
- Monitoreo de consumo antes del despliegue

### Beneficios de la observabilidad en DevOps

La observabilidad permite:
- Detectar fallos antes de que afecten al usuario
- Visualizar el comportamiento real del sistema
- Tomar decisiones técnicas basadas en métricas
- Automatizar alertas y respuestas
- Aumentar la confiabilidad del despliegue

Sin observabilidad, un sistema puede estar fallando sin que nadie lo note; con observabilidad, los problemas se vuelven visibles, medibles y solucionables.

## Repositorio

https://github.com/xJuanes21/parcial-telematicos-final
