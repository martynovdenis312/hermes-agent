# Qwen native provider session mapping

## Problem this ultragoal solves

Qwen через `qwen-local` умеет и контекст, и tool calls, но Hermes подключает его как обычный OpenAI-compatible endpoint без стабильной привязки Hermes session/Telegram thread к Qwen chatId/parentId. Из-за этого Qwen может открывать новый чат и терять контекст.

## Scope

Добавить минимальный, поддерживаемый путь для Qwen-web/Qwen-local в существующий Hermes Agent, без нового репозитория и без shell-костылей.

## Constraints

- Не сохранять секреты в коде, логах, ultragoal или тестах.
- Main agent пишет код; субагенты помогают поиском и ревью.
- DeepSeek/`ds_scout` использовать только для read-only поиска, чтобы экономить основное контекстное окно.
- Сохранение состояния Qwen должно быть profile-safe через Hermes home, а не через текущую директорию.
- Tools должны идти через обычный Hermes tool loop и OpenAI-compatible `tool_calls`.

## Acceptance summary

- Есть стабильный ключ сессии для Qwen provider.
- Qwen `chatId`/`parentId` сохраняются и переиспользуются между ходами одной Hermes session.
- Обычные custom/OpenAI-compatible провайдеры не меняют поведение.
- Есть тесты на маппинг и отсутствие регрессии.
- Живой smoke на `qwen-local`: context + tool_calls.
- Независимое ревью не находит blocker.
