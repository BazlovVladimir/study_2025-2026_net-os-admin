---
## Front matter
lang: ru-RU
title: Лабораторная работа №15
subtitle: Настройка сетевого журналирования (rsyslog)
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 09 декабря 2025

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

Освоить настройку **сетевого журналирования**  
с использованием **rsyslog** на сервере и клиенте.

---

# Выполнение работы

## Конфигурация rsyslog

![Настройка netlog-server.conf](Screenshot_1.png){ width=70% }

## Проверка работы и Firewall

![Прослушивание порта rsyslog](Screenshot_2.png){ width=70% }

## Конфигурация пересылки логов


![Конфигурация netlog-client.conf](Screenshot_3.png){ width=70% }

## Логи сервера и клиента

![Просмотр логов messages](Screenshot_4.png){ width=70% }

## Графический просмотрщик

![Графический просмотр логов](Screenshot_5.png){ width=70% }

# Заключение

## Итоги работы

В ходе выполнения работы:

- настроено сетевое журналирование rsyslog;
- сервер принимает логи по TCP 514;
- клиент успешно отправляет сообщения;
- изучены инструменты анализа логов;
- подготовлена автоматизация через Vagrant.
