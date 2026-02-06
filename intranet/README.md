# Intranet Jet - Vehicle Management System

## Description

Intranet Jet is a modern web application developed for comprehensive vehicle management. The platform allows users to browse vehicle catalogs, make bookings, manage brand information, and access image galleries through an intuitive and responsive interface.

## Main Technologies

### Frontend
- **React 18**: JavaScript framework for building interactive user interfaces
- **Vite**: High-performance bundler and development server
- **Tailwind CSS**: Utility-first CSS framework for responsive design
- **JavaScript ES6+**: Modern programming language

### Development Tools
- **ESLint**: Linter to maintain code quality and consistency
- **npm**: Dependency manager and scripts

## Project Structure

```
intranet-jet/
├── src/
│   ├── components/          # Reusable components
│   │   ├── acordeonCoche.jsx          # Accordion for vehicle information
│   │   ├── dialogoOpcion.jsx          # Modal dialogs
│   │   ├── formularioBooking.jsx      # Booking form
│   │   ├── formularioNuevoCoche.jsx   # Form to add vehicles
│   │   ├── galeriaImagenes.jsx        # Image gallery
│   │   ├── marcasImagenes.jsx         # Brand image management
│   │   ├── selectConOpciones.jsx      # Custom select
│   │   └── SelectMultiple.jsx         # Multiple select
│   ├── hooks/               # Custom React hooks
│   ├── pages/               # Main pages
│   │   └── mainPage.jsx     # Main page
│   ├── services/            # Services (API calls, business logic)
│   ├── utils/               # Utility functions
│   ├── assets/              # Static resources (images, icons)
│   ├── App.jsx              # Root component
│   ├── App.css              # Global styles
│   ├── index.css            # Base styles
│   └── main.jsx             # Entry point
├── public/                  # Public static files
├── vite.config.js           # Vite configuration
├── tailwind.config.js       # Tailwind CSS configuration
├── eslint.config.js         # ESLint configuration
├── .env.local               # Local environment variables
├── package.json             # Dependencies and scripts
└── index.html               # Main HTML file
```

## Main Features

- **Vehicle Management**: Browse and manage the vehicle catalog
- **Booking System**: Full booking form for vehicle reservations
- **Image Gallery**: Multimedia catalog viewing
- **Brand Management**: Manage vehicle brand information
- **Responsive Interface**: Design adapted for mobile and desktop
- **Reusable Components**: Modular architecture for maintainability

## Architecture and Patterns

### Components
- **Functional Components**: Using React Hooks for state logic
- **Props Drilling**: Passing props between components
- **Custom Hooks**: Reusable logic encapsulated in custom hooks

### Styling
- **Tailwind CSS**: Utility classes for fast, consistent styling
- **CSS Modules**: Modular styles when isolation is required

### Services
- Separation of business logic into independent services
- Management of API calls and HTTP requests

## Implemented Best Practices

- Small, focused components (Single Responsibility)
- Separation of concerns (Services, Hooks, Components)
- Clean, readable code with ESLint
- Scalable and modular structure
- Component reuse
- Responsive design with Tailwind CSS

**Last updated**: 2025
