# IKEA Web Scraper - Prueba Técnica

🛍️ **Web scraper para IKEA España con técnicas anti-bot y manejo de paginación**

## 📋 Características

- ✅ **Sitio web objetivo**: IKEA España (e-commerce con JavaScript)
- ✅ **Interacción de usuario**: Scroll, paginación automática, botón "Cargar más"
- ✅ **Técnicas anti-bot**: Evasión de detección, manejo de cookies, timing humano
- ✅ **Extracción de datos**: 10+ productos con campos completos
- ✅ **Manejo de pseudoelementos**: Botones en `::after` y `::before`
- ✅ **Salida JSON estructurada**: Según especificaciones de prueba técnica
- ✅ **Screenshots automáticos**: Durante ejecución para debugging

## 🎯 Campos Extraídos por Producto

```json
{
  "id": "product_id",
  "title": "Nombre del producto", 
  "price": "29,99€",
  "imageUrl": "https://...",
  "productUrl": "https://...",
  "rating": "4.5/5",
  "availability": "Disponible",
  "details": {
    "type": "Tipo de producto",
    "measurements": "Medidas", 
    "category": "Accesorios de exterior"
  }
}
```

## 🛡️ Medidas Anti-Bot Implementadas

### 1. **Evasión de Detección de WebDriver**
```javascript
// Eliminar propiedades que delatan automatización
Object.defineProperty(navigator, 'webdriver', {
    get: () => undefined,
});
```

### 2. **User Agent Realista**
```javascript
await page.setUserAgent('Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36...');
```

### 3. **Timing Humano**
```javascript
slowMo: 500,  // Retraso entre acciones
await new Promise(resolve => setTimeout(resolve, randomDelay));
```

### 4. **Manejo de Cookies Automático**
```javascript
// Detecta y acepta automáticamente banners de cookies
const cookieSelectors = ['#onetrust-reject-all-handler', '#accept-choices', ...];
```

### 5. **Manejo de Pseudoelementos**
```javascript
// Detecta botones en ::after y ::before
const afterStyles = window.getComputedStyle(element, '::after');
const afterContent = afterStyles.content;
```

## 🚀 Instalación y Uso

### Prerrequisitos
```bash
node --version  # v14.0.0 o superior
npm --version   # 6.0.0 o superior
```

### Instalación
```bash
git clone https://github.com/PeterManga/ScrapTest.git
cd ScrapTest
npm install
```

### Ejecución
```bash
# Ejecutar scraper principal
node ikeaScrap.js

# Salida esperada:
# - Archivo JSON en ./output/
# - Screenshots en ./output/ 
# - Logging detallado en consola
```

## 📊 Estructura de Salida JSON

```json
{
  "metadata": {
    "scrapingSession": {
      "startTime": "2025-06-06T10:30:00Z",
      "endTime": "2025-06-06T10:35:00Z", 
      "duration": 300000,
      "targetUrl": "https://www.ikea.com/es/es/cat/accesorios-exterior-34203/",
      "totalProducts": 45,
      "method": "Pagination with load more detection"
    },
    "antiBotMeasures": {
      "humanLikeBehavior": true,
      "cookieHandling": true,
      "webdriverEvasion": true
    },
    "dataQuality": {
      "productsWithPrice": 42,
      "productsWithImages": 45,
      "productsWithRating": 38,
      "productsWithUrl": 45
    }
  },
  "products": [
    // Array de productos extraídos
  ]
}
```

## 🔧 Configuración

```javascript
const CONFIG = {
    targetUrl: 'https://www.ikea.com/es/es/cat/accesorios-exterior-34203/',
    minProducts: 10,
    maxLoadMoreAttempts: 20,
    outputDir: './output'
};
```

## 🧪 Funcionalidades de Prueba Técnica

### ✅ Requisitos Cumplidos

1. **Sitio web con JavaScript**: IKEA e-commerce con contenido dinámico
2. **Interacción de usuario**: Paginación automática, scroll, botón "Cargar más"  
3. **Medidas anti-bot**: Múltiples técnicas implementadas
4. **Extracción de datos**: 10+ productos con campos completos
5. **Salida JSON**: Estructura completa según especificaciones

### 🔍 Técnicas Avanzadas

- **Detección de pseudoelementos** `::after` y `::before`
- **Múltiples estrategias de clic** para elementos complejos
- **Fallback con scroll** para lazy loading
- **Verificación de contenido nuevo** después de cada acción
- **Logging detallado** para debugging y monitoring

## 📸 Screenshots

El scraper toma automáticamente screenshots en puntos clave:
- `01_initial_load.png` - Carga inicial
- `02_after_pagination.png` - Después de paginación
- `03_final.png` - Estado final

## ❌ Manejo de Errores

```javascript
// Múltiples estrategias de clic para elementos problemáticos
const clickStrategies = [
    async () => await page.click(selector),                    // Clic normal
    async () => await page.evaluate(sel => document.querySelector(sel).click(), selector), // JS click
    async () => await page.mouse.click(x, y),                 // Coordenadas específicas
    async () => element.dispatchEvent(new MouseEvent('click')) // Dispatch event
];
```

## 📈 Resultados Típicos

- **Productos extraídos**: 30-50 productos
- **Tiempo de ejecución**: 2-5 minutos
- **Tasa de éxito**: 95%+ en condiciones normales
- **Calidad de datos**: 90%+ de productos con campos completos

## 🔮 CAPTCHA Handling (Conceptual)

```javascript
// Enfoque para resolución de CAPTCHAs (no implementado en demo)
/*
const captchaSolver = new TwoCaptchaAPI('API_KEY');
const solution = await captchaSolver.solveRecaptcha({
    googlekey: siteKey,
    pageurl: page.url()
});
await page.evaluate(token => {
    document.querySelector('#g-recaptcha-response').value = token;
}, solution.code);
*/
```

## 🐛 Debugging

```javascript
// Activar logging detallado
DEBUG=true node ikeaScrap.js

// Ejecutar en modo no-headless para ver proceso
headless: false  // en browser launch options
```

## 📦 Dependencias

```json
{
  "dependencies": {
    "puppeteer": "^21.0.0"
  }
}
```

## 🤝 Contribuciones

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para detalles.

## 🔗 Enlaces

- [Repositorio GitHub](https://github.com/PeterManga/ScrapTest)
- [IKEA España](https://www.ikea.com/es/es/)
- [Puppeteer Documentation](https://pptr.dev/)

---

**Desarrollado por Pedro Manga** - Prueba Técnica de Web Scraping
