---
## Front matter
lang: ru-RU
title: Лабораторная работа №1
subtitle: Подготовка лабораторного стенда (Vagrant + VirtualBox + Rocky Linux)
author:
  - Владимир Базлов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 10 ноября 2025

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

Получить навыки развёртывания виртуальной машины **Rocky Linux** в VirtualBox с использованием инструмента **Vagrant**  
и ознакомиться с процессом автоматизации создания образов.

# Что использовалось

## Инструменты

- **VirtualBox** — гипервизор для виртуальных машин  
- **Vagrant** — автоматизация развёртывания ВМ  
- **Packer** — создание box-образов  
- **Rocky Linux** — устанавливаемая ОС

# Установка и настройка

## Установка плагина Vagrant

![Добавление box-файла в Vagrant](Screenshot_1.png){ width=70% }

## Команда запуска

![Запуск виртуальной машины server](Screenshot_2.png){ width=70% }

## Вход в систему

![Подключение по SSH к серверу](Screenshot_3.png){ width=70% }

## Открытый терминал

![Графическое окружение виртуальной машины](Screenshot_4.png){ width=70% }

# Итоги работы

## Вывод

В ходе выполнения лабораторной работы:

- создан и зарегистрирован box-образ Rocky Linux;
- запущена виртуальная машина `server`;
- выполнено подключение по SSH;
- освоены базовые команды Vagrant.

Полученные навыки позволяют в дальнейшем быстро создавать рабочие среды для следующих лабораторных работ.
