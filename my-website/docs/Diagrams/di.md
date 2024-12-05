---
title: BPMN Диаграмма
sidebar_position: 1
---

# BPMN Диаграмма процессов

## Описание

Данная диаграмма описывает бизнес-процесс оформления заказа в системе Ru.Poizon. Она включает в себя следующие основные этапы:

1. **Система управления товарами**: 
    - Проверка количества товаров в корзине.
    - Добавление товаров в корзину.
    - Проверка доступности товаров.

2. **Система управления доставкой**:
    - Запрос информации о доставке.
    - Обработка анкеты пользователя.
    - Утверждение сроков и стоимости доставки.
    - Генерация оптимальных условий доставки.

3. **Система платежей**:
    - Перенаправление пользователя на страницу оплаты.
    - Обработка успешной или неуспешной оплаты.

## Участники

- **Пользователь**: инициирует процесс оформления заказа.
- **Система управления товарами**: отвечает за работу с корзиной пользователя.
- **Система управления доставкой**: занимается расчетом условий и сроков доставки.
- **Система платежей**: обрабатывает платежи и завершает процесс оформления заказа.

## Основные сценарии

1. **Добавление товаров в корзину**:
    - Если количество товаров в корзине не превышает лимит, товар добавляется.
    - В случае превышения лимита пользователю отказывается в добавлении.

2. **Проверка доступности товара**:
    - Если товар доступен, формируется заказ.
    - Если товар недоступен, отображается отказ в выборе.

3. **Обработка доставки**:
    - Пользователь предоставляет данные о доставке.
    - Условия доставки проверяются и утверждаются.

4. **Оплата заказа**:
    - Пользователь перенаправляется на страницу оплаты.
    - При успешной оплате заказ подтверждается, иначе пользователю сообщается об ошибке.

## Диаграмма
Вы можете скачать XML-файл диаграммы для редактирования [здесь](./process-definition.bpmn).