---
## Front matter
lang: ru-RU
title: Лабораторная работа №16
subtitle: Базовая защита от атак типа «brute force» (Fail2ban)
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 10 декабря 2025

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

Получить практические навыки установки, настройки и проверки работы **Fail2ban**  
для защиты сервера от атак типа *brute force*.

# Установка Fail2ban

## Установка пакета

![Установка fail2ban](Screenshot_1.png){ width=70% }

## Запуск и автозагрузка

![Логи запуска](Screenshot_2.png){ width=70% }

# Начальная конфигурация

## Защита SSH

![Конфигурация SSH](Screenshot_3.png){ width=50% }

## Применение и проверка

![Запуск jail’ов SSH](Screenshot_4.png){ width=70% }

## Активирование модулей Apache

![HTTP-защита](Screenshot_5.png){ width=60% }

## Проверка работы HTTP-защиты

![Активация jail’ов](Screenshot_6.png){ width=70% }

## Активированные jail’ы

![Почтовые jail’ы](Screenshot_7.png){ width=40% }

## Проверка работы

![Запуск всех jail’ов](Screenshot_8.png){ width=70% }

# Проверка Fail2ban

## Статус сервиса

![Статус Fail2ban](Screenshot_9.png){ width=75% }

## Проверка jail’a SSH

- уменьшено maxretry до 2;  
- выполнены неверные попытки входа.

![Блокировка IP](Screenshot_11.png){ width=75% }

## Разблокировка и повторная проверка

![Unban и проверка](Screenshot_10.png){ width=75% }

# Игнорирование IP

## Настройка ignoreip

![ignoreip](Screenshot_12.png){ width=50% }

## Проверка в логах

![ignoreip в логах](Screenshot_13.png){ width=75% }

## Повторная проверка SSH-защиты

![Нет блокировок](Screenshot_14.png){ width=75% }

# Итоги работы

## Вывод

В ходе лабораторной работы:

- установлен и настроен Fail2ban;
- активирована защита SSH, Apache и почтовых служб;
- проверен механизм блокировки и разблокировки IP;
- реализована автоматизация настройки Fail2ban в составе Vagrant-среды.
