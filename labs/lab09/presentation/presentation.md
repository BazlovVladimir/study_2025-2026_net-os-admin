---
title: Лабораторная работа №9
subtitle: Настройка POP3/IMAP-сервера 
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 29 ноября 2025

lang: ru-RU

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

- Приобрести навыки установки и простой настройки POP3/IMAP-сервера.  
- Освоить связку **Postfix + Dovecot** для приёма и доставки почты.  
- Научиться проверять работоспособность сервера с помощью клиента и утилит командной строки.

# Выполнение работы

## Протоколы Dovecot

![Протоколы Dovecot](Screenshot_1.png){ width=70% }

## Механизм аутентификации

![Механизм аутентификации](Screenshot_2.png){ width=70% }

## Настройки PAM и passwd

![Настройки PAM и passwd](Screenshot_3.png){ width=70% }

## Расположение почтовых ящиков

![Расположение почтовых ящиков](Screenshot_4.png){ width=70% }

## Настройка межсетевого экрана

![Настройка межсетевого экрана](Screenshot_5.png){ width=70% }

## Настройка учётной записи в Evolution

![Настройка учётной записи в Evolution](Screenshot_6.png){ width=70% }

## Создание тестового письма

![Создание тестового письма](Screenshot_7.png){ width=70% }

## Полученное письмо во входящих

![Полученное письмо во входящих](Screenshot_8.png){ width=70% }

## Записи maillog при работе сервиса

![Записи maillog при работе сервиса](Screenshot_11.png){ width=70% }

## POP3-сессия через telnet

![POP3-сессия через telnet](Screenshot_9.png){ width=70% }

## Просмотр Maildir через s-nail

![Просмотр Maildir через s-nail](Screenshot_10.png){ width=70% }

## Скрипт provisioning на сервере

![Скрипт provisioning на сервере](Screenshot_13.png){ width=70% }

## Скрипт provisioning на клиенте

![Скрипт provisioning на клиенте](Screenshot_14.png){ width=70% }

# Итоги работы

## Выводы

- Развёрнут и настроен почтовый сервер **Postfix + Dovecot**.  
- Реализована работа по протоколам **POP3** и **IMAP**.  
- Подтверждена корректная доставка и чтение почты через Evolution, telnet и s-nail.  
- Настроен журнал маил-сервера и проанализированы основные события.  
- Подготовлены скрипты provisioning для автоматического развёртывания стенда.
