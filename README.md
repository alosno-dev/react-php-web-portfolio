# Jet Rent a Car - Technical Documentation

## Table of Contents

1. [Overview](#overview)
2. [Technology Stack](#technology-stack)
3. [Architecture](#architecture)
4. [Best Practices](#best-practices)
5. [Main Modules](#main-modules)
6. [Optimizations](#optimizations)
7. [Security](#security)

---

## Overview

[**Jet Rent a Car**](https://jet-rentacar.com) is a vehicle rental and leasing platform built with a modern, scalable architecture. The project follows full-stack best practices with a clear separation between frontend and backend.

Key features:
- Dynamic vehicle catalog
- Flexible leasing system
- Responsive interface with PWA capabilities
- Multi-language support (ES/EN)
- Optimized caching system
- API and database security
- Intelligent image processing

---

## Technology Stack

### Frontend
```
React 18+ (with Hooks)
├── Vite (fast development/build tool)
├── Tailwind CSS (utility-first styling)
├── React Router v6 (SPA routing)
├── React i18next (internationalization)
├── Embla Carousel (accessible carousels)
├── Lucide Icons (modern iconography)
└── React Select (form components)
```

### Backend
```
PHP 8.0+
├── PDO (database access)
├── PHPMailer (email sending)
├── cURL (external integrations)
├── ImageMagick / GD (image processing)
└── Composer (dependency manager)
```

### Database
```
MySQL 5.7+
├── ACID transactions
├── Optimized indexes
├── Many-to-many relationships
└── Views for reporting
```

### DevOps & Infrastructure
```
Hostinger (shared hosting)
├── cPanel (administration)
├── FTP (file transfer)
├── Cron Jobs (scheduled tasks)
└── SSL/HTTPS (secure communications)
```

### External Tools
```
n8n (workflow automation)
Cloudinary (image CDN)
LibreTranslate (automatic translations)
```

---

## Architecture

### Folder Structure

```
jet-rent-a-car/
│
├── src/                          # Frontend (React)
│   ├── components/               # Reusable components
│   │   ├── Navbar.jsx
│   │   ├── Hero.jsx
│   │   ├── Marcas.jsx
│   │   ├── CarouselTexto.jsx
│   │   ├── ContactaConNosotros.jsx
│   │   └── LazySection.jsx       # Lazy loading wrapper
│   │
│   ├── pages/                    # Pages / routes
│   │   ├── homePage.jsx
│   │   ├── coche.jsx
│   │   └── renting.jsx
│   │
│   ├── services/                 # API services
│   │   ├── apiCache.js           # Cache & deduplication layer
│   │   └── translationService.js
│   │
│   ├── context/                  # Context API
│   │   ├── AppContext.jsx        # Global state
│   │   └── HeroLoadContext.jsx   # Hero load control
│   │
│   ├── hooks/                    # Custom hooks
│   │   ├── useViewportHeight.js
│   │   ├── useLanguageSync.js
│   │   └── useTranslations.js
│   │
│   └── styles/                   # CSS styles
│       ├── index.css
│       ├── hero.css
│       └── grid6puntos.css
│
├── api/                          # Backend (PHP)
│   ├── utils/                    # Shared functions
│   │   ├── db.php                # MySQL connection
│   │   ├── funciones.php         # Global utilities
│   │   ├── cache.php             # Caching system
│   │   ├── imageProcessor.php    # Image processing
│   │   └── get.php               # Prepared queries
│   │
│   ├── cron/                     # Scheduled scripts
│   │   ├── updateMarcasCache.php
│   │   ├── updateCarouselCache.php
│   │   ├── procesarImagenesCoche.php
│   │   └── traducirNuevosCampos.php
│   │
│   ├── intranet/                 # Internal endpoints (admin)
│   │   ├── addCoche.php
│   │   ├── SubirPPTX.php
│   │   └── crearBooking.php
│   │
│   ├── carouselData.php          # Carousel data
│   ├── getMarcas.php             # Brands list
│   ├── filterOptions.php         # Filter options
│   ├── guardarSolicitud.php      # Save requests
│   ├── enviarSolicitud.php       # Send requests (email)
│   ├── diagnostico-correo.php    # Email diagnostics
│   └── api_proxy.php             # Central router
│
├── public/                       # Static assets
│   ├── images/
│   └── manifest.json
│
├── scripts/
│   └── generate-pages.js         # Static page generator
│
├── vite.config.js                # Vite config
├── package.json                  # Node deps
├── composer.json                 # PHP deps
├── .env                          # Production env vars
├── .env.local                    # Development env vars
└── README.md
```

### Data Flow

```
Client (Browser)
    ↓
React SPA (apiCache.js)
    ↓
api_proxy.php (router)
    ↓
├─→ PHP Endpoint (getCoches.php, etc.)
│   ├─→ CacheManager (cache.php)
│   ├─→ PDO Database Query
│   └─→ JSON Response
│
└─→ Static asset (images, etc.)
```

### React Component Pattern

```javascript
// Example: component with lazy loading + cache
export default function Marcas() {
  const [marcas, setMarcas] = useState([]);
  const [loading, setLoading] = useState(true);
  const { t, i18n } = useTranslation();
  
  useEffect(() => {
    // Call cached service
    apiService.getMarcas()
      .then(data => {
        setMarcas(data);
        setLoading(false);
      })
      .catch(error => console.error(error));
  }, [i18n.language]);

  if (loading) {
    return <LazySection isLoading={true} />;
  }

  return (
    <LazySection isLoading={false}>
      {/* Content */}
    </LazySection>
  );
}
```

---

## Best Practices

### 1. Separation of Concerns

| Layer | Responsibility | Files |
|-------|----------------|-------|
| Presentation | UI/UX, interactions | `src/components/*.jsx` |
| Business Logic | State, rules | `src/context/`, `src/hooks/` |
| Data Access | API, cache | `src/services/apiCache.js` |
| Backend | Server-side processes | `api/*.php` |

### 2. DRY (Don't Repeat Yourself)

Shared services example:

```javascript
// src/services/apiCache.js
export const apiService = {
  async getCoches(filters = {}) {
    return cachedFetch('getCoches', { ...filters, lang: getCurrentLanguage() });
  },
  
  async getMarcas() {
    return cachedFetch('getMarcas', { lang: getCurrentLanguage() });
  }
};
```

Reusable component example:

```jsx
// components/LazySection.jsx - lazy loading wrapper
export default function LazySection({ children, isLoading, loadingText }) {
  return isLoading ? <LoadingSpinner /> : children;
}
```

### 3. Data Validation

Frontend (example uses server-side validation pattern shown here for clarity):
```javascript
// validate with filter_var (server-side PHP example)
if (!filter_var($input['email'], FILTER_VALIDATE_EMAIL)) {
  throw new Exception('Invalid email');
}
```

Backend sanitization:
```php
// sanitize strings
function sanitizeString($str) {
  return trim(preg_replace('/[<>"'"\0%\\]/', '', $str));
}
```

### 4. Error Handling

Global error handler example:

```php
// api/enviarSolicitud.php
set_error_handler(function($errno, $errstr, $errfile, $errline) {
  error_log("PHP Error: $errstr in $errfile:$errline");
  return true;
});

try {
  // logic
} catch (Exception $e) {
  http_response_code(400);
  echo json_encode(['success' => false, 'message' => $e->getMessage()]);
}
```

### 5. Transaction Control

```php
$pdo->beginTransaction();
try {
  // multiple operations
  $stmt1->execute([...]);
  $stmt2->execute([...]);
  $pdo->commit();
} catch (Exception $e) {
  $pdo->rollBack();
  throw $e;
}
```

### 6. Prepared Statements (Prevent SQL Injection)

```php
// CORRECT
$stmt = $pdo->prepare("SELECT * FROM coches WHERE ID = :id");
$stmt->execute(['id' => $id]);

// INCORRECT
$query = "SELECT * FROM coches WHERE ID = $id"; // SQL injection risk
```

### 7. Functional Components with Hooks

```jsx
export default function CochePage() {
  const [coche, setCoche] = useState(null);
  const { globalOptions } = useAppContext();
  
  useEffect(() => {
    loadCoche();
  }, [id]); // explicit dependencies
}
```

---

## Main Modules

### 1. Caching System (apiCache.js & cache.php)

Goal: reduce DB queries and improve performance.

```javascript
// Frontend deduplication
const cache = new Map();
const pendingRequests = new Map();

async function cachedFetch(endpoint, params = {}) {
  const key = generateCacheKey(endpoint, params);
  
  if (cache.has(key) && isCacheValid(key)) {
    return cache.get(key);
  }
  
  if (pendingRequests.has(key)) {
    return pendingRequests.get(key);
  }
  
  const promise = fetch(endpoint, params)
    .then(response => {
      cache.set(key, response);
      pendingRequests.delete(key);
      return response;
    });
  
  pendingRequests.set(key, promise);
  return promise;
}
```

Backend file-based cache example:

```php
// api/utils/cache.php
class CacheManager {
  public function get($endpoint, $params = []) {
    $filename = $this->getCacheFilename($endpoint, $params);
    
    if (!file_exists($filename)) return null;
    
    // Validate TTL (3600 seconds = 1 hour)
    $age = time() - filemtime($filename);
    if ($age > $this->cacheDuration) {
      unlink($filename);
      return null;
    }
    
    return json_decode(file_get_contents($filename), true);
  }
  
  public function set($endpoint, $data, $params = []) {
    $filename = $this->getCacheFilename($endpoint, $params);
    file_put_contents($filename, json_encode($data));
  }
}
```

### 2. Automated Translation System

Architecture:
```
New field in DB (ES)
    ↓
Cron job: traducirNuevosCampos.php
    ↓
LibreTranslate API (translation)
    ↓
Save into DB (EN)
    ↓
Frontend loads both languages
```

Implementation example:

```php
// api/cron/traducirNuevosCampos.php
$logs = $pdo->query(
  "SELECT * FROM TRADUCCIONES_LOG WHERE ESTADO = 'pendiente' FOR UPDATE"
)->fetchAll();

foreach ($logs as $log) {
  $originalText = obtenerTexto($log['tabla'], $log['registro_id']);
  $translatedText = traducirConIA($originalText, 'EN');
  
  actualizarEnBD($log['tabla'], $log['registro_id'], $translatedText);
  marcarComoCompletado($log['id']);
}
```

### 3. Image Processing

Pipeline:
```
Image upload
    ↓
Validate (MIME type, size)
    ↓
Detect vehicle edges (ImageMagick)
    ↓
Crop with margin
    ↓
Save to FTP
    ↓
Save reference in DB
```

Code example:

```php
// api/utils/imageProcessor.php
function procesarImagenLocal($imagenPath, $marginPercent = 5) {
  $imagick = new Imagick($imagenPath);
  
  // Detect edges
  $imagick->edgeImage(1);
  
  // Get image geometry and bounding box for the car
  $geometry = $imagick->getImageGeometry();
  $boundingBox = detectarCoche($imagick);
  
  // Apply margin
  $margin = $geometry['width'] * ($marginPercent / 100);
  $croppedBox = aplicarMargen($boundingBox, $margin);
  
  // Crop
  $imagick->cropImage(
    $croppedBox['width'], 
    $croppedBox['height'],
    $croppedBox['x'], 
    $croppedBox['y']
  );
  
  return $imagick->getImageBlob();
}
```

### 4. Central Router (api_proxy.php)

Purpose: centralize all API calls.

```php
// api/api_proxy.php
$allowed = [
  'getCoches' => __DIR__ . '/getCoches.php',
  'getMarcas' => __DIR__ . '/getMarcas.php',
  'carouselData' => __DIR__ . '/carouselData.php',
  'enviarSolicitud' => __DIR__ . '/enviarSolicitud.php',
  // ... more endpoints
];

$endpoint = $_GET['endpoint'] ?? null;

if (!isset($allowed[$endpoint])) {
  http_response_code(404);
  die(json_encode(['error' => 'Endpoint not found']));
}

if (!file_exists($allowed[$endpoint])) {
  http_response_code(500);
  die(json_encode(['error' => 'File not found']));
}

require_once $allowed[$endpoint];
```

Advantages:
- Centralized control
- Shared validation
- Unified CORS headers
- Centralized logging

---

## Optimizations

### 1. Lazy Loading

```jsx
// LazySection component
export default function LazySection({ children, isLoading, className, forceLoad = false }) {
  const [shouldLoad, setShouldLoad] = useState(forceLoad);
  const ref = useRef(null);
  
  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setShouldLoad(true);
          observer.unobserve(entry.target);
        }
      },
      { threshold: 0.1 }
    );
    
    if (ref.current) observer.observe(ref.current);
    return () => observer.disconnect();
  }, []);
  
  return (
    <div ref={ref} className={className}>
      {shouldLoad ? children : <Skeleton />}
    </div>
  );
}
```

### 2. Preload Critical Resources

```javascript
// scripts/generate-pages.js
let optimizedHtml = indexHtml.replace(
  '</head>',
  '  <link rel="preload" href="/images/coche.png" as="image">\n</head>'
);

optimizedHtml = optimizedHtml.replace(
  /<link rel="stylesheet" href="(\/assets\/[^\"]+\.css)">/g,
  '<link rel="preload" href="$1" as="style" onload="this.onload=null;this.rel=\'stylesheet\'">'
);
```

### 3. Request Deduplication

```javascript
const pendingRequests = new Map();

async function cachedFetch(endpoint, params) {
  const key = generateCacheKey(endpoint, params);
  
  if (pendingRequests.has(key)) {
    return pendingRequests.get(key);
  }
  
  const promise = fetch(`/api_proxy.php?endpoint=${endpoint}&...params`);
  pendingRequests.set(key, promise);
  
  return promise.finally(() => pendingRequests.delete(key));
}
```

### 4. Image Compression

```php
// Convert to WebP (smaller)
$imagick->setImageFormat('webp');
$imagick->setImageCompressionQuality(80);
file_put_contents($outputPath, $imagick);
```

---

## Security

### 1. CORS

```php
// Allow only specific origin
header('Access-Control-Allow-Origin: https://jet-rentacar.com');
header('Access-Control-Allow-Methods: GET, POST, OPTIONS');
header('Access-Control-Allow-Headers: Content-Type, X-API-Key');
header('Access-Control-Max-Age: 86400');
```

### 2. API Key for Sensitive Endpoints

```php
// api/sentenciaSQL.php
$apiKey = $_GET['api_key'] ?? $headers['X-API-KEY'] ?? null;
$validApiKey = getenv('SQL_API_KEY');

if ($apiKey !== $validApiKey) {
  http_response_code(401);
  die(json_encode(['error' => 'Unauthorized']));
}
```

### 3. Input Validation

```php
// Whitelist dangerous commands
$dangerousCommands = [
  'DROP TABLE', 'DELETE FROM', 'ALTER USER', 'GRANT', 'REVOKE'
];

foreach ($dangerousCommands as $cmd) {
  if (strpos(strtoupper($sql), $cmd) !== false) {
    throw new Exception("Command not allowed: $cmd");
  }
}
```

### 4. Password Hashing

```php
// Never store plain text
$hashedPassword = password_hash($input['password'], PASSWORD_BCRYPT);

// Verify
if (password_verify($inputPassword, $hashedPassword)) {
  // Correct password
}
```

### 5. HTTPS & SSL

```
SSL certificate installed
Automatic HTTP → HTTPS redirect
Security headers:
  - Strict-Transport-Security
  - X-Frame-Options: SAMEORIGIN
  - X-Content-Type-Options: nosniff
  - Content-Security-Policy
```

## Deployment

### CI/CD Flow (future)

```yaml
Push to main
  ↓
GitHub Actions
  ├─ Tests (Jest, PHPUnit)
  ├─ Linter (ESLint, PHPStan)
  ├─ Build (npm run build)
  └─ Deploy (FTP/SSH)
      ↓
      Hostinger
```

### Useful Commands

```bash
# Frontend
npm install       # Install dependencies
npm run dev       # Local development
npm run build     # Production build
npm run preview   # Preview build

# Backend
php api/setup-cache.php                          # Initialize cache
php api/cron/updateMarcasCache.php               # Update brands cache
php api/cron/procesarImagenesCoche.php?force=1  # Process images
```

---

## References

- React Hooks: https://react.dev/reference/react/hooks
- Vite Docs: https://vitejs.dev/
- PDO: https://www.php.net/manual/en/book.pdo.php
- Tailwind CSS: https://tailwindcss.com/docs
- i18next: https://www.i18next.com/
- Embla Carousel: https://www.embla-carousel.com/

---

## Author

**Alonso** - Full Stack Developer  
Email: [alonsogomezjimenez@gmail.com]  
LinkedIn: [www.linkedin.com/in/alonso-gómez-jiménez-138245114]

---

**Last updated:** January 2025
