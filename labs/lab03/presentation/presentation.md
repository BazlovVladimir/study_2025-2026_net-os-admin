---
## Front matter
lang: ru-RU
title: Лабораторная работа №3
subtitle: Настройка DHCP-сервера KEA и интеграция с DNS
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 14 ноября 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf (Beamer)
toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы

## Основная цель

Получить практические навыки настройки DHCP-сервера **Kea**,  
а также реализовать динамическое обновление DNS-зон (**DDNS**)  
с использованием Bind9 и TSIG-ключей.

# Конфигурирование DHCP-сервера

## Изменение доменных параметров

![Редактирование kea-dhcp4.conf](Screenshot_1.png){ width=70% }

## Настройка подсети

![Конфигурирование подсети](Screenshot_2.png){ width=70% }

## Проверка и включение службы

![systemctl действия](Screenshot_3.png){ width=70% }

## Добавление записей в зоны

![Прямая зона](Screenshot_4.png){ width=70% }

## Добавление записей в зоны

![Обратная зона](Screenshot_5.png){ width=70% }

## Проверка работы DNS

![Проверка](Screenshot_6.png){ width=70% }

## Выдача адреса

![ifconfig клиента](Screenshot_8.png){ width=70% }

## Запись аренды

![lease файл](Screenshot_9.png){ width=70% }

## Генерация TSIG-ключа

![Создание ключа](Screenshot_10.png){ width=70% }

## Разрешение обновлений зон

![named.conf изменения](Screenshot_11.png){ width=70% }

## Подготовка ключа для Kea

![tsig-keys.json](Screenshot_12.png){ width=70% }

## Настройка Kea DHCP-DDNS

![kea-dhcp-ddns.conf](Screenshot_13.png){ width=70% }

## Запуск службы DDNS

![Статус ddns](Screenshot_14.png){ width=70% }

## Включение DDNS в Kea DHCP

![kea-dhcp4 изменения](Screenshot_15.png){ width=70% }

## Проверка результата

![dig client.vabazlov.net](Screenshot_18.png){ width=70% }

# Итоги работы

## Вывод

В результате выполнения лабораторной работы:

- Настроен DHCP-сервер Kea на выдачу сетевых параметров.
- Реализована интеграция DHCP с Bind9 через DDNS.
- Настроены TSIG-ключи и безопасность взаимодействия.
- Подтверждена работа автоматического создания A- и PTR-записей.
- Подготовлены provisioning-скрипты для автоматизации настройки.
- Клиентские машины корректно получают настройки и DNS-записи.

Настройка завершена успешно, инфраструктура функционирует стабильно.

