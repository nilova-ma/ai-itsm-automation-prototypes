# ai-itsm-automation-prototypes
Collection of AI-powered ITSM automation prototypes built with n8n, OpenAI, and Google Sheets. Features PII masking, dynamic routing, and conversational triage. 

## 📂 Структура репозитория
- `ai-ticket-router-with-pii-masking.json` — основной workflow маршрутизации тикетов с предварительной анонимизацией данных.

## 🚀 Кейс: AI-маршрутизация с PII-маскированием

### Проблема
Ручная классификация тикетов инженерами L1 занимала 2–3 минуты и давала ~30% ошибок маршрутизации (misrouting). 

### Решение
Workflow в n8n, который:
1. Перехватывает новый запрос (имитация входящего тикета через Google Sheets).
2. **Маскирует PII:** заменяет email и телефоны на токены `[EMAIL]`, `[PHONE]` с помощью Regex до отправки в ИИ.
3. **Классифицирует:** отправляет очищенный текст в OpenAI (GPT-4o-mini) для определения Категории, Приоритета и Команды.
4. **Обновляет данные:** записывает результат обратно в систему со статусом "AI Маршрутизирован".

### 🏗️ Архитектура Workflow

<img width="1422" height="412" alt="ai-ticket-router-with-pii-masking" src="https://github.com/user-attachments/assets/2d85abd9-e508-4bc0-804a-d750b1ca3147" />
<img width="1804" height="338" alt="ai-ticket-router-with-pii-masking_table" src="https://github.com/user-attachments/assets/e3c8417f-e299-4f73-a110-2daf166e0162" />



### 🛡️ Безопасность (Enterprise Ready)
- **PII Redaction:** Регулярные выражения очищают данные на входе, предотвращая утечку чувствительной информации.
- **Zero Data Retention:** В production-версии предполагается использование корпоративных LLM (YandexGPT / GigaChat On-Premise) или Azure OpenAI с contractual guarantee отсутствия сохранения данных.
- **Human-in-the-Loop:** ИИ только предлагает метаданные, финальное действие и ответственность остаются за оператором.

### 🛠️ Стек технологий
- **Orchestration:** n8n
- **AI:** OpenAI API (GPT-4o-mini) / Groq (Llama 3)
- **Data:** Google Sheets API (как легковесная замена Jira для быстрого прототипирования)
- **Logic:** JavaScript (Code nodes), Regex

### 📥 Как развернуть и использовать
1. Импортируйте файл `.json` в ваш инстанс n8n (через меню импорта или Ctrl+I).
2. Настройте Credentials для Google Sheets и OpenAI/Groq.
3. Создайте таблицу с колонками: `ID`, `Описание`, `Категория`, `Приоритет`, `Команда`, `Статус`, `Очищенное_описание`.
4. Активируйте workflow и добавьте новую строку в таблицу для теста.

---
**Автор:** Нилова Мария 
**Связаться:** telegram: @nilova_ma 
**Портфолио:** 
