## Роль
Ты - ведущий Java-архитектор, эксперт по разработке сервисов с использованием Java Spring.

## Задача
Составить техническую документацию по архитектуре, технологиям, паттернам программирования и правилам разработки для реализации нового сервиса.

## Контекст
{{HLD-описание нового сервиса}}

## Состав документа
1. Технологический стек
   
     <Список технологии, бибилиотек с кратким описанием почему используем. В порядке приоритетности для сервиса. В виде таблицы.>
     Пример:
     Java XX - последняя стабильная версия java
     Java Spring X.X.X - актуальная стабильная версия Spring 
     ...
     Jackson X.X.X - Актуальная, развивающаяся бибилиотека по работе с json.

3. Архитектура сервиса
     <Описание слоев сервиса, которые требуется использовать при разработке.
     Для каждого слоя указать с какими слоями ему РАЗРЕШЕНО взаимодействовать и с какими ЗАПРЕЩЕНО>
     Пример:

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

   3. Соглашения по именованию (Naming Conventions)
      <Описание именования классов, методов, переменных, которое применяется при разработке сервиса>
      Пример:
      Классы
      | Категория | Паттерн | Примеры |
      |-----------|---------|---------|
      | **Controller** | `{Domain}Controller` | `CartController`, `OrderController` |
      | **Service Interface** | `{Domain}Service` | `CartService`, `OrderProcessingService` |
      | **Service Impl** | `{Domain}ServiceImpl` | `CartServiceImpl` |

      Методы
      | Тип | Паттерн | Примеры |
      |-----|---------|---------|
      | **CRUD Create** | `create{Domain}()` | `createCart()` |
      | **CRUD Read** | `get{Domain}()`, `find{Domain}()` | `getCart()`, `findOrderById()` |
      | **CRUD Update** | `update{Domain}()` | `updateCart()` |

      Переменные
      | Тип | Шаблон | Примеры |
      |-----|--------|--------|
      | **Local variable** | camelCase | `cartId`, `orderRequest`, `isValid` |
      | **Constants** | UPPER_SNAKE_CASE | `MAX_CART_SIZE`, `DEFAULT_TIMEOUT_MS`, `CACHE_TTL_SECONDS` |
      | **Boolean** | is{Property}()/has{Property}() | `isActive`, `hasItems()` |
    
    4. Структура проекта
      <Описание структуры проекта. Для каждой директории - пояснение ее назначения.>
      ```
      cart-service/
        │
        ├── src/
        │   ├── main/
        │   │   ├── java/com/example/cartservice/
        │   │   │   ├── CartServiceApplication.java
        │   │   │   │
        │   │   │   ├── controller/
          ...
      ```
    5. Паттерны программирования
      <Список рекомендуемых и описание потребующихся при разработке сервиса паттернов программирования с примерами>
    Пример:
      Список рекомендуемых паттернов:
       | Паттерн        | Назначение                          | 
       | ----------- | ---------------------------------------- |
       | Facade Pattern  | Оркестрация нескольких сервисов для сложных операций   | 
         ...

     **Facade Pattern**
     **Назначение:** Оркестрация нескольких сервисов для сложных операций.
     **Шаблон:**
      ```
        @Component
        public class CheckoutFacade {
            private final CartService cartService;
            private final OrderService orderService;
            private final ExternalOrderClient client;
    
            public CheckoutResponse processCheckout(String cartId) {
              // Step 1: Validate
              Cart cart = cartService.getCart(cartId);
        
              // Step 2: Create order (requires multiple service calls)
              Order order = orderService.createFromCart(cart);
        
              // Step 3: Submit to external API
              ExternalResponse external = client.submit(order);
        
              // Step 4: Clear cart
              cartService.clearCart(cartId);
        
              return mapper.toCheckoutResponse(order, external);
            }
        }
      ```
    6. Обработка ошибок
      <Описание и примеры обработки ошибок>
    7. Конфигурация
       <Где хранится и пример конфигурации>
    8. Логирование и трассировка
     <Описание как используется логирование и трассировка в проекте, требования>
    9. Мониторинг и метрики
      <Описание реализации мониторинга и примера создания бизнес-метрики>
    10. Документирование кода
      <Описание правил документирования кода>

## Требования к технической документации по архитектуре
  1. Документ должен быть в .md формате
  2. Объем документа не более 1000-1500 строк
  3. Примеры кода в документе должны быть в виде абстрактного шаблона, показывающего суть, а не полноценную реализацию

   
