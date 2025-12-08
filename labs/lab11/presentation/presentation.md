---
## Front matter
lang: ru-RU
title: Лабораторная работа №11
subtitle: Настройка безопасного удалённого доступа по SSH
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

Приобрести практические навыки настройки защищённого удалённого доступа  
к серверу по протоколу **SSH**, включая управление пользователями, портами,  
туннелями, SELinux и межсетевым экраном.

# Ограничение доступа

## Запрет входа root

![Запрет входа пользователя root](Screenshot_1.png){ width=70% }

## Разрешение только конкретных пользователей

![AllowUsers vagrant](Screenshot_6.png){ width=70% }

## Подключение после изменения правил

![Успешный вход после добавления пользователя](Screenshot_9.png){ width=70% }

# Дополнительные порты SSH

## Ошибка доступа к порту 2022

![Ошибка SELinux при привязке порта](Screenshot_10.png){ width=70% }

## Открытие порта в firewall и SELinux

![SSH слушает два порта](Screenshot_11.png){ width=70% }

## Подключение через порт 2022

![Успешный вход через альтернативный порт](Screenshot_12.png){ width=70% }

# Настройка SSH-ключей

## Включение PubkeyAuthentication

![Параметр PubkeyAuthentication](Screenshot_13.png){ width=70% }

## Копирование ключа на сервер

![Копирование SSH-ключа](Screenshot_14.png){ width=70% }

# Туннелирование SSH

## Просмотр TCP перед созданием туннеля

![lsof перед туннелем](Screenshot_15.png){ width=70% }

## Доступ по адресу localhost:8080

![Страница через SSH-туннель](Screenshot_16.png){ width=70% }

# Консольные команды по SSH

## Удалённый hostname

![hostname на сервере](Screenshot_17.png){ width=70% }

## Почта на сервере

![Просмотр почты по SSH](Screenshot_18.png){ width=70% }

# Графические приложения X11

## Разрешение X11Forwarding

![Включение X11Forwarding](Screenshot_19.png){ width=70% }

## Ошибка из-за отсутствия DISPLAY

![Ошибка запуска Firefox](Screenshot_20.png){ width=70% }

# Итоги работы

## Вывод

В результате лабораторной работы:

- изучены механизмы управления безопасным SSH-доступом;
- ограничен вход root и настроен список разрешённых пользователей;
- добавлены дополнительные порты и исправлены правила SELinux;
- выполнена настройка SSH-ключей;
- реализовано туннелирование TCP-портов;
- протестирован запуск консольных и графических приложений через SSH;