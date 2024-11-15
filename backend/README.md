# Doodocs Backend Challenge

Вы приглашены на первый этап стажировки в Doodocs.kz!

На этом этапе вам предстоит выполнить техническое задание, которое позволит нам познакомиться с вашим подходом к решению задач и оценить уровень ваших навыков.

## Задание

В данном задании вам нужно разработать REST API.

Гайдлайны:
- 💻 Чистый код
- 🏛️ Чистая архитектура
- 🧩 Расширяемый код
- 🧪 Наличие тестов
- 🚀 Оптимизация по CPU и RAM

Список требуемых роутов перечислен ниже.

### 1. Роут для отображения информации по архиву

Роут получает файл, который является архивом и возвращает детальную информацию по структуре этого архива.

Роут должен принимать файл через "multipart/form-data" в секции с названием "file".

> 💡 Если файл не является архивом, верните ошибку. Формат ошибок произвольный.
> Проявите фантазию, лучшие варианты оценим по достоинству.

Структура ответа:

| Поле         | Тип         | Описание                             |
| ------------ | ----------- | ------------------------------------ |
| filename     | string      | Название файла архива                |
| archive_size | float       | Размер архива в байтах               |
| total_size   | float       | Размер всех файлов в архиве в байтах |
| total_files  | float       | Общее кол-во файлов в архиве         |
| files        | Объект файл | Массив файлов содержащихся в архиве  |

Объект файл:

| Поле      | Тип    | Описание                           |
| --------- | ------ | ---------------------------------- |
| file_path | string | Полный путь к файлу, включая папки |
| size      | float  | Размер файла в байтах              |
| mimetype  | string | mimetype файла (тип файла)         |

Пример HTTP-запроса:

```http
POST /api/archive/information HTTP/1.1
Content-Type: multipart/form-data; boundary=-{some-random-boundary}

-{some-random-boundary}
Content-Disposition: form-data; name="file"; filename="my_archive.zip"
Content-Type: application/zip

{Binary data of ZIP file}
-{some-random-boundary}--
```

HTTP-ответ:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "filename": "my_archive.zip",
    "archive_size": 4102029.312,
    "total_size": 6836715.52,
    "total_files": 2,
    "files": [
        {
            "file_path": "photo.jpg",
            "size": 2516582.4,
            "mimetype": "image/jpeg"
        },
        {
            "file_path": "directory/document.docx",
            "size": 4320133.12,
            "mimetype": "application/vnd.openxmlformats-officedocument.wordprocessingml.document"
        }
    ]
}
```

### 2. Роут для формирования архива

Роут принимает список файлов, объединяет их в .zip архив и возращает архив клиенту.

Роут должен принимать файлы через "multipart/form-data" в секции с названием "files[]".

Файлы ограничены по mime типу. Допускается к архивировании только след. типы:
- `application/vnd.openxmlformats-officedocument.wordprocessingml.document`
- `application/xml`
- `image/jpeg`
- `image/png`

Если в списке файлов содержится хоть один невалидный формат файла - возвращать ошибку.

> 💡 Формат ошибок произвольный. Проявите фантазию, лучшие варианты оценим по достоинству.

Пример HTTP-запроса:

```http
POST /api/archive/files HTTP/1.1
Content-Type: multipart/form-data; boundary=-{some-random-boundary}

-{some-random-boundary}
Content-Disposition: form-data; name="files[]"; filename="document.docx"
Content-Type: application/vnd.openxmlformats-officedocument.wordprocessingml.document

{Binary data of file}
-{some-random-boundary}
Content-Disposition: form-data; name="files[]"; filename="avatar.png"
Content-Type: image/png

{Binary data of file}
-{some-random-boundary}--
```

HTTP-ответ:

```http
HTTP/1.1 200 OK
Content-Type: application/zip

{Binary data of ZIP file}
```

### 3. Роут для отправки файла нескольким почтам

Роут принимает файл и список почт - отправляет этот файл указанным почтам.

Роут должен принимать данные через "multipart/form-data":
- Файл в секции с названием "file"
- Список почт в секции с названием "emails"

Файлы ограничены по mime типу. Допускается к отправке только след. типы:
- `application/vnd.openxmlformats-officedocument.wordprocessingml.document`
- `application/pdf`

> 💡 Формат ошибок произвольный. Проявите фантазию, лучшие варианты оценим по достоинству.

> 💡 Можете использовать SMTP сервер GMail или любого другого провайдера. Главное чтобы 
> ваши логины и пароли не утекли в код проекта. Использовать переменные окружения.

Пример HTTP-запроса:

```http
POST /api/mail/file HTTP/1.1
Content-Type: multipart/form-data; boundary=-{some-random-boundary}

-{some-random-boundary}
Content-Disposition: form-data; name="file"; filename="document.docx"
Content-Type: application/vnd.openxmlformats-officedocument.wordprocessingml.document

{Binary data of file}
-{some-random-boundary}
Content-Disposition: form-data; name="emails"

elonmusk@x.com,jeffbezos@amazon.com,zuckerberg@meta.com
-{some-random-boundary}--
```

HTTP-ответ:

```http
HTTP/1.1 200 OK
```

### 4. Запустить проект на render.com (опционально)

Запустите вашу API на render.com (аналог heroku).

> 💡 На render.com не снимают денег и не требуют карты при использовании бесплатного тарифа.

После запуска вы получите URL ссылку на свой REST API.

### 5. Запишите видео объяснение вашего проекта

Длительность видео не должна быть более 5 минут. Продемонстрируйте работу вашего проекта и объясните как он работает, расскажите про выбранный подход. Видео можно выложить на YouTube, Google Drive или любой другой видеохостинг.

### 6. Отправить форму

На форму сдачи отправить:
- ссылку на видео (заранее откройте доступ по ссылке на видео)
- ссылку от render.com
- ссылку на репозиторий (заранее откройте доступ)

[ФОРМА СДАЧИ](https://tally.so/r/nGolOp)
