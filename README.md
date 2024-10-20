# README

## Описание проекта

Этот тестовый проект представляет собой веб-приложение на Vue.js, которое позволяет пользователям просматривать сообщения с возможностью динамической загрузки данных при прокрутке вниз. Приложение использует json-server для эмуляции API.
Проект развернут на gh-pages, [здесь.] (https://olvsivkov.github.io/test_indins/)


## Установка и развертывание

### Шаг 1: Клонирование репозитория

```bash
git clone https://github.com/olvsivkov/test_indins.git
cd test_indins

```
### Шаг 2: установка зависимостей

```bash
npm install

```
### Шаг 3: Установите json-server глобально, если он еще не установлен:

```bash
npm install -g json-server

```
### Шаг 4: Запустите json-server для эмуляции API:

```bash
json-server --watch ./src/dataBase/feed.json

```
### Шаг 5: Запустите приложение Vue.js:

```bash
npm run serve

```

Откройте браузер и перейдите по адресу http://localhost:3000 (или другой порт, указанный в консоли).