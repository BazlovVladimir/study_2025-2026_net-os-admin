---
## Front matter
lang: ru-RU
title: Лабораторная работа №10
subtitle: Настройка SMTP-аутентификации, LMTP и TLS в Postfix + Dovecot
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 30 ноября 2025

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

Изучить расширенные механизмы конфигурирования почтового сервера,  
включая **LMTP**, **SMTP-аутентификацию** и **SMTP over TLS**,  
а также интегрировать Postfix и Dovecot в единое почтовое решение.

# Выполнение работы

## Включение LMTP в Dovecot

![Добавление LMTP в список протоколов](Screenshot_1.png){ width=70% }

## Настройка сокета LMTP

![Настройка unix_listener для LMTP](Screenshot_2.png){ width=70% }

## Настройка имени пользователя

![auth_username_format](Screenshot_3.png){ width=70% }

## Логи успешной доставки

![Логи прохождения письма](Screenshot_4.png){ width=70% }

## Проверка INBOX

![Просмотр полученных сообщений](Screenshot_5.png){ width=70% }

## Конфигурация службы auth

![service auth конфигурация](Screenshot_6.png){ width=70% }

## Ограничения Postfix

![postconf ограничения](Screenshot_7.png){ width=70% }

## Параметры smtpd

![master.cf настройки](Screenshot_8.png){ width=70% }

## Проверка аутентификации

![Проверка через telnet](Screenshot_9.png){ width=70% }

## Подготовка сертификатов

![TLS конфигурация и настройка Postfix](Screenshot_10.png){ width=70% }

## Добавление службы submission (порт 587)

![Конфигурация master.cf для 587-го порта](Screenshot_11.png){ width=70% }

## Проверка через openssl

![Проверка TLS-подключения через openssl](Screenshot_12.png){ width=70% }

## Проверка отправки в Evolution

![Проверка отправки через Evolution](Screenshot_13.png){ width=70% }

## Логи работы TLS + LMTP

![Фрагмент логов доставки письма](Screenshot_14.png){ width=70% }

## Копирование конфигурационных файлов

![Копирование конфигураций](Screenshot_15.png){ width=70% }

## Обновлённый provisioning-скрипт

![Расширенная версия mail.sh](Screenshot_16.png){ width=70% }

## Обновление конфигурации клиента

![Provisioning-скрипт клиента](Screenshot_17.png){ width=70% }

# Заключение

## Итоги работы

- Настроены LMTP, SMTP-AUTH и TLS в связке Postfix + Dovecot  
- Проверены доставка писем и защищённые SMTP-сессии  
- Реализована интеграция с системой Vagrant  
- Получены навыки настройки защищённого почтового окружения
