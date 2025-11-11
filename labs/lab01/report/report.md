---
## Front matter
title: "Отчёт по лабораторной работе №1"
subtitle: "Подготовка лабораторного стенда"
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

Целью данной работы является приобретение практических навыков установки Rocky Linux на виртуальную машину с помощью инструмента Vagrant.

# Выполнение

## Развёртывание виртуальной машины в VirtualBox с использованием Vagrant

1. Установлен плагин **vagrant-vbguest**, необходимый для корректной работы гостевых дополнений VirtualBox.  
   После выполнения команды отображается сообщение об успешной установке.

2. В рабочем каталоге зарегистрирован образ виртуальной машины Rocky Linux, сформированный ранее packer-ом.  
   Для добавления box-файла использована команда:

   vagrant box add rockylinux10 vagrant-virtualbox-rockylinux10-x86_64.box

   Vagrant успешно обнаружил файл и добавил его в локальное хранилище образов.

   ![Добавление box-файла в Vagrant](Screenshot_1.png){ #fig:001 width=80% }

3. Выполнен запуск виртуальной машины **server** командой:

   vagrant up server

   Vagrant импортировал образ, подготовил виртуальную машину и настроил сетевые параметры.

   ![Запуск виртуальной машины server](Screenshot_2.png){ #fig:002 width=80% }

4. Подключение к серверу выполнено через SSH:

   vagrant ssh server

   После ввода пароля `vagrant` выполнен успешный вход в систему.

   ![Подключение по SSH к серверу](Screenshot_3.png){ #fig:004 width=80% }

5. После входа открыта терминальная сессия пользователя в запущенной виртуальной машине Rocky Linux.

   ![Графическое окружение виртуальной машины](Screenshot_4.png){ #fig:005 width=80% }

Преподаватель **Д. С. Кулябов** уточнил, что **клиентская виртуальная машина начнёт работать только после выполнения лабораторной работы №3**.  
По этой причине в рамках текущего задания была запущена **только виртуальная машина server**, а развёртывание client не выполнялось.

# Контрольные вопросы

1. **Для чего предназначен Vagrant?**  
   Vagrant — это инструмент для автоматизированного развёртывания и управления виртуальными машинами. Он позволяет быстро создавать, конфигурировать и запускать виртуальные среды для тестирования и разработки, обеспечивая воспроизводимость окружения.

2. **Что такое box-файл? В чём назначение Vagrantfile?**  
   **Box-файл** — это образ виртуальной машины, содержащий заранее установленную операционную систему. Box используется Vagrant как шаблон, из которого создаются новые виртуальные машины.  
   **Vagrantfile** — файл конфигурации проекта Vagrant. В нём задаются параметры виртуальной машины: выбор box-образа, настройки сети, общие папки, управление провайдером VirtualBox и т.п.

3. **Приведите описание и примеры вызова основных команд Vagrant.**  
   - `vagrant box add` — добавление box-файла в локальное хранилище.  
     Пример: *vagrant box add rockylinux10 vagrant-virtualbox-rockylinux10-x86_64.box*
   - `vagrant up` — запуск виртуальной машины.  
     Пример: *vagrant up server*
   - `vagrant ssh` — подключение к виртуальной машине через SSH.  
     Пример: *vagrant ssh server*
   - `vagrant halt` — корректное завершение работы виртуальной машины.  
     Пример: *vagrant halt server*
   - `vagrant destroy` — полное удаление виртуальной машины.  
     Пример: *vagrant destroy client*

4. **Дайте построчные пояснения содержания файлов vagrant-rocky.pkr.hcl, ks.cfg, Vagrantfile, Makefile.**  
   - **vagrant-rocky.pkr.hcl** — конфигурация для Packer.  
     Описывает процесс автоматизированного создания box-образа (тип билда, параметры VirtualBox, путь к файлам и установочным скриптам).
   - **ks.cfg** — kickstart-скрипт автоматической установки ОС Rocky Linux.  
     Определяет параметры разметки диска, локализацию, пользователя, сетевые настройки и пакеты для установки.
   - **Vagrantfile** — основная конфигурация Vagrant.  
     Определяет используемый box, настройки сети, каталогов, имя виртуальной машины, провайдер (VirtualBox).
   - **Makefile** — автоматизация выполнения задач сборки.  
     Содержит команды для вызова Packer, добавления box-файла в Vagrant, запуска виртуальных машин, упрощая выполнение повторяющихся действий.

# Заключение

В ходе работы удалось развернуть виртуальную машину Rocky Linux в VirtualBox с использованием Vagrant. Были изучены основные команды Vagrant, процесс добавления box-файла и запуск виртуальной машины. Подключение по SSH подтвердило корректность конфигурации. Полученные навыки позволяют далее автоматизировать создание рабочих окружений и использовать их в последующих лабораторных работах.
