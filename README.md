# Упрощенное Wiki по деплою проектов на Laravel
Проект работает на [Laravel Framework 8.x](https://laravel.com/).

## Требования к серверному ПО для работы приложения 
- [Composer](https://getcomposer.org/).
- [Nginx](https://www.nginx.com/).
- [Mysql 5.7+](https://www.mysql.com/).
- [PHP 7.4.x](https://www.php.net/downloads.php#v7.4.16).

Версия PHP 7.4 используется, если проект сделан с админ. панелью [Twill](https://twill.io/)
В ином случае ставится PHP 8.x

### Необходимые расширения PHP
- FileInfo
- GD Library (>=2.0) или Imagick PHP extension (>=6.5.7)
- BCMath PHP Extension
- Ctype PHP Extension
- Fileinfo PHP Extension
- JSON PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PDO PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension

## Шаги по разворачиванию проекта на сервере

#### 1. В конфиге Nginx ставим:
```nginx
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```
Устанавливаем root домена на папку public

#### 2. Клонируем репозиторий
#### 3. Следующие шаги:
```bash
composer install #из корня проекта
```
*Если файлы закидываем готовым архивом на сервер - пропускаем предыдущую команду.*

Делаем файл .env из .env.example, если его еще нет.
В нем устанавливаем параметры:
```bash
APP_URL=домен, на котором работает приложение
DB_HOST=хост_БД  
DB_PORT=порт_БД
DB_DATABASE=название_БД
DB_USERNAME=пользователь_БД
DB_PASSWORD=пароль_БД
```
Остальное оставляем как есть.
```bash
php artisan key:generate #из корня проекта
```

```bash
php artisan migrate #из корня проекта, запускаем только если не было импорта дампа БД!
```
Или, если есть дамп базы - импортируем его в заранее созданную БД.

```bash
php artisan config:cache #из корня проекта
```
```bash
php artisan route:cache #из корня проекта
```

#### 4. Заливаем предоставленную папку storage в корень проекта

Пропустить, если был передан полный архив

#### 5. php artisan storage:link
Если симлинк public/storage уже существует - перед выполнением команды его нужно удалить

#### 6. Настраиваем SMTP для отправки почты с сайта в .env

MAIL_MAILER=smtp
MAIL_HOST=smtp.yandex.ru
MAIL_PORT=587
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=
MAIL_FROM_NAME="${APP_NAME}"
MAIL_DRIVER=log
MAIL_LOG_CHANNEL=mailer

#### 7. Если в качестве админ. панели используется [Twill](https://twill.io/)

Запускаем из корня проекта
```bash
php artisan twill:superadmin
```
для создания пользователя-администратора
