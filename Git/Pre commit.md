Pre-commit позволяет запускать различные git хуки перед коммитом или пушем в Git репозиторий - один раз настроив, можно сэкономить много времени на запуск линтеров и анализаторов кода, ожидания проверок от CI системы, в некоторых случаях.
Хуков великое множество для разных языков программирования и задач из которых можно собрать практически любой нужный вокрфлоу

1. Установить фреймворк можно через пакетный менеджер [pip](https://pre-commit.com/#install) или [homebrew](https://brew.sh/) для Mac OS
2. После установки в корень нашего проекта необходимо добавить файл `.pre-commit-config.yaml` . Базовый конфиг так же можно [сгенерировать](https://pre-commit.com/#pre-commit-sample-config) командой `pre-commit sample-config`
3. Далее можно подключать интересующие нас плагины в файл конфигурации (`.pre-commit-config.yaml`) . Доступные плагины можно найти и выбрать на [этой странице](https://pre-commit.com/hooks.html)
4. После добавления плагинов нужно выполнить `pre-commit install`

Для нашего текущего проекта рекомендую использовать следующие хуки

1. [Gitlab CI Linter](https://gitlab.com/devopshq/gitlab-ci-linter) - один из нескольких линтеров для Gitlab-CI
2. [Стандартные хуки](https://github.com/pre-commit/pre-commit-hooks) для проверки YAML

А вот пример базового pre-commit конфига для нашего проекта в Gitlab CI.

```
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.1.0
    hooks:
    -   id: check-yaml
    -   id: check-added-large-files
	
-   repo: https://gitlab.com/devopshq/gitlab-ci-linter
    rev: v1.0.2
    hooks:
    -   id: gitlab-ci-linter
        args:
        - '--server'
        - 'https://gitlab.praktikum-services.ru'
        - '--project'
        - 'erakhmetzyanov/sausage-store'
```

В дальнейшем наш репозиторий будет пополняться новым кодом для различных инструментов, под которые так же есть хуки, линтеры, анализаторы