# Intranet Jet - Sistema de Gestión de Vehículos

## Descripción

Intranet Jet es una aplicación web moderna desarrollada para la gestión integral de vehículos. La plataforma permite a los usuarios consultar catálogos de vehículos, realizar reservas (bookings), gestionar información de marcas y acceder a galerías de imágenes mediante una interfaz intuitiva y responsiva.

## Tecnologías Principales

### Frontend
- **React 18**: Framework JavaScript para construcción de interfaces de usuario interactivas
- **Vite**: Bundler y servidor de desarrollo de alto rendimiento
- **Tailwind CSS**: Framework CSS utility-first para diseño responsivo
- **JavaScript ES6+**: Lenguaje de programación moderno

## Estructura del Proyecto

```
intranet-jet/
├── src/
│   ├── components/          # Componentes reutilizables
│   │   ├── acordeonCoche.jsx          # Acordeón para información de vehículos
│   │   ├── dialogoOpcion.jsx          # Diálogos modales
│   │   ├── formularioBooking.jsx      # Formulario de reservas
│   │   ├── formularioNuevoCoche.jsx   # Formulario para agregar vehículos
│   │   ├── galeriaImagenes.jsx        # Galería de imágenes
│   │   ├── marcasImagenes.jsx         # Gestión de imágenes de marcas
│   │   ├── selectConOpciones.jsx      # Select personalizado
│   │   └── SelectMultiple.jsx         # Select múltiple
│   ├── hooks/               # Custom React hooks
│   ├── pages/               # Páginas principales
│   │   └── mainPage.jsx     # Página principal
│   ├── services/            # Servicios (API calls, lógica de negocio)
│   ├── utils/               # Funciones utilitarias
│   ├── assets/              # Recursos estáticos (imágenes, íconos)
│   ├── App.jsx              # Componente raíz
│   ├── App.css              # Estilos globales
│   ├── index.css            # Estilos base
│   └── main.jsx             # Punto de entrada
├── public/                  # Archivos públicos estáticos
├── vite.config.js           # Configuración de Vite
├── tailwind.config.js       # Configuración de Tailwind CSS
├── eslint.config.js         # Configuración de ESLint
├── .env.local               # Variables de entorno locales
├── package.json             # Dependencias y scripts
└── index.html               # Archivo HTML principal
```

## Características Principales

- **Gestión de Vehículos**: Consulta y administración de catálogo de vehículos
- **Sistema de Reservas**: Formulario completo para booking de vehículos
- **Galería de Imágenes**: Visualización de catálogo multimedia
- **Gestión de Marcas**: Administración de información sobre marcas de vehículos
- **Interfaz Responsiva**: Diseño adaptado para dispositivos móviles y desktop
- **Componentes Reutilizables**: Arquitectura modular para mantenibilidad

## Arquitectura y Patrones

### Componentes
- **Componentes Funcionales**: Utilizando React Hooks para lógica de estado
- **Props Drilling**: Paso de props entre componentes
- **Custom Hooks**: Lógica reutilizable encapsulada en hooks personalizados

### Styling
- **Tailwind CSS**: Clases utilitarias para estilos rápidos y consistentes
- **CSS Modules**: Estilos modular cuando es necesario aislamiento

### Servicios
- Separación de lógica de negocio en servicios independientes
- Gestión de llamadas a APIs y solicitudes HTTP

## Variables de Entorno

Las variables de entorno se configuran en el archivo `.env.local`:

```
VITE_API_URL=<url-api>
VITE_APP_NAME=Intranet Jet
```

## Mejores Prácticas Implementadas

 Componentes pequeños y enfocados (Single Responsibility)
- Separación de responsabilidades (Services, Hooks, Components)
- Código limpio y legible con ESLint
- Estructura escalable y modular
- Reutilización de componentes
- Responsive Design con Tailwind CSS

**Última actualización**: 2025
