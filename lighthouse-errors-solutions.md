# Типовые ошибки Lighthouse и способы их исправления

## Performance (Производительность)

### ❌ Проблема: "Eliminate render-blocking resources"
**Описание**: CSS и JavaScript блокируют отрисовку страницы

**Решения**:
```html
<!-- Плохо -->
<link rel="stylesheet" href="styles.css">

<!-- Хорошо -->
<link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="styles.css"></noscript>

<!-- Для некритичных стилей -->
<link rel="stylesheet" href="styles.css" media="print" onload="this.media='all'">
```

### ❌ Проблема: "Properly size images"
**Описание**: Изображения слишком большие для контейнера

**Решения**:
```html
<!-- Плохо -->
<img src="large-image.jpg" width="300">

<!-- Хорошо -->
<img src="image-300.jpg" 
     srcset="image-300.jpg 300w, image-600.jpg 600w, image-900.jpg 900w"
     sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
     alt="Описание">
```

### ❌ Проблема: "Serve images in next-gen formats"
**Решение**:
```html
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Описание">
</picture>
```

### ❌ Проблема: "Defer offscreen images"
**Решение**:
```html
<img src="image.jpg" loading="lazy" alt="Описание">
```

### ❌ Проблема: "Minify CSS/JavaScript"
**Решения**:
- Использовать инструменты сжатия (Terser, cssnano)
- Удалить неиспользуемый код
- Включить gzip/brotli сжатие на сервере

### ❌ Проблема: "Reduce unused CSS"
**Решения**:
- Использовать PurgeCSS
- Загружать CSS по требованию
- Разделить CSS на критический и некритический

## Accessibility (Доступность)

### ❌ Проблема: "Image elements do not have [alt] attributes"
**Решения**:
```html
<!-- Плохо -->
<img src="photo.jpg">

<!-- Хорошо -->
<img src="photo.jpg" alt="Описание изображения">

<!-- Декоративные изображения -->
<img src="decoration.jpg" alt="" role="presentation">
<img src="icon.svg" alt="" aria-hidden="true">
```

### ❌ Проблема: "Background and foreground colors do not have sufficient contrast ratio"
**Решения**:
```css
/* Плохо - контраст 2.1:1 */
.text {
    color: #666;
    background: #ccc;
}

/* Хорошо - контраст 4.5:1+ */
.text {
    color: #333;
    background: #fff;
}

/* Проверить контраст можно на https://contrast-ratio.com */
```

### ❌ Проблема: "Form elements do not have associated labels"
**Решения**:
```html
<!-- Плохо -->
<input type="text" placeholder="Имя">

<!-- Хорошо -->
<label for="name">Имя *</label>
<input type="text" id="name" required aria-required="true">

<!-- Альтернативно -->
<label>
    Имя *
    <input type="text" required aria-required="true">
</label>
```

### ❌ Проблема: "Links do not have a discernible name"
**Решения**:
```html
<!-- Плохо -->
<a href="/more">Читать далее</a>

<!-- Хорошо -->
<a href="/article-1">Читать далее: "Название статьи"</a>
<a href="/more" aria-label="Читать далее о веб-доступности">Читать далее</a>
```

### ❌ Проблема: "Heading elements are not in a sequentially-descending order"
**Решения**:
```html
<!-- Плохо -->
<h1>Главный заголовок</h1>
<h3>Подзаголовок</h3>

<!-- Хорошо -->
<h1>Главный заголовок</h1>
<h2>Подзаголовок</h2>
<h3>Подподзаголовок</h3>
```

### ❌ Проблема: "Interactive controls are not keyboard focusable"
**Решения**:
```html
<!-- Плохо -->
<div onclick="doSomething()">Кликабельный элемент</div>

<!-- Хорошо -->
<button onclick="doSomething()">Кликабельный элемент</button>

<!-- Или для div -->
<div role="button" tabindex="0" onclick="doSomething()" 
     onkeydown="handleKeyDown(event)">Кликабельный элемент</div>
```

## Best Practices (Лучшие практики)

### ❌ Проблема: "Does not use HTTPS"
**Решение**: Настроить SSL сертификат на сервере

