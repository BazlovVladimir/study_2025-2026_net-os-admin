---
## Front matter
lang: ru-RU
title: Лабораторная работа №2
subtitle: Настройка DNS-сервера BIND (прямая и обратная зона)
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 10 ноября 2025

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

Получить практические навыки настройки **DNS-сервера BIND**, создания прямой и обратной зон  
и проверки работы DNS-разрешения в локальной сети.

# Что использовалось

## Инструменты

- **Rocky Linux** – серверная ОС  
- **BIND (named)** – DNS-сервер  
- **Vagrant + VirtualBox** – виртуальная лаборатория  
- **nmcli, dig, host** – сетевые утилиты

# Анализ конфигурации

## Основные конфигурационные файлы

- `/etc/resolv.conf`
- `/etc/named.conf`
- `/var/named/named.ca`
- `/var/named/named.localhost`

![Просмотр named.conf](Screenshot_2.png){ width=70% }


## Проверка работы хоста как DNS-клиента

`dig www.yandex.ru`  
→ запрос уходит на внешний DNS

`dig @127.0.0.1 www.yandex.ru`  
→ ответ получает локальный сервер BIND

![Сравнение dig](Screenshot_5.png){ width=70% }

# Настройка локального DNS

## Назначение сервера DNS по умолчанию

Интерфейс `eth0` настроен на использование `127.0.0.1`

![Настройка DNS через nmcli](Screenshot_6.png){ width=70% }

## Разрешение запросов от внутренней сети

В `named.conf` изменено:

- `listen-on port 53 { any; };`
- `allow-query { localhost; 192.168.0.0/16; };`

![Изменение доступа](Screenshot_7.png){ width=70% }

# Создание прямой и обратной зоны

## Создание файла зон

![Создание зон vabazlov.net](Screenshot_9.png){ width=70% }

## Прямая зона — `vabazlov.net`

![Файл прямой зоны](Screenshot_10.png){ width=70% }

## Обратная зона — `1.168.192.in-addr.arpa`

![Файл обратной зоны](Screenshot_11.png){ width=70% }

# Проверка работы DNS

## Проверка A-записей и PTR-записей

![dig и host](Screenshot_13.png){ width=70% }

# Итоги работы

## Вывод

В ходе выполнения лабораторной работы:

- установлен и настроен DNS-сервер **BIND**;
- созданы **прямая и обратная зоны**;
- настроен firewall и SELinux;
- выполнена проверка через `dig` и `host`;
- подготовлен **provisioning-скрипт** для автоматического развёртывания.

Настроенный DNS-сервер успешно разрешает доменные имена и может использоваться в локальной сети.
