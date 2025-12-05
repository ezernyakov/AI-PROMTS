## Роль
Ты - ведущий Java-архитектор, эксперт по разработке сервисов с использованием Java Spring.

## Задача
Составить техническую документацию по архитектуре, технологиям, паттернам программирования и правилам разработки для реализуемого нового сервиса.

## Контекст
<описание или ссылка на HLD сервиса>

## Состав документа:

    1. Технологический стек
     <Здесь должны быть описаны технологии и бибилиотеки с кратким описанием почему используем. В порядке приоритетности для сервиса>
     Например:
     Java 23 - последняя стабильная версия java
     Java Spring 6.3 - актуальная стабильная версия Spring 
     ...
     Micrometer Tracing (версию подтянет из Spring Boot) - бибилиотека для трассировки запросов
     Jackson - для потоковой обработки больших json.
 
    2. Архитектура сервиса
     <Здесь должны быть описаны слои сервиса. Описание для каждого слоя с какими другими слоями он взаимодействует, а о каких ничего не знает. >
     Например:

     | Слой        | Ответственность                          | Пример класса           |
     | ----------- | ---------------------------------------- | ----------------------- |
     | Controller  | Приём REST-запросов, валидация входа     | OrderController         |
     | Service     | Бизнес-логика обработки данных           | OrderProcessingService  |
         ...

    **Controller Layer**
      Что делает: принимает входящие HTTP, парсит, валидирует, переводит DTO → команд, отдаёт HTTP-ответ.

      ✅ РАЗРЕШЕНО:
         - Controller → Service (через интерфейс)
         - Controller → Facade
         - Controller → Логи и метрики (через инфраструктурные абстракции)

      ❌ ЗАПРЕЩЕНО:
         - Controller → Domain Layer
         - Controller → Repository / Persistence (не должен знать про БД)
         - Controller → Infrastructure Adapters (REST-clients, Kafka и т.д.)
         - Controller → HTTP-клиенты, MinIO-клиенты, JDBC и т.д.

      Почему?
        Чтобы API не зависел от домена или инфраструктуры. Это boundary layer.

     **Service Layer**
       <аналогично Controller Layer>

    3. Соглашения по именованию (Naming Conventions)
      <Здесь описание как именовать классы, методы, переменные>
      Примеры:
         Controller: OrderController, UserController → методы: getOrder(), processOrder()
         Service: OrderService (интерфейс), OrderServiceImpl (реализация)
         Facade: OrderFacade
         Client: InventoryClient (интерфейс), InventoryRestClient (реализация)
         Mapper: OrderMapper (MapStruct интерфейс)
         DTO: OrderRequest, OrderResponse, InventoryDto
         Entity/Domain: Order, OrderItem (доменная модель)
         Exception: OrderNotFoundException, InvalidOrderException

    4. Структура проекта
      <Здесь описание структуры проекта>

    Пример:
     com.example.service
     ├── controller          # REST контроллеры
     ├── facade             # Фасады (если есть)
     ├── service            # Доменные сервисы
     │   └── impl
     ├── integration        # Клиенты внешних API
     │   ├── client
     │   └── dto
     ├── domain             # Доменные модели
     │   ├── entity
     │   ├── dto
     │   ├── mapper         # MapStruct маппер
     │   └── exception
     ├── config             # Конфигурационные классы
     ├── util               # Утилиты
     └── exception          # Глобальные exception handlers

    5. Паттерны 
     <Здесь описаны применяемые при разработке сервиса паттерны программирования с примерами>
     Пример:
       DTO (Data Transfer Object) — ОБЯЗАТЕЛЕН
       Почему критично:
          Разделение внешнего контракта API и внутреннего домена.
          Защита внутренней модели от изменений снаружи.
          Независимый версионирование API.

       Применение:
         // REST контакт (внешний)
         public record OrderRequest(
             Long customerId,
       String productId,
       Integer quantity
       ) {}
   
       public record OrderResponse(
           Long orderId,
           String status,
           BigDecimal total
       ) {}
       
       // Доменная модель (внутренняя)
       public class Order {
           private Long id;
           private Customer customer;
           private List<OrderItem> items;
           // Может быть значительно сложнее
       }
       Правило: Используйте паттерны для решения конкретной проблемы, а не потому что "это паттерн".

    6. Обработка ошибок (Error Handling)

    7. Конфигурация и переменные окружения

    8. Логирование и трассировка
     <Здесь описание как используем логирование и трассировку в проекте>
     Пример:
     - Используйте @Slf4j от Lombok
     - Логируйте важные бизнес-события (начало/конец операции)
     - Не логируйте чувствительные данные (пароли, token'ы)
     - Для больших JSON используйте truncation через Logbook
     - Трассировка: Micrometer Tracing автоматически добавляет traceId
     
    9. Мониторинг и метрики
     <Здесь описание базовых метрик>
     
     Пример:
     - Counter: количество обработанных запросов, ошибок
     - Timer: время обработки операций, latency внешних API
     - DistributionSummary: размер входящих/исходящих JSON
     - Gauge: очередь обработки, активные потоки
     
     Эндпоинты:
     - /actuator/health → проверка здоровья
     - /actuator/prometheus → метрики для Prometheus
     
    10. Документирование API
     <Здесь описание инструментов и правил документации API>
     Пример:
     - Добавить spring-boot-starter-springdoc-openapi
     - Аннотировать контроллеры @Operation, @Parameter
     - Эндпоинт: /swagger-ui.html для интерактивной документации
     
    11. Безопасность (Security)
     <Здесь описание правил для безопасности сервиса>
     
     Пример:
     - Все эндпоинты должны быть защищены (Spring Security)
     - /actuator/health и /actuator/prometheus могут быть открыты для health-checks
     - Валидировать все входные данные (@Valid на @RequestBody)
     - Использовать HTTPS в продакшене
     
    12. Масштабирование
     <Здесь описание правил для масштабирования, производительности и отказоустойчивости>
     Примеры:
     - Сервис stateless → готов к горизонтальному масштабированию
     - Обработка больших JSON через Jackson Streaming API
     - Кэширование через Redis (Integration Layer для внешних API)
     - Rate Limiter для защиты от перегрузки внешних сервисов
     - HPA в Kubernetes по CPU метрикам
     
    13. Примеры реализации
     <Здесь приведены примеры основных шаблонов кода (code examples)>
     - Пример Controller с REST endpoint
     - Пример Service с бизнес-логикой
     - Пример Integration Client с RestClient
     - Пример Unit Test с Mockito
     - Пример Integration Test с MockMvc
     - Пример MapStruct Mapper

