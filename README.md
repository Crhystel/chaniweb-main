# ChaniWeb - Comparador de Precios de Supermercados

🛒 **Plataforma ecuatoriana para comparar precios en tiempo real**

[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](https://www.docker.com/)
[![React](https://img.shields.io/badge/React-19.2.3-61dafb.svg)](https://reactjs.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104.1-009688.svg)](https://fastapi.tiangolo.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue.svg)](https://www.postgresql.org/)

## 🚀 **Instalación y Ejecución**

### **Requisitos Previos**
- [Docker](https://www.docker.com/) y Docker Compose
- [Git](https://git-scm.com/) para clonar el repositorio

### **Pasos para Iniciar**
```bash
# 1. Clonar repositorio
git clone <URL_DEL_REPOSITORIO>
cd Chaniweb-2

# 2. Construir y levantar todos los servicios
docker-compose up --build -d

# 3. Verificar estado
docker-compose ps

# 4. Cargar datos de productos
docker-compose exec backend python scraper.py
```

### **Acceso a la Aplicación**
- **Frontend**: http://localhost/
- **API Backend**: http://localhost/api/productos
- **Sin puertos expuestos**: Acceso profesional vía proxy Nginx

## 🏗️ **Arquitectura del Sistema**

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend    │    │     Proxy      │    │    Backend     │
│   (React)     │◄──►│   (Nginx)      │◄──►│   (FastAPI)    │
│   Puerto 80    │    │   Puerto 80     │    │   Puerto 8000   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                        │
                                                        ▼
                                              ┌─────────────────┐
                                              │     Redis       │
                                              │   (Cache)      │
                                              │   Puerto 6379   │
                                              └─────────────────┘
                                                        │
                                                        ▼
                                              ┌─────────────────┐
                                              │  PostgreSQL     │
                                              │   (Base de     │
                                              │    Datos)       │
                                              │   Puerto 5432   │
                                              └─────────────────┘
```

## 📁 **Estructura del Proyecto**

```
Chaniweb-2/
├── README.md                    # 📋 Documentación principal
├── chaniweb-backend/          # 🚀 API FastAPI
│   ├── README.md              # Documentación del backend
│   ├── main.py              # Endpoints y lógica
│   ├── models.py            # Modelos SQLAlchemy
│   ├── database.py         # Conexión a BD
│   └── requirements.txt     # Dependencias Python
├── chaniweb-frontend/         # ⚛️ React App
│   ├── README.md              # Documentación del frontend
│   ├── src/
│   │   ├── App.js         # Componente principal
│   │   └── App.css        # Estilos modernos
│   ├── public/
│   │   └── index.html     # Metadatos SEO
│   └── package.json       # Dependencias Node
├── chaniweb-ingesta/            # 🕷️ Scraper de datos
│   ├── README.md              # Documentación del ingesta
│   └── scraper.py         # Ingestión de productos
├── chaniweb-proxy/             # 🌐 Nginx Proxy
│   ├── README.md              # Documentación del proxy
│   └── nginx.conf         # Configuración proxy
├── docker-compose.yml          # 🐳 Orquestación Docker
├── .env                     # 🔧 Variables de entorno
└── k8s/                       # ☸️ Configuración Kubernetes
```

## 📊 **Datos del Sistema**

### **Productos Disponibles**
- **Total**: 168 productos distribuidos en 9 categorías
- **Supermercados**: Supermaxi, Akí, Mi Comisariato
- **Imágenes**: URLs reales verificadas de Walmart, Supermaxi, Facundo

### **Categorías**
1. **Granos**: Arroz, Lentejas, Garbanzos, Fréjoles, Quinua
2. **Lácteos**: Leche, Yogurt, Mantequilla, Margarina
3. **Proteínas**: Pollo, Carne, Jamón, Huevos, Tocino
4. **Enlatados**: Atún, Sardinas
5. **Despensa**: Pastas, Condimentos, Aceites, Harinas
6. **Bebidas**: Café, Gaseosas
7. **Panadería**: Pan, Galletas
8. **Repostería**: Cocoa, Tapioca
9. **Endulzantes**: Miel, Panela, Azúcar

## 🔧 **Configuración**

### **Variables de Entorno**
```bash
DB_USER=chaniweb_user
DB_PASSWORD=TU_PASSWORD_SEGURO
DB_NAME=chaniweb_db
```

## 🚀 **Ejecución en Diferentes Entornos**

### **Desarrollo Local**
```bash
# Iniciar todos los servicios
docker-compose up --build -d

# Ver logs
docker-compose logs -f

# Entrar a contenedores
docker-compose exec backend sh
docker-compose exec frontend sh
```

### **Producción**
```bash
# Variables de entorno producción
export DATABASE_URL=postgresql://user:PASSWORD_SEGURO@prod-db:5432/chaniweb
export REDIS_URL=redis://prod-redis:6379

# Deploy con datos frescos
docker-compose -f docker-compose.prod.yml up --build -d
```

### **Kubernetes**
```bash
# Aplicar configuración k8s
kubectl apply -f k8s/

# Verificar estado
kubectl get pods
kubectl get services
```

## 🐛 **Troubleshooting**

### **Problemas Comunes**
```bash
# Si las imágenes no se muestran
docker-compose exec backend python scraper.py

# Si el frontend muestra versión vieja
docker-compose stop frontend
docker-compose build --no-cache frontend
docker-compose up -d frontend

# Verificar estado de servicios
docker-compose ps

# Limpiar y reconstruir todo
docker-compose down
docker system prune -f
docker-compose up --build -d
```

## 📝 **Documentación por Componente**

### **Backend** 📖
Ver `chaniweb-backend/README.md` para:
- API endpoints y documentación
- Modelo de datos y estructura
- Configuración y dependencias
- Ejecución y troubleshooting

### **Frontend** 📖  
Ver `chaniweb-frontend/README.md` para:
- Arquitectura de componentes React
- Sistema de diseño y CSS moderno
- Estado y lógica de la aplicación
- Responsive design y optimización

### **Ingesta** 📖
Ver `chaniweb-ingesta/README.md` para:
- Estructura de datos y productos
- Sistema anti-duplicados
- URLs reales de supermercados
- Flujo completo de ingesta

### **Proxy** 📖
Ver `chaniweb-proxy/README.md` para:
- Configuración Nginx y routing
- Headers de seguridad y CORS
- Health checks y monitoreo
- Configuración SSL para producción

