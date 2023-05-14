# Task tracker
Многопользовательский планировщик задач. Пользователи могут использовать его в качестве TODO листа. 

## Использованные технологии

- Java
- Maven
- Spring Boot, Spring Security, Spring Session, Spring AMQP, Spring Scheduler, Spring Mail, Spring Data JPA
- PostgreSQL
- RabbitMQ
- JWT
- Docker
- GitHub Actions

## Микросервисы

При разработке проекта использовался микросервисный подход. Проект включает в себя 3 микросервиса:

- [task-tracker-api](https://github.com/AtoDaX/task-tracker-api)
- [task-tracker-email-sender](https://github.com/AtoDaX/task-tracker-email-sender)
- [task-tracker-scheduler](https://github.com/AtoDaX/task-tracker-scheduler)

### [task-tracker-api](https://github.com/AtoDaX/task-tracker-api)
Spring Boot приложение, реализующее REST API для работы с пользователями и задачами.

Предоставление доступа к функционалу реализовано с помощью JWT-аутентификации.

Работа с пользователями:

- Регистрация
- Авторизация

Работа с задачами (доступно только авторизованым пользователям):

- Создание
- Чтение
- Редактирование(Изменение заголовка и описания, пометка задачи, как сделанной)
- Удаление

### [task-tracker-email-sender](https://github.com/AtoDaX/task-tracker-email-sender)
Spring Boot приложение с двумя модулями - Spring Mail и Spring AMQP.

С помощью Spring AMQP приложение подключается к RabbitMQ и создает очередь, после чего подписывается на сообщения, поступающие от планировщика и бэкенд-сервиса.

Для каждого полученного сообщения, содержимое которого десериализуется в экземпляр модели, используется модуль Spring Mail, чтобы отправить письмо.

### [task-tracker-scheduler](https://github.com/AtoDaX/task-tracker-scheduler)
Spring Boot приложение с двумя модулями - Spring Scheduler и Spring AMQP.

Задача сервиса - раз в сутки итерировать всех пользователей, формировать для них отчёты об отогах дня, а также формировать email для отправки. Сформированные сообщения отправляются в RabbitMQ очередь.

## CI/CD
В рамках проекта с помощью GitHub Actions для каждого микросервиса была реализована автоматическая сборка docker-образов и их загрузка в публичный репозиторий ([dockerhub](https://hub.docker.com/)) при пуше в master ветку.

## Запуск на локальном компьютере
Необходимо:

- Установить Docker на локальную машину — https://docs.docker.com/get-docker/
- Склонировать репозиторий task-tracker-stack
```bash
git clone https://github.com/AtoDaX/task-tracker-stack
```
- Заполнить SMTP-конфигурацию в secret.env. В качестве SMTP-сервиса использовался [Brevo](https://www.brevo.com/)
```bash
#Пример заполнения конфигурации:
SMTP_USERNAME=email@google.com
SMTP_PASSWORD=password
```
- Запустить docker-compose файл
```bash
docker-compose up
```
- Страница с документацией будет доступна по ссылке: http://localhost:8080/swagger-ui/index.html



