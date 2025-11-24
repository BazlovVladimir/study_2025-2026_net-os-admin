---
lang: ru-RU
title: Лабораторная работа №5
subtitle: Расширенная настройка HTTP-сервера Apache
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 22 ноября 2025

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

Приобрести навыки расширенного конфигурирования HTTP-сервера **Apache**:  
настройка **HTTPS**, работа **PHP** и обновление provisioning-окружения.

# Настройка HTTPS

## Генерация SSL-сертификата

![Генерация SSL-сертификата](Screenshot_1.png){ width=70% }

## Настройка виртуальных хостов

![Конфигурация хостов](Screenshot_2.png){ width=70% }

## Разрешение HTTPS в firewall

![Firewall](Screenshot_3.png){ width=70% }

## Проверка работы

![Перенаправление](Screenshot_4.png){ width=70% }

## Проверка работы

![Просмотр сертификата](Screenshot_5.png){ width=70% }

## Установка PHP

![Установка PHP](Screenshot_6.png){ width=70% }

## Проверка phpinfo()

![phpinfo](Screenshot_7.png){ width=70% }

# Итоги работы

- настроен HTTPS с самоподписанным сертификатом;
- выполнено перенаправление HTTP → HTTPS;
- настроена поддержка PHP;
- обновлены SELinux-контексты и права;
- конфигурация сохранена в provisioning для автоматического развёртывания.
