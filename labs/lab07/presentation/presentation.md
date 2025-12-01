---
lang: ru-RU
title: Лабораторная работа №7
subtitle: Настройка Firewalld, перенаправления портов и Masquerading
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 24 ноября 2025

toc: false
slide_level: 2
aspectratio: 169
theme: metropolis
section-titles: true
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы

## Основная цель

Получить практические навыки настройки **Firewalld**,  
создания пользовательских служб, настройки **Port Forwarding**  
и включения **Masquerading** в Linux.

# Работа с  Firewalld

## Пользовательская служба

![Просмотр файла ssh-custom](Screenshot_1.png){ width=70% }

## Проверка и активация службы

![Список служб](Screenshot_3.png){ width=70% }

## Forwarding 2022 → 22

![SSH на порт 2022](Screenshot_5.png){ width=70% }

## Включение forwarding

![Проверка forwarding](Screenshot_6.png){ width=70% }

## Включение Masquerading

![Доступ в Интернет](Screenshot_7.png){ width=70% }

# Итоги работы

## Вывод

- Создана пользовательская служба Firewalld  
- Настроено перенаправление порта  
- Включён forwarding и masquerading  
- Настройки интегрированы в Vagrant provisioning  
- Подтверждена корректная работа сети и механизмов Firewalld
