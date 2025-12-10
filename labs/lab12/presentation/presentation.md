---
## Front matter
lang: ru-RU
title: Лабораторная работа №12
subtitle: Синхронизация времени (chrony)
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 03 декабря 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
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

Получить навыки работы с системным временем Linux и  
настроить синхронизацию времени между сервером и клиентом  
с использованием службы **chrony**.

# Просмотр настроек времени

## Сервер

![Системное время на сервере](Screenshot_1.png){ width=70% }

## Клиент

![Системное время на клиенте](Screenshot_2.png){ width=70% }

# Источники синхронизации

## Сервер

![Источники времени на сервере](Screenshot_3.png){ width=70% }

## Клиент

![Источники времени на клиенте](Screenshot_4.png){ width=70% }

# Настройка сервера NTP

## Конфигурация chrony

![Настройка chrony.conf на сервере](Screenshot_5.png){ width=70% }

## Конфигурация chrony

![Перезапуск chronyd на сервере](Screenshot_6.png){ width=70% }

# Настройка клиента NTP

## Конфигурация

![Настройка chrony.conf на клиенте](Screenshot_6.png){ width=70% }

## Проверка

![Проверка источников времени клиента](Screenshot_7.png){ width=70% }

# Итоги работы

## Вывод

- Настроена синхронизация времени между сервером и клиентом  
- Изучены механизмы chrony и NTP  
- Настроен файрвол для разрешения NTP  
- Проверена корректность работы синхронизации  
- Автоматизировано развёртывание конфигураций через скрипты
