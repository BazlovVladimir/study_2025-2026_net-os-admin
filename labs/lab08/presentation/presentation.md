---
lang: ru-RU
title: Лабораторная работа №8
subtitle: Настройка SMTP-сервера (Postfix)
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов
date: 29 ноября 2025
aspectratio: 169
theme: metropolis
slide_level: 2
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы

## Основная цель

Овладеть установкой и настройкой почтового сервера **Postfix**,  
а также диагностикой доставки локальных и доменных сообщений.

# Выполнение работы

## Установка Postfix и s-nail

![Установка postfix и s-nail](Screenshot_1.png){width=80%}

## Проверка списка параметров Postfix

![Список параметров postconf](Screenshot_2.png){width=80%}

## Параметры myorigin и mydomain

![Проверка myorigin и mydomain](Screenshot_3.png){width=80%}

## Вывод postconf -n

![Вывод postconf -n](Screenshot_4.png){width=80%}

## Полученное локальное письмо

![Полученное письмо на сервере](Screenshot_5.png){width=80%}

## Логи доставки письма

![Логи доставки локального письма](Screenshot_6.png){width=80%}

## Установка postfix на клиенте

![Установка postfix на клиенте](Screenshot_7.png){width=80%}

## Новые подключения после перезапуска Postfix

![Перезапуск Postfix и новые подключения](Screenshot_8.png){width=80%}

## Доставка письма от клиента

![Доставка письма от клиента](Screenshot_9.png){width=80%}

## Почтовые файлы с двумя сообщениями

![Письма от сервера и клиента](Screenshot_10.png){width=80%}

## Отправка письма на доменный адрес

![Отправка письма на доменный адрес](Screenshot_11.png){width=80%}

## Ошибка loops back to myself / No route to host

![Журнал сервера](Screenshot_12.png){width=80%}

## Прямая зона DNS

![Прямая зона DNS](Screenshot_13.png){width=80%}

## Обратная зона DNS

![Обратная зона DNS](Screenshot_14.png){width=80%}

## Настройка mydestination

![Изменение mydestination](Screenshot_15.png){width=80%}

## Логи успешной обработки

![Успешная обработка письма сервером](Screenshot_16.png){width=80%}

# Итоги работы

## Вывод

- Настроен сервер Postfix для локальной и доменной почты  
- Реализована отправка между двумя узлами  
- Исправлены ошибки DNS  
- Добавлены MX и PTR записи  
- Проверена доставка в логах  
- Создан скрипт автоматизации окружения

Работа выполнена успешно.
