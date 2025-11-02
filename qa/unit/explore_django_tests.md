Проанализируй существующие тесты в Django проекте:

    @Folder tests/
    @Folder */tests/
    @Folder */test_*.py
    @Folder conftest.py
    @File pytest.ini
    @File setup.cfg 
    @File pyproject.toml

Создай детальный отчёт по следующей структуре:

### 1. ТЕХНИЧЕСКИЙ СТЕК ТЕСТИРОВАНИЯ:

**Основные инструменты:**
- Версия pytest и установленные плагины
- pytest-django и его конфигурация
- Дополнительные pytest плагины (pytest-cov, pytest-mock, pytest-xdist и т.д.)
- Используемые фабрики (factory_boy, model_bakery, faker)
- Библиотеки для HTTP тестирования (pytest-django client, requests-mock, responses)

**Конфигурация pytest:**
- Настройки в pytest.ini / setup.cfg / pyproject.toml
- DJANGO_SETTINGS_MODULE
- Markers (@pytest.mark.django_db, @pytest.mark.parametrize и т.д.)
- Опции командной строки по умолчанию

### 2. АРХИТЕКТУРНЫЕ ПАТТЕРНЫ:

**Структура тестовых файлов:**
- Где располагаются тесты (отдельная папка tests/ или рядом с кодом?)
- Соглашения по именованию файлов (test_*.py или *_test.py?)
- Организация по модулям/приложениям
- Использование __init__.py в тестовых директориях

**Fixtures и фабрики:**
- Как определяются fixtures (в conftest.py или в тестах?)
- Scope fixtures (function, class, module, session)
- Использование @pytest.fixture
- Паттерны создания тестовых данных (factories, fixtures, model instances)
- Примеры фабрик если используется factory_boy или model_bakery

**Работа с базой данных:**
- Использование @pytest.mark.django_db
- Транзакционные vs нетранзакционные тесты
- Fixtures для настройки БД (db, transactional_db)
- Паттерны очистки/подготовки данных

**Тестирование API/Views:**
- Использование Django TestClient или APIClient (DRF)
- Паттерны аутентификации в тестах
- Проверка статус-кодов и содержимого ответов
- Использование reverse() для URL

**Моки и патчинг:**
- Использование pytest-mock / unittest.mock
- Паттерны мокирования внешних сервисов
- Мокирование Django signals, tasks, email

### 3. КОНВЕНЦИИ КОДА:

**Стиль именования:**

    # Примеры из существующих тестов:
    # - Формат test functions
    # - Формат test classes
    # - Формат fixtures
    # - Формат параметризованных тестов

Структура тестов:

    Используется ли паттерн Arrange-Act-Assert?
    Как организованы docstrings?
    Стиль комментариев

Imports:

    Порядок импортов
    Относительные vs абсолютные импорты
    Группировка импортов

### 4. ПРИМЕРЫ ПАТТЕРНОВ:

Приведи 2-3 реальных примера из существующих тестов с объяснением:

    Простой тест модели
    Тест view/API endpoint
    Тест с fixtures
    Параметризованный тест (если есть)

Сохрани этот анализ в файл в md-формате arc_tests.md - он будет использован на следующих шагах.
