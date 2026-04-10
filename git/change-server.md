---
title: How to change servers (move) or use two at the same time
description: 
published: true
date: 2026-04-10T10:31:22.722Z
tags: 
editor: markdown
dateCreated: 2026-04-10T10:31:22.722Z
---


Допустим, при вводе `git remote -v` ты видишь, что твой репозиторий привязан к GitLab:
```text
origin  https://gitlab.com/username/obsidian-vault.git (fetch)
origin  https://gitlab.com/username/obsidian-vault.git (push)
```

У тебя есть два пути: полностью переехать на новый сервер (например, GitHub) или оставить оба для надежности. **В обоих случаях сначала создай абсолютно пустой репозиторий на новом сервере** (без файлов README или .gitignore).

#### Вариант А: Полный переезд (с GitLab на GitHub)
Используй этот вариант, если GitLab тебе больше не нужен, и ты хочешь просто заменить адрес.

**1. Измени адрес у локального `origin`:**
Вместо добавления нового репозитория, мы просто меняем ссылку у существующего:
```bash
git remote set-url origin https://github.com/username/obsidian-vault.git
```
**2. Проверь, что адрес изменился:**
```bash
git remote -v
```
*(Теперь в списке должен быть только GitHub).*

**3. Отправь все данные на новый сервер:**
```bash
git push -u origin --all   # отправляет все ветки
git push -u origin --tags  # отправляет все теги (версии), если есть
```
🎉 **Готово!** Проект переехал, старый репозиторий на GitLab больше не связан с твоей локальной папкой.

---

#### Вариант Б: Использовать оба сервера одновременно
Используй этот вариант, если хочешь делать резервные копии сразу в два места. Оставляем `origin` смотреть на GitLab, а GitHub добавляем как второй сервер.

**1. Добавь второй удаленный репозиторий под новым именем (например, `github`):**
```bash
git remote add github https://github.com/username/obsidian-vault.git
```

**2. Проверь привязки:**
```bash
git remote -v
```
Теперь у тебя будет 4 строки: две для `origin` (GitLab) и две для `github` (GitHub).

**3. Как теперь отправлять код:**
*   Чтобы отправить на GitLab: `git push origin main`
*   Чтобы отправить на GitHub: `git push github main`

🔥 **Продвинутый трюк: как отправлять код на ОБА сервера одной командой**
Если тебе лень каждый раз делать два пуша, ты можешь настроить свой `origin` так, чтобы он отправлял изменения по нескольким адресам одновременно при обычном вводе `git push`.

Для этого нужно добавить оба адреса специально для отправки (`push`):
```bash
git remote set-url --add --push origin https://gitlab.com/username/obsidian-vault.git
git remote set-url --add --push origin https://github.com/username/obsidian-vault.git
```
Теперь, когда ты введешь просто `git push`, Git автоматически отправит твой код и в GitLab, и в GitHub!