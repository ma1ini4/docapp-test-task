# Тестовое задание Docapp Front-End Developer

Это стартовый проект для выполнения тестового задания для Docapp Front-End.

## Подготовка

 - Убедитесь, что в вашей системе установлен node.js последней стабильной версии - https://nodejs.org/en/
 - Сделайте fork проекта; подробную информацию можно получить здесь:
 https://help.github.com/articles/about-forks/
 - Склонируйте репозиторий своего форка:
```sh
git clone https://github.com/<username>/docapp-test-task.git
```
 - Перейдите в корневую директорию проекта и установите зависимости  командой (подробно об npm и зависимостях можно узнать на сайте: https://docs.npmjs.com/):
```sh
npm install
```
 - Для запуска проекта выполните команду:
```sh
npm start
```

## Задание 1

### `rooms.json`

В файле `rooms.json` хранится сериализованный в JSON массив комнат, каждый из которых соответствует следующей схеме:

```javascript
{
    "code": 2, // Номер комнаты
    "appointment": {
      "code": "IMPL", // Код appointment'a
      "first_name": "Michael", // Имя пациента
      "last_name": "Kaminsky", // Фамилия пациента
      "gender": "Male", // Пол
      "birthday": "03/21/1990", // Дата рождения
      "start_date": "06/13/2019", // Дата appointment'a
      "start_time": "14:00", // Время appointment'a
      "doctor_title": "Dr. Henrietta Jacoby", // Имя доктора
      "assistant": "Slavica Julien", // Имя ассистента
      "is_doctor": true, // Является ли пациент доктором
      "vital_signs": {
              "height_ft": "6", // Рост в футах
              "height_in": "02", // Рост в дюймах
              "weight": "190", // Вес в фунтах
              "bmi": "22.9" // Индекс массы тела
            }
    },
    "status": {
      "code": "room_occupied", // Код статуса комнаты
      "title": "Room Occupied" // user-friendly title статуса комнаты
    },
    "update_time": 1530215877209 // Дата последнего изменения (для подсчета времени ожидания)
}
```

Необходимо реализовать вывод комнат на странице нашего приложения. Каким образом это будет сделано - неважно. Таблица, блоки, список - креативность приветствуется.

Каждая комната должна отображать:
- Номер комнаты + код appointment'а (ex. Room 1 (3CON)).
- Имя и фамилию пациента (first_name, last_name)
- Статус комнаты (status.title)
- Время appointment'a в формате hh:mm A **(04:30 PM)**
- Время ожидания в формате HH:mm:SS (обновление в реальном времени будет плюсом) **(00:32:46)**
- Имя доктора **(D: Dr. Dave Brown)**
- Имя ассистента **(A: Nancy Robinson)**

## Задание 2

При нажатии на комнату необходимо отобразить детали appointment'a (ну и, немножечко комнаты):
- Номер комнаты **(Room 1)**
- Дата appointment'a в формате MM/DD/YYYY **(02/22/1897)**
- Vital signs: рост, вес, индекс массы тела (BMI) **(HT: 5'2'', WT: 180lbs., BMI: 21.95)**
- Имя, фамилия, возраст, пол пациента с обращением (Mr. если Male в поле gender, иначе Ms. Однако, если в поле is_doctor стоит true - обращение должно быть Dr.) **(ex: Dr. Charles Green, 47 years, M)**

Опять же, дизайн в данном случае не первостепенен, важна реализация. Располагайте элементы, как подсказывает сердце :)

## Задание 3

### `consent-forms.json`
В файле `consent-forms.json` хранится сериализованный в JSON массив списка форм типа:

```javascript
{
    "id": 1, // Порядковый номер формы
    "code": "implant", // Код формы
    "title": "Consent for Dental Implant Surgery", // Полное название формы
    "short_title": "Implant" // Короткое название формы
  }
```

На этой же странице деталей пациента необходимо отобразить список форм вида 
 
 ☑ Oral And Maxillofacial Surgery Consent
 
 ☑ Consent for Dental Implant Surgery

С чекбоксами и возможностью выбирать каждую из форм.

- Плюсом будет добавление функционала Select/Deselect all forms

Далее, при выборе одной или нескольких форм - необходимо показывать кнопку Sign. Эта кнопка не должна отображаться, если ни одна форма не была выбрана.


## Задание 4

### `consent-form-details`

И, наконец, в файлах `consent-form-details/{form_code}.json` хранится сериализованный в JSON массив с деталями каждой формы.

```javascript
{
    "content": "Nam et ex ac erat mattis euismod sit amet malesuada libero.", // Собственно, контент внутри li
    "need_initials": true // Флаг о необходимости проставить инициалы
  },
```

При нажатии на кнопку Sign внизу страницы необходимо отобразить табы с выбранными формами. Например, можно использовать Bootstrap tabs. По умолчанию, первый таб должен быть открыт. Заголовком таба будет служить переменная short_title. Контент - необходимо брать соответственно из файла `consent-form-details/{form_name}.json`. Информация для контента должна подгружаться только при нажатии на таб.

Контентом каждой формы будет являться несортированный список `ul` полей, загруженных из json'a. Для каждого поля, имеющего переменную need_initials, необходимо отобразить checkbox в начале строки. По нажатию на него, checkbox скрывается, вместо него необходимо отобразить инициалы пациента

Например, представим, что пациента зовут John Smith:

**До нажатия на checkbox:**

▢ I certify that I read and write English and have read and fully understand this consent for surgery.  

**После:**

**JS** I certify that I read and write English and have read and fully understand this consent for surgery.  

 
## Как выполнять задания

Вывод данных должен быть организован в html-элемент `div` с идентификатором `app` в файле `src/index.html`

Структура приложения - на ваше усмотрение.

> Задание может быть решено на чистом JavaScript (можно использовать ES2015+). Если вы хотите выполнить тестовое задание с использованием фреймворка - опишите, почему было решено использовать именно его.

> Нельзя модифицировать файлы с исходными данными (`data/consent-forms.json`, `data/rooms.json`, `consent-forms-details/*.json`), секцию `output` из `webpack.config.js`, а также команды запуска проекта из секции `scripts` файла `package.json`.

> Для реализации CSS-стилей можно использовать любой css-фреймворк - например, [Bootstrap](https://getbootstrap.com/). Использование препроцессоров sass, less - будет плюсом

> Организовать асинхронную загрузку данных посредством вызовов к API, которое будет доступно по адресу `http://localhost:9000/api/{file_name}` после запуска проекта - будет плюсом

