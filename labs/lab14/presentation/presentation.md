---
lang: ru-RU
title: Лабораторная работа №14
subtitle: Настройка файловых служб Samba
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 06 декабря 2025
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
Приобрести навыки настройки доступа групп пользователей к общим ресурсам  
по протоколу **SMB/Samba**.

# Настройка сервера Samba

## Установка пакетов

![Установка пакетов](Screenshot_1.png){ width=70% }

## Конфигурация smb.conf

![Редактирование smb.conf](Screenshot_2.png){ width=70% }

## Проверка и запуск сервиса

![Доступ через smbclient](Screenshot_3.png){ width=70% }

## Настройка Firewalld

![Описание сервиса Samba](Screenshot_4.png){ width=70% }

## Права доступа и SELinux

![Настройка прав доступа](Screenshot_5.png){ width=70% }

## Проверка пользователя

![Проверка групп](Screenshot_6.png){ width=70% }

# Монтирование Samba на клиенте

## Установка и настройка клиента

![Описание samba-client](Screenshot_7.png){ width=70% }

## Настройка групп и smb.conf

![Создание группы sambagroup](Screenshot_8.png){ width=70% }

## Проверка доступных ресурсов

![Просмотр ресурсов](Screenshot_10.png){ width=70% }

## Монтирование CIFS

![Монтирование ресурса](Screenshot_11.png){ width=70% }

## Проверка на сервере

![Проверка файла](Screenshot_13.png){ width=70% }

## Автоматическое монтирование

![fstab для SMB](Screenshot_15.png){ width=70% }

# Итоги работы

## Вывод
- Настроены сервер и клиент Samba
- Реализована групповая модель доступа
- Проверены SELinux и межсетевой экран
- Настроено монтирование вручную и через fstab
- Созданы автоматизирующие скрипты для обеих ВМ

