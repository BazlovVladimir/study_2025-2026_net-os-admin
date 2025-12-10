---
## Front matter
lang: ru-RU
title: Лабораторная работа №13
subtitle: Настройка NFS
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 06 декабря 2025

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

Приобрести навыки настройки **NFS-сервера и клиента**, включая:
- экспорт каталогов;
- настройку SELinux и firewall;
- автоматическое монтирование ресурсов;
- подключение пользовательских каталогов.

# Настройка NFSv4

## Установка и подготовка сервера

![Установка ПО и каталог /srv/nfs](Screenshot_1.png){ width=70% }

## Конфигурация экспорта

![Редактирование /etc/exports](Screenshot_2.png){ width=70% }

## Разрешение NFS в firewall

![Настройка firewall](Screenshot_3.png){ width=70% }

## Работа программы showmount

![Обзор экспортируемых ресурсов](Screenshot_5.png){ width=70% }

## Подключение каталога

![Монтирование NFS](Screenshot_6.png){ width=70% }

## Автоматическое монтирование

![Редактирование fstab](Screenshot_7.png){ width=70% }

## Экспорт каталога веб-сервера

![Bind-монтирование www](Screenshot_9.png){ width=70% }

## Экспорт каталога в сеть

![Редактирование exports](Screenshot_10.png){ width=70% }

## Настройка домашнего каталога

![Создание каталогов пользователя](Screenshot_13.png){ width=70% }

## Настройка домашнего каталога

![Экспорт home](Screenshot_14.png){ width=70% }

## Проверка работы прав

![Создание файла клиентом и отказ root](Screenshot_17.png){ width=70% }

# Итоги работы

## Вывод

В ходе выполнения лабораторной работы были:

- настроены сервер и клиент NFS;
- выполнен экспорт системных и пользовательских каталогов;
- реализовано bind-монтирование в дерево NFS;
- обеспечена работа SELinux и firewall;
- настроено автоматическое монтирование через `/etc/fstab`;
- выполнена автоматизация через shell-скрипты.

Получены практические навыки работы с NFS и сетевыми файловыми системами.