### ❌ Проблема: "Links to cross-origin destinations are unsafe"
**Решения**:
```html
<!-- Плохо -->
<a href="https://external-site.com" target="_blank">Внешняя ссылка</a>

<!-- Хорошо -->
<a href="https://external-site.com" target="_blank" rel="noopener noreferrer">
    Внешняя ссылка
</a>
```

### ❌ Проблема: "Displays images with incorrect aspect ratio"
**Решения**:
```css
/* Использовать object-fit */
.image {
    width: 300px;
    height: 200px;
    object-fit: cover; /* или contain */
}

/* Или aspect-ratio */
.image-container {
    aspect-ratio: 16 / 9;
}
```

### ❌ Проблема: "Browser errors were logged to the console"
**Решения**:
- Исправить JavaScript ошибки
- Проверить валидность HTML
- Убедиться что все ресурсы загружаются корректно

## SEO

### ❌ Проблема: "Document does not have a meta description"
**Решение**:
```html
<meta name="description" content="Краткое описание страницы (150-160 символов)">
```

### ❌ Проблема: "Document doesn't have a valid hreflang"
**Решения**:
```html
<!-- Для многоязычных сайтов -->
<link rel="alternate" hreflang="en" href="https://example.com/en/">
<link rel="alternate" hreflang="ru" href="https://example.com/ru/">
<link rel="alternate" hreflang="x-default" href="https://example.com/">
```

### ❌ Проблема: "Image elements do not have [alt] attributes"
**Решение**: См. раздел Accessibility выше

### ❌ Проблема: "Links are not crawlable"
**Решения**:
```html
<!-- Плохо -->
<span onclick="navigate()">Ссылка</span>

<!-- Хорошо -->
<a href="/page">Ссылка</a>

<!-- Для SPA -->
<a href="/page" onclick="navigate(); return false;">Ссылка</a>
```

### ❌ Проблема: "Tap targets are not sized appropriately"
**Решения**:
```css
/* Минимальный размер 44x44px для touch устройств */
.button, .link {
    min-height: 44px;
    min-width: 44px;
    padding: 12px;
}

/* Или использовать псевдо-элементы */
.small-link::after {
    content: '';
    position: absolute;
    top: -12px;
    right: -12px;
    bottom: -12px;
    left: -12px;
}
```

## Инструменты для проверки и исправления

### Автоматизация проверки:
```bash
# Lighthouse CI
npm install -g @lhci/cli
lhci autorun

# Для проверки локально
lighthouse https://example.com --output html --output-path ./report.html
```

### Онлайн инструменты:
- **PageSpeed Insights** - https://pagespeed.web.dev/
- **Web.dev Measure** - https://web.dev/measure/
- **GTmetrix** - https://gtmetrix.com/
- **WebPageTest** - https://www.webpagetest.org/

### Плагины для VS Code:
- **Lighthouse** - встроенный аудит
- **axe DevTools** - проверка доступности
- **Web Accessibility** - подсказки по a11y

### CSS инструменты:
- **PurgeCSS** - удаление неиспользуемого CSS
- **Critical** - выделение критического CSS
- **cssnano** - минификация CSS

### Изображения:
- **ImageOptim** - сжатие изображений
- **Squoosh** - https://squoosh.app/
- **TinyPNG** - https://tinypng.com/

## Контрольный список оптимизации

### Performance:
- [ ] Сжаты и оптимизированы изображения
- [ ] Используется lazy loading
- [ ] Минифицированы CSS/JS файлы
- [ ] Настроено кэширование
- [ ] Удален неиспользуемый код
- [ ] Используются современные форматы изображений

### Accessibility:
- [ ] Все изображения имеют alt атрибуты
- [ ] Контрастность текста соответствует WCAG
- [ ] Формы имеют связанные labels
- [ ] Навигация работает с клавиатуры
- [ ] Правильная иерархия заголовков
- [ ] Добавлены ARIA атрибуты где необходимо

### SEO:
- [ ] Добавлены meta теги (title, description)
- [ ] Используется семантический HTML
- [ ] Правильная структура заголовков
- [ ] Ссылки имеют описательный текст
- [ ] Изображения имеют alt атрибуты
- [ ] Настроен robots.txt и sitemap.xml

### Best Practices:
- [ ] Используется HTTPS
- [ ] Внешние ссылки имеют rel="noopener"
- [ ] Нет ошибок в консоли
- [ ] Корректные пропорции изображений
- [ ] Безопасность форм