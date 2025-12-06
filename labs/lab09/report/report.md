---
## Front matter
title: "Отчёт по лабораторной работе №9"
subtitle: "Настройка POP3/IMAP сервера"
author: "Владимир Базлов"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true
toc-depth: 2
lof: true
lot: true
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
    - spelling=modern
    - babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы

Приобретение практических навыков по установке и простейшему конфигурированию POP3/IMAP-сервера.

# Выполнение

## Установка и настройка Dovecot

На виртуальной машине **server** выполнен переход в режим суперпользователя.

Установлены пакеты **dovecot** и **telnet**.

В конфигурационном файле **/etc/dovecot/dovecot.conf** указан список поддерживаемых протоколов:

protocols = imap pop3

![Настройка протоколов в dovecot.conf](Screenshot_1.png){ #fig:001 width=80% }

В файле **/etc/dovecot/conf.d/10-auth.conf** подтверждено использование механизма аутентификации **plain**:

auth_mechanisms = plain

![Метод аутентификации plain](Screenshot_2.png){ #fig:002 width=80% }

В файле **/etc/dovecot/conf.d/auth-system.conf.ext** проверено, что для поиска пользователей и паролей используются **pam** и **passwd**:

passdb {  
 driver = pam  
}  

userdb {  
 driver = passwd  
}

![Настройка PAM и passwd](Screenshot_3.png){ #fig:003 width=80% }

В конфигурационном файле **/etc/dovecot/conf.d/10-mail.conf** настроено расположение почтовых ящиков пользователей:

mail_location = maildir:~/Maildir

![Настройка maildir](Screenshot_4.png){ #fig:004 width=80% }

Задан каталог доставки почты в формате Maildir.

Разрешены службы POP3, POP3S, IMAP и IMAPS. Выполнена перезагрузка конфигурации межсетевого экрана и проверен список активных сервисов.

![Настройка межсетевого экрана](Screenshot_5.png){ #fig:005 width=80% }

Контексты безопасности восстановлены для каталога /etc.

Postfix перезапущен, служба Dovecot включена и запущена.

## Проверка работы Dovecot

Через `doveadm` были выведены доступные почтовые ящики пользователя, что подтверждает корректную работу Dovecot.

На виртуальной машине client установлен почтовый клиент Evolution.

В процессе настройки в Evolution были введены параметры подключения:

- Адрес почты вида `user@user.net`
- IMAP-сервер: `mail.user.net`, порт 143
- SMTP-сервер: `mail.user.net`, порт 25
- Способ шифрования: IMAP — STARTTLS, SMTP — отсутствует
- Метод аутентификации для IMAP — обычный пароль, для SMTP — без аутентификации

![Настройка учётной записи](Screenshot_6.png){ #fig:006 width=80% }

Было отправлено тестовое письмо самому себе через Evolution.

![Отправка письма](Screenshot_7.png){ #fig:007 width=80% }

Письмо успешно доставлено и появилось во входящих Evolution.

![Письмо получено](Screenshot_8.png){ #fig:008 width=80% }

При отправке и приёме писем в журнале `/var/log/maillog` появлялись корректные записи Postfix и Dovecot:

- успешная доставка в Maildir
- успешная авторизация посредством механизма PLAIN
- корректное завершение POP3 сессий

![Записи maillog](Screenshot_11.png){ #fig:011 width=80% }

С сервера выполнено подключение через Telnet к службе POP3:

- Выполнен вход под своим логином
- Получен список писем
- Извлечено письмо `retr 1`
- Удалено второе письмо
- Завершена POP3-сессия

![Работа через Telnet](Screenshot_9.png){ #fig:009 width=80% }

### 9. Проверка Maildir средствами s-nail

Затем содержимое Maildir было проверено через консольный клиент `s-nail`, что также подтвердило наличие доставленных писем.

![Проверка через s-nail](Screenshot_10.png){ #fig:010 width=80% }

## Внесение изменений в настройки внутреннего окружения виртуальной машины

В каталоге `/vagrant/provision/server` была создана структура для хранения конфигурации Dovecot. В неё были скопированы файлы:

- `dovecot.conf`
- `10-auth.conf`
- `auth-system.conf.ext`
- `10-mail.conf`

![Копирование конфигов Dovecot](Screenshot_12.png){ #fig:012 width=80% }

В скрипт были добавлены:

- установка Dovecot и Telnet
- настройка межсетевого экрана
- настройка Postfix (Maildir)
- перезапуск Postfix и запуск Dovecot

![Обновлённый provisioning-скрипт сервера](Screenshot_13.png){ #fig:013 width=80% }

В скрипт на client добавлена установка Evolution.

![Provisioning скрипт client](Screenshot_14.png){ #fig:014 width=80% }

# Контрольные вопросы

1. **За что отвечает протокол SMTP?**  
   SMTP — протокол, предназначенный для отправки электронной почты. Он обеспечивает передачу сообщений от клиента к почтовому серверу и между почтовыми серверами. Используется только для исходящих писем.

2. **За что отвечает протокол IMAP?**  
   IMAP — протокол для работы с почтовыми сообщениями на сервере. Позволяет хранить письма на сервере, синхронизировать состояние почтовых папок между разными устройствами, работать с несколькими клиентами одновременно.

3. **За что отвечает протокол POP3?**  
   POP3 — протокол для получения почты. Он загружает письма с сервера на устройство пользователя, после чего сообщения обычно удаляются на сервере. Подходит для локальной работы без постоянной синхронизации.

4. **В чём назначение Dovecot?**  
   Dovecot — почтовый сервер, отвечающий за предоставление доступа к пользовательским почтовым ящикам по протоколам IMAP и POP3. Также обеспечивает аутентификацию пользователей и управление почтовыми хранилищами.

5. **В каких файлах обычно находятся настройки работы Dovecot? За что отвечает каждый из файлов?**  
   Основные файлы настроек:
   - **/etc/dovecot/dovecot.conf** — основной файл конфигурации, включает общие параметры, список используемых почтовых протоколов.  
   - **/etc/dovecot/conf.d/10-auth.conf** — настройки механизмов аутентификации пользователей.  
   - **/etc/dovecot/conf.d/auth-system.conf.ext** — параметры доступа к базе пользователей (PAM, passwd и др.).  
   - **/etc/dovecot/conf.d/10-mail.conf** — параметры месторасположения почтовых ящиков, формат хранения почты (Maildir или mbox).  
   - **/etc/dovecot/conf.d/10-ssl.conf** — настройки SSL/TLS для шифрованных соединений.

6. **В чём назначение Postfix?**  
   Postfix — это MTA (Mail Transfer Agent), почтовый сервер, отвечающий за приём, доставку и отправку писем. Он получает сообщения от клиентов или других серверов и доставляет их локальному пользователю или пересылает другим почтовым системам.

7. **Какие методы аутентификации пользователей можно использовать в Dovecot и в чём их отличие?**  
   Основные методы:
   - **plain / login** — передача пароля в открытом виде (желательно использовать только при работе через STARTTLS или SSL).  
   - **cram-md5 / digest-md5** — пароль не передаётся напрямую, используется хеширование.  
   - **gssapi / gss-spnego** — аутентификация через Kerberos, без передачи пароля.  
   Различие заключается в уровне защищённости и способе передачи учётных данных.

8. **Приведите пример заголовка письма с пояснениями его полей.**  
   Пример:
   
```
From: [user@example.com](mailto:user@example.com)
To: [admin@example.com](mailto:admin@example.com)
Subject: Test message
Date: Mon, 01 Dec 2025 12:00:00 +0000
Message-ID: [abc123@example.com](mailto:abc123@example.com)
```
Пояснения:
- **From** — отправитель письма.  
- **To** — получатель письма.  
- **Subject** — тема сообщения.  
- **Date** — дата и время отправки.  
- **Message-ID** — уникальный идентификатор письма.

9. **Приведите примеры использования команд для работы с почтовыми протоколами через терминал.**  
Примеры POP3 через telnet:
- Подключение к серверу: `telnet mail.server.net 110`  
- Вход пользователя: `user имя_пользователя`  
- Ввод пароля: `pass пароль`  
- Список писем: `list`  
- Получение письма №1: `retr 1`  
- Удаление письма №2: `dele 2`  
- Завершение сеанса: `quit`

Пример работы с SMTP через telnet:  
- `telnet mail.server.net 25`  
- `ehlo client`  
- `mail from:<user@server.net>`  
- `rcpt to:<user@server.net>`  
- `data` — начало письма  
- `.` — конец письма  
- `quit`

10. **Приведите примеры с пояснениями по работе с doveadm.**  
Примеры:
- Просмотр списка почтовых папок пользователя:  
  `doveadm mailbox list -u user`  
  Позволяет увидеть доступные папки Maildir или IMAP.

- Проверка состояния почтового ящика:  
  `doveadm mailbox status -u user all INBOX`  
  Показывает количество писем, непрочитанных сообщений и т.п.

- Поиск писем по условию:  
  `doveadm search -u user subject test`  
  Находит письма с указанной темой.

- Экспорт сообщений:  
  `doveadm fetch -u user hdr subject`  
  Выводит заголовки писем.

# Заключение

В ходе выполнения работы была проверена корректность функционирования почтового сервера, включающего связку Postfix и Dovecot. Настроены и протестированы протоколы IMAP и POP3, обеспечивающие доступ к пользовательским почтовым ящикам. С помощью почтового клиента Evolution выполнена отправка и приём тестовых сообщений, что подтвердило правильность настроек аутентификации, маршрутизации и доставки почты.

Дополнительно проведена проверка работы сервера через Telnet, что позволило вручную просмотреть список писем, извлечь их содержимое и выполнить операции удаления. Журнал почтовой службы показал корректное взаимодействие Postfix и Dovecot на всех этапах обработки сообщений.

По итогам работы подготовлены и сохранены конфигурационные файлы для повторного развёртывания сервера через механизм Vagrant provisioning. Полученные навыки позволяют уверенно настраивать почтовые службы, анализировать их работу и автоматизировать процесс конфигурации в виртуализированных средах.
