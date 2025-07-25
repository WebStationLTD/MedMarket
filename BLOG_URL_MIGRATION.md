# Blog URL Migration Documentation

## Промени

### Преди

- Блог постове: `/blog/example-post`
- Блог index: `/blog`

### След

- Блог постове: `/example-post` (директно в root)
- Блог index: `/blog` (остава същото)

## Направени промени

### 1. Създаден нов `[slug].vue` файл

- Локация: `woonuxt_base/app/pages/[slug].vue`
- Обработва блог постове директно от root URL
- Включва резервиран списък от страници за избягване на конфликти

### 2. Премахнати стари файлове

- `woonuxt_base/app/pages/blog/[slug].vue`
- `pages/blog/[slug].vue`

### 3. Обновени линкове

- `BlogPostCard.vue` компонентите вече сочат към `/${post.slug}`
- Blog index страницата генерира правилните линкове

### 4. SEO оптимизации

- Canonical URLs сочат към новата структура
- Schema.org данни се запазват
- Yoast SEO интеграция работи

### 5. Config промени

- Добавено ISR кеширане за блог постове
- Обновени prerender routes

## Резервирани страници

Следните slug-ове НЕ ще се обработват като блог постове:

- categories, magazin, contact, thank-you
- order-summary, wishlist, privacy-policy
- checkout, products, produkt, produkt-kategoriya
- blog, my-account, oauth, order-pay, porachka-2

## Тестване

1. Проверете, че блог постовете се зареждат на `/your-post-slug`
2. Проверете, че резервираните страници работят нормално
3. Проверете, че 404 грешки се показват за несъществуващи постове
