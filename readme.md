# Genesis Software Engineering School 2: BTC Project
## Опис завдання
Потрібно реалізувати сервіс з АРІ, який дозволить:
* дізнатись поточний курс біткоіну (BTC) у гривні (UAH);
* підписати емейл на отримання інформації по зміні курсу;
* запит, який відправить всім підписаним користувачам актуальний курс.

## Додаткові вимоги
1. Сервіс має відповідати описаному АРІ. Сам АРІ описаний тут у вигляді 
   swagger документації. Для зручного перегляду можна скористатися 
   сервісом https://editor.swagger.io/.
2. Закривати рішення аутентифікацією не потрібно.
3. **Всі дані, для роботи додатку повинні зберігатися в файлах
   (підключати базу даних не потрібно)**. Тобто, потрібно реалізувати
   збереження та роботу з даними (наприклад, електронними
   адресами) через файлову систему.
4. **В репозиторії повинен бути Dockerfile, який дозволяє запустити
   систему в Docker**. З матеріалом по Docker вам необхідно
   ознайомитись самостійно.
5. Документацію потрібно дотримуватись повною мірою, тому
   змінювати контракти не можна.
6. Можна використовувати релевантні фреймворки.
7. Також ти можеш додавати коментарі чи опис логіки виконання
   роботи в README.md документі. Правильна логіка може стати
   перевагою при оцінюванні, якщо ти не повністю виконаєш
   завдання.
8. **Очікувані мови виконання завдання: PHP, Go**. Виконувати 
   завдання іншими мовами можна, проте, це не буде перевагою.

## Використані засоби
1. Docker
2. PHP + Composer + Laravel
3. BitPay API - https://bitpay.com/api/
4. Mailtrap - https://mailtrap.io/

## Опис технічних деталей
* Вся конфігурація проекту знаходиться у файлі .env
* app/Utils/FileDatabase.php - клас, в якому реалізована робота з файловою базою даних. 
  В моїй реалізації - файлова БД це не один файл, а група файлів, розбитих за розміром FILE_DB_PART_SIZE.
* Для прискорення виконання запитів та зменшення навантаження на SMTP сервер - надсилаються групові EMAIL
  з максимальною кількістю отримувачів в одному листі MAIL_OFFSET

## Процес встановлення
Для встановлення і ініціалізації - виконати команди:

    git clone https://github.com/genesis-tmp/gses2-shool-btc.git
    cd gses2-shool-btc
    composer update

Після успішного виконання налаштувати файл .env

Для запуску Docker контейнера:

    sudo docker-compose up -d --build

## Використання сервісу

Для отримання поточного курсу біткоїна:

    curl -X 'GET' \
    'http://localhost/api/rate' \
    -H 'accept: application/json' -i

Для підписання емейлу на сервіс:

    curl -X 'POST' \
    'http://localhost/api/subscribe' \
    -H 'accept: application/json' \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'email=test%40gmail.com' -i

Для відправки актуального курсу всім підписаним користувачам:

    curl -X 'POST' \
    'http://localhost/api/sendEmails' \
    -H 'accept: application/json' \
    -d '' -i
