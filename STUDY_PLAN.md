# 📚 План глубокого изучения Nuxt UI v4

## 🎯 Обзор проекта

**Nuxt UI** — это библиотека UI-компонентов для Vue 3 и Nuxt, построенная на:
- **Reka UI** (headless компоненты с доступностью)
- **Tailwind CSS v4** (стилизация)  
- **Tailwind Variants** (управление вариантами стилей)

---

## 📋 Этап 1: Фундамент (1-2 недели)

### 1.1 Изучение зависимостей

| Технология | Что изучить | Ресурсы |
|------------|------------|---------|
| **Vue 3 Composition API** | `ref`, `reactive`, `computed`, `watch`, `inject/provide` | [Vue Docs](https://vuejs.org) |
| **Reka UI** | Headless компоненты, доступность (a11y) | [Reka UI Docs](https://reka-ui.com) |
| **Tailwind CSS v4** | Утилитарные классы, кастомизация | [Tailwind Docs](https://tailwindcss.com) |
| **Tailwind Variants** | `tv()`, слоты, варианты, `compoundVariants` | [Tailwind Variants](https://www.tailwind-variants.org) |

### 1.2 Настройка окружения

```bash
# Клонирование и установка
pnpm install

# Запуск playground для экспериментов
cd playgrounds/nuxt && pnpm dev

# Запуск документации
cd docs && pnpm dev
```

---

## 📋 Этап 2: Архитектура проекта (1 неделя)

### 2.1 Структура директорий

| Директория | Назначение |
|------------|------------|
| `src/module.ts` | Точка входа Nuxt-модуля |
| `src/vite.ts` / `src/unplugin.ts` | Vite плагин для Vue без Nuxt |
| `src/runtime/components/` | ~110+ Vue компонентов |
| `src/runtime/composables/` | Composables (`useToast`, `useOverlay`, etc.) |
| `src/theme/` | Конфигурация стилей для каждого компонента |
| `src/plugins/` | Внутренние плагины сборки |
| `src/utils/` | Утилиты |

### 2.2 Ключевые файлы для изучения

1. **`src/module.ts`** — как регистрируется Nuxt-модуль
2. **`src/unplugin.ts`** — unplugin архитектура для универсальности
3. **`src/runtime/utils/tv.ts`** — обёртка над `tailwind-variants`

---

## 📋 Этап 3: Система тем (1-2 недели)

### 3.1 Анатомия темы компонента

Изучите `src/theme/button.ts`:

```typescript
export default (options) => ({
  slots: {           // Именованные части компонента
    base: '...',
    label: '...',
    leadingIcon: '...'
  },
  variants: {        // Варианты стилей
    color: { primary: '', secondary: '' },
    variant: { solid: '', outline: '', ghost: '' },
    size: { xs: {}, sm: {}, md: {}, lg: {} }
  },
  compoundVariants: [ // Комбинированные варианты
    { color: 'primary', variant: 'solid', class: '...' }
  ],
  defaultVariants: { color: 'primary', size: 'md' }
})
```

### 3.2 Задания

1. Изучите 5-10 тем в `src/theme/`
2. Поймите паттерн `slots` + `variants` + `compoundVariants`
3. Создайте свою кастомную тему для Button

---

## 📋 Этап 4: Компоненты (2-3 недели)

### 4.1 Категории компонентов

| Категория | Компоненты | Сложность |
|-----------|------------|-----------|
| **Базовые** | Button, Badge, Icon, Link, Avatar | ⭐ |
| **Формы** | Input, Select, Checkbox, RadioGroup, Form | ⭐⭐ |
| **Overlays** | Modal, Drawer, Popover, Tooltip, ContextMenu | ⭐⭐⭐ |
| **Data Display** | Table, Tree, Accordion, Tabs | ⭐⭐⭐ |
| **Layout** | Container, Card, Separator, Main | ⭐ |
| **Dashboard** | DashboardSidebar, DashboardNavbar, DashboardPanel | ⭐⭐⭐ |
| **AI/Chat** | ChatMessage, ChatPrompt, ChatPalette | ⭐⭐ |

### 4.2 План изучения компонента

Для каждого компонента:

1. **Тема**: `src/theme/{component}.ts`
2. **Компонент**: `src/runtime/components/{Component}.vue`
3. **Документация**: `docs/content/docs/2.components/{component}.md`
4. **Тесты**: `test/components/{Component}.spec.ts`

### 4.3 Детальный разбор Button

```
Button.vue
├── Props: color, variant, size, loading, disabled, icon
├── Slots: leading, default, trailing
├── Composables: useComponentIcons, useFieldGroup
├── Integration: ULink, UIcon, UAvatar
└── Theme: tv() для генерации классов
```

---

## 📋 Этап 5: Composables (1 неделя)

### 5.1 Ключевые composables

| Composable | Назначение | Файл |
|------------|------------|------|
| `useToast` | Управление уведомлениями | `src/runtime/composables/useToast.ts` |
| `useOverlay` | Программное создание модалок | `src/runtime/composables/useOverlay.ts` |
| `useFormField` | Интеграция с формами | `src/runtime/composables/useFormField.ts` |
| `useShortcuts` | Клавиатурные сокращения | `src/runtime/composables/defineShortcuts.ts` |
| `useLocale` | Интернационализация | `src/runtime/composables/useLocale.ts` |

### 5.2 Паттерны

- `inject/provide` для контекста компонентов
- `useState` (Nuxt) для глобального состояния
- `createSharedComposable` (@vueuse/core) для синглтонов

---

## 📋 Этап 6: Система сборки (1 неделя)

### 6.1 Unplugin архитектура

```
src/unplugin.ts
├── TemplatePlugin     — генерация тем
├── ComponentImportPlugin — auto-import компонентов
├── AutoImportPlugin   — auto-import composables
├── AppConfigPlugin    — app.config.ts интеграция
└── NuxtEnvironmentPlugin — совместимость Nuxt/Vue
```

### 6.2 Задания

1. Изучите как работает tree-shaking компонентов
2. Поймите генерацию `#build/ui/*.ts` файлов
3. Исследуйте интеграцию с Tailwind CSS v4

---

## 📋 Этап 7: Тестирование (1 неделя)

### 7.1 Структура тестов

```
test/
├── components/     — тесты компонентов
├── composables/    — тесты composables
├── utils/          — тесты утилит
└── component-render.ts — хелперы рендеринга
```

### 7.2 Запуск тестов

```bash
pnpm test           # все тесты
pnpm test:watch     # watch режим
```

---

## 📋 Этап 8: Продвинутые темы (2+ недели)

### 8.1 Кастомизация

1. **App Config** — переопределение тем через `app.config.ts`
2. **CSS переменные** — цветовая система (`--ui-primary`, etc.)
3. **Создание своих компонентов** на базе примитивов

### 8.2 Интеграции

- **@nuxt/content** — prose компоненты
- **@nuxtjs/color-mode** — тёмная тема
- **Inertia.js** — роутинг
- **Vee-validate/Zod** — валидация форм

### 8.3 Контрибьюция

1. Изучите `docs/content/docs/1.getting-started/4.contribution.md`
2. Найдите issue на GitHub
3. Создайте PR с улучшением

---

## 📅 Рекомендуемый таймлайн

| Неделя | Этап | Цель |
|--------|------|------|
| 1-2 | Фундамент | Освоить зависимости |
| 3 | Архитектура | Понять структуру проекта |
| 4-5 | Темы | Освоить систему стилей |
| 6-8 | Компоненты | Изучить 20+ компонентов |
| 9 | Composables | Освоить все composables |
| 10 | Сборка | Понять unplugin систему |
| 11 | Тесты | Научиться писать тесты |
| 12+ | Продвинутое | Контрибьюция, кастомизация |

---

## 🛠 Практические задания

### Уровень 1: Новичок
- [ ] Создать страницу с 10 разными вариантами Button
- [ ] Реализовать форму входа с Form, Input, Checkbox
- [ ] Добавить Toast уведомления

### Уровень 2: Средний
- [ ] Создать Dashboard layout с Sidebar и Navbar
- [ ] Реализовать таблицу с сортировкой и пагинацией
- [ ] Добавить CommandPalette с поиском

### Уровень 3: Продвинутый
- [ ] Создать свой компонент с полной интеграцией тем
- [ ] Написать тесты для компонента
- [ ] Сделать PR в репозиторий

---

## 📖 Полезные ресурсы

- **Документация**: https://ui.nuxt.com
- **GitHub**: https://github.com/nuxt/ui
- **Discord**: https://discord.com/invite/nuxt
- **Reka UI**: https://reka-ui.com
- **Tailwind Variants**: https://www.tailwind-variants.org
