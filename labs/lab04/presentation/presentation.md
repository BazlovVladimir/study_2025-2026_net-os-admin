---
lang: ru-RU
title: Лабораторная работа №4
subtitle: Настройка HTTP-сервера Apache и виртуального хостинга
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 22 ноября 2025

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

Получить практические навыки установки и настройки **Apache HTTP Server**,  
включая работу с DNS-зонами и конфигурацией **виртуального хостинга**. :contentReference[oaicite:0]{index=0}

# Конфигурация HTTP-сервера

## Просмотр конфигурационных файлов

![Конфигурационные файлы](Screenshot_1.png){ width=70% }

## Разрешение HTTP в firewall

![Запуск httpd](Screenshot_2.png){ width=70% }

## Проверка работоспособности

![Тестовая страница Apache](Screenshot_3.png){ width=70% }

## Логи ошибок и доступа

![Журналы httpd](Screenshot_4.png){ width=70% }

## Изменения в DNS-зонах

![Прямая зона](Screenshot_5.png){ width=70% }

## Изменения в DNS-зонах

![Обратная зона](Screenshot_6.png){ width=70% }

## Конфигурация виртуальных хостов

![server.vabazlov.net](Screenshot_7.png){ width=70% }

## Конфигурация виртуальных хостов

![www.vabazlov.net](Screenshot_8.png){ width=70% }

## Создание веб-контента

![Создание контента](Screenshot_9.png){ width=70% }

## Проверка виртуальных хостов

![server.vabazlov.net](Screenshot_10.png){ width=70% }

## Проверка виртуальных хостов

![www.vabazlov.net](Screenshot_11.png){ width=70% }

# Заключение

## Итог

Настроен веб-сервер Apache с поддержкой виртуального хостинга.  
Внесены изменения в DNS-зоны, созданы конфигурации хостов и веб-контент,  
выполнена проверка доступности сайтов. Полученные навыки позволяют  
развёртывать и сопровождать многосайтовые веб-службы.  
