---
title: Взаимодействие пользователя с системой
sidebar_position: 1
---

# Сценарии взаимодействия с системой

## Диаграмма 1: Основной сценарий взаимодействия

```plantuml
@startuml

actor "Пользователь" as user
participant "Система Poizon на русском" as poizon
participant "Система управления товарами" as inventory
participant "Система доставки" as delivery
participant "Система оплаты" as payment

user -> poizon: Просмотр каталога товаров
activate poizon
poizon -> inventory: Запрос списка доступных товаров
activate inventory
inventory --> poizon: Возврат списка товаров
deactivate inventory
poizon --> user: Показать доступные товары
deactivate poizon

user -> poizon: Добавить товар в корзину
activate poizon
poizon --> user: Подтверждение добавления товара
deactivate poizon

user -> poizon: Оформить заказ
activate poizon
poizon -> user: Запрос информации для выбора доставки
deactivate poizon

user -> poizon: Выбор способа и сроков доставки
activate poizon
poizon -> delivery: Запрос на проверку возможностей доставки
activate delivery
delivery --> poizon: Подтверждение возможности доставки
deactivate delivery

alt Доставка возможна
poizon --> user: Подтверждение выбранного способа доставки
deactivate poizon

user -> poizon: Ввод данных платежа
activate poizon
poizon -> payment: Проверка платежных данных

alt Успешная обработка платежа
payment --> poizon: Подтверждение успешного платежа

poizon -> delivery: Запрос на организацию доставки
activate delivery
delivery --> poizon: Подтверждение доставки
deactivate delivery
poizon --> user: Подтверждение заказа и доставки
else Ошибка при обработке платежа
payment --> poizon: Ошибка обработки платежа
poizon -> user: Сообщение об ошибке оплаты
end
else Доставка невозможна
delivery --> poizon: Ошибка возможности доставки
poizon -> user: Сообщение об ошибке доставки
end

@enduml

```
![код](.\sequence-diagram.puml)

# Описание алгоритма

## Пользователь взаимодействует с системой:

1. Запрашивает каталог товаров.
2. Выбирает товар и добавляет его в корзину.

## Система Poizon выполняет:

1. Запрос данных из системы управления товарами.
2. Подтверждение добавления товара в корзину.
3. Проверку возможности доставки через систему доставки.
4. Обработку платежей через систему оплаты.

## Обработка сценариев:

- Если все данные корректны:
  - Пользователь получает подтверждение оформления заказа и доставки.
- В случае ошибок:
  - Пользователь уведомляется о проблемах с оплатой или доставкой.

---

# Детализированный сценарий оформления заказа

## Диаграмма

```plantuml

@startuml use-case
actor "Пользователь" as user
participant "Web-интерфейс" as webUI
participant "Система корзины" as cart
participant "Система управления товарами" as inventory
participant "Система управления заказами" as orderSystem
participant "Система доставки" as delivery
participant "Система оплаты" as payment
database "База данных" as db

user -> webUI: Просмотр каталога товаров
activate webUI
webUI -> inventory: Запрос списка доступных товаров
activate inventory
inventory --> webUI: Возврат списка товаров
deactivate inventory
webUI --> user: Показать доступные товары
deactivate webUI

user -> webUI: Добавить товар в корзину
activate webUI
webUI -> cart: Запрос на добавление товара в корзину
activate cart
cart --> webUI: Подтверждение добавления товара
deactivate cart
webUI --> user: Подтверждение добавления товара
deactivate webUI

user -> webUI: Оформить заказ
activate webUI
webUI -> cart: Получение содержимого корзины
cart --> webUI: Содержимое корзины

webUI -> orderSystem: Запрос информации для оформления заказа
activate orderSystem
orderSystem -> delivery: Запрос на проверку возможностей доставки
activate delivery
delivery --> orderSystem: Подтверждение возможности доставки
deactivate delivery

alt Доставка возможна
orderSystem --> webUI: Подтверждение выбранного способа доставки
deactivate orderSystem
webUI --> user: Подтверждение выбранного способа доставки

user -> webUI: Ввод данных платежа
activate webUI
webUI -> payment: Проверка платежных данных
activate payment

alt Успешная обработка платежа
payment --> webUI: Подтверждение успешного платежа
deactivate payment

webUI -> orderSystem: Подтверждение оформления заказа
activate orderSystem
orderSystem -> db: Сохранение информации о заказе
db --> orderSystem: Подтверждение сохранения
orderSystem -> delivery: Запрос на организацию доставки
activate delivery
delivery --> orderSystem: Подтверждение доставки
deactivate delivery
orderSystem --> webUI: Подтверждение заказа и доставки
webUI --> user: Подтверждение заказа и доставки
deactivate orderSystem

else Ошибка при обработке платежа
payment --> webUI: Ошибка обработки платежа
webUI -> user: Сообщение об ошибке оплаты
end
else Доставка невозможна
delivery --> orderSystem: Ошибка возможности доставки
orderSystem --> webUI: Сообщение об ошибке доставки
webUI -> user: Сообщение об ошибке доставки
end
@enduml


```
![код](.\use-case.puml)
---

## Описание алгоритма

### Пользователь взаимодействует с веб-интерфейсом:

1. Просматривает каталог товаров.
2. Добавляет товары в корзину.
3. Оформляет заказ.

### Взаимодействие между системами:

1. Система корзины предоставляет данные о содержимом корзины.
2. Система управления заказами проверяет возможность доставки и обрабатывает запрос.
3. Система оплаты выполняет проверку и подтверждение платежей.

### Результат:

- **Успешный заказ**: данные сохраняются в базе, доставка организуется.
- **Ошибка**: пользователь получает уведомление о проблеме.

