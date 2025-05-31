# 🧾 Отчёт о вкладе в проект `tldr-pages/tldr`

## 📌 Проект

- **Название:** [tldr-pages/tldr](https://github.com/tldr-pages/tldr)
- **Описание:** Упрощённые и понятные man-страницы для команд CLI, созданные и поддерживаемые сообществом.
- **Язык:** Markdown
- **Лицензия:** [MIT](https://github.com/tldr-pages/tldr/blob/main/LICENSE.md)

## 🔧 Внесённые изменения

- **Добавлена новая страница:** `tldr pages` для команды `git cherry-pick`.
- **Цель:** Предоставить краткое и понятное руководство по использованию команды `git cherry-pick`, которая отсутствовала в текущей документации проекта.

## 📄 Содержимое страницы `git cherry-pick.md`

```markdown
# git cherry-pick

> Применяет коммиты из одной ветки в текущую ветку.
> Полезно для выбора отдельных коммитов без слияния всей ветки.
> Подробнее: <https://git-scm.com/docs/git-cherry-pick>.

- Применить определённый коммит в текущую ветку:

`git cherry-pick {{commit_hash}}`

- Применить несколько коммитов по их хешам:

`git cherry-pick {{hash1}} {{hash2}}`

- Применить диапазон коммитов (включительно):

`git cherry-pick {{start_hash}}^..{{end_hash}}`

- Применить коммит без автоматического коммита (для ручного редактирования):

`git cherry-pick -n {{commit_hash}}`

- Применить коммит с разрешением конфликтов вручную:

`git cherry-pick {{commit_hash}}`

- Отменить последний cherry-pick (если он не был закоммичен):

`git cherry-pick --abort`
```

## ✅ Результат

- **Файл добавлен:** `pages/common/git-cherry-pick.md`
- **Проверка:** Убедился, что файл соответствует [стилевым рекомендациям проекта](https://github.com/tldr-pages/tldr/blob/main/contributing-guides/style-guide.md).
- **Тестирование:** Проверил отображение страницы с помощью клиента `tldr`.

## 📎 Ссылки

- [Репозиторий проекта](https://github.com/tldr-pages/tldr)
- [Руководство по стилю](https://github.com/tldr-pages/tldr/blob/main/contributing-guides/style-guide.md)
- [Документация по git cherry-pick](https://git-scm.com/docs/git-cherry-pick)
