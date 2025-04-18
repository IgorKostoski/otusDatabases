# 📦 Delivery System Database (PostgreSQL)

Этот репозиторий содержит схему для системы управления доставкой/заказами, построенную на PostgreSQL. Включает поддержку для клиентов, продуктов, курьеров, заказов, ценообразования, складов и отслеживания доставок.


---

## 📚 Contents

- [Описание](#-about)
- [Обзор схемы](#-schema-overview)
- [Диаграмма сущностей и связей](#-entity-relationship-diagram)
- [Используемые функции PostgreSQL](#-postgresql-features-used)
- [Начало работы](#-getting-started)
- [Таблицы и типы](#-tables-and-types)
- [Примеры бизнес-использования](#-business-use-case-examples)
- [Лицензия](#-license)

---

## 📖 About

Эта схема базы данных предназначена для поддержки масштабируемой и нормализованной бэкенд-системы для электронной коммерции или системы доставки. Она управляет:

- Каталогами продуктов
- Данными о поставщиках и производителях
- Профилями клиентов и адресами доставки
- Отслеживанием курьеров
- Заказами и товарами
- Инвентаризацией и ценами

---

## 🧠 Schema Overview

Схема содержит следующие ключевые модули:

  - Продукты и Цены: Включая поставщиков, производителей, категории и исторические цены
  - Инвентаризация: Отслеживание запасов по продуктам
  - Клиенты: Профили, адреса и предпочтения
  - Заказы: Заголовки заказов и строки товаров
  - Доставка: Управление курьерами и отслеживание доставок

---

## 🧭 Entity Relationship Diagram

> Вы можете заново сгенерировать эту диаграмму с помощью таких инструментов как dbdiagram.io, SQLDBM или pgModeler.

> You can regenerate this diagram using tools like [dbdiagram.io](https://dbdiagram.io), [SQLDBM](https://sqldbm.com), or [pgModeler](https://pgmodeler.io/).

![Schema Diagram](docs/schema_diagram.pdf)

---

## 🛠 PostgreSQL Features Used

Эта схема использует следующие особенности PostgreSQL:

  - Типы `ENUM` для `order_status` и `currency_code`
  - `SERIAL` для первичных ключей
  - Ограничения `CHECK` (например, для количества на складе)
  - Значения по умолчанию (`DEFAULT`), такие как временные метки и валюта
  - Ограничения `UNIQUE` и `FOREIGN KEY` для обеспечения целостности данных


---

## ⚙️ Getting Started

 1. Клонируйте репозиторий:

        git clone https://github.com/IgorKostoski/otusDatabases/tree/main/homeWork_1
    cd delivery-system-db

  2. Запустите PostgreSQL (например, через Docker): docker run --name delivery-db -e POSTGRES_PASSWORD=pass -p 5432:5432 -d postgres:14

  3. Запустите схему: psql -h localhost -U postgres -d postgres -f schema.sql



## 🧾 Tables and Types
| Name               | Description                                      |
|--------------------|--------------------------------------------------|
| order_status       | Enum для отслеживания статуса заказов           |
| currency_code      | Enum для поддерживаемых валют                   |
| categories         | Категории продуктов                              |
| manufacturers      | Информация о производителях                     |
| suppliers          | Контакты и адреса поставщиков                   |
| products           | Каталог продуктов                               |
| product_suppliers  | Связь многие ко многим между продуктами и поставщиками |
| prices             | Историческая информация о ценах                |
| stock              | Уровни запасов продуктов                        |
| customers          | Профили клиентов                                |
| addresses          | Адреса доставки                                 |
| couriers           | Курьеры                                          |
| orders             | Заказы и их статусы                             |
| order_items        | Товары в заказах                                |



## 💼 Business Use Case Examples

## 🛒 Order Processing

-- Размещение нового заказа 
INSERT INTO orders (...) VALUES (...);
INSERT INTO order_items (...) VALUES (...);

-- Проверка наличия товара
SELECT quantity FROM stock WHERE product_id = ?;

-- Расчет стоимости 
SELECT price_value FROM prices WHERE product_id = ? AND start_date <= NOW() ORDER BY start_date DESC LIMIT 1;

## 🚚 Delivery Management

-- Заказы, готовые к отправке 
SELECT * FROM orders WHERE status = 'processing';


-- Назначение курьера 
UPDATE orders SET courier_id = ?, status = 'assigned' WHERE order_id = ?;

-- Отслеживание доставки 
UPDATE orders SET status = 'shipped' WHERE order_id = ?;


## 📊 Reporting & Analytics

-- Общие продажи за период  
SELECT SUM(total_value) 
FROM orders 
WHERE order_date BETWEEN ? AND ? status = 'delivered';

## 📄 License
Этот проект лицензирован под лицензией MIT. Подробности в файле LICENSE.
This project is licensed under the MIT License.See LICENSE file for details.
