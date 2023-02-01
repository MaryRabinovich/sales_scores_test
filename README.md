# Задача

В этой задаче необходимо автоматизировать начисление скидочных баллов по мере поступления заказов и их изменений. При оценке результата в первую очередь учитывается понимание экосистемы Laravel.

В экосистеме Laravel

1) Развернуть laravel

2) Создать route c адресом /api/insales/scores/update

3) На маршрут приходит post-запрос с заказом в json-формате 

$request = [
    'id' => 12345,
    'client_id' => 54321
    'items' => [
        [
            'article' => '3005-12',
            'name' => "Сосиська в тесте",
            'price' => 100,
            'quantity' => 12
        ],
        [
            'article' => '3005-13',
            'name' => "Дырка от бублика",
            'price' => 340,
            'quantity' => 3
        ],
        [
            'article' => '3005-14',
            'name' => "Усы Фредди Меркьюри",
            'price' => 900,
            'quantity' => 90
        ],
    ],
    'status' => 'new' 
]; 

4) Если в заказе есть скидочный товар "Дырка от бублика" (3005-13), то вернуть json 

$response = [
    'client_id' => ...,
    'scores' => //3 балла с каждой единицы скидочного товара,
    'order_id' => ...
]; 

5) Если в заказе нет скидочного товара, то поле scores = 0 

6) $response и $request сохранять в бд MySQL. В случае, если заказ и его баллы были уже созданы в БД, то обновлять их. 

7) Написать консольную команду, которая раз в 30 минут собирает все заказы со статусом 'accepted' и меняет на статус 'shipping' 

8) Свой вариант залить на github и прислать ссылку проверяющему

# Запуск проекта на выполнение в браузере

Впишите данные вашей БД в файл .env

Зайдите в папку проекта в консоли, установите необходимые пакеты РНР и запустите сервер:

`composer install`

`php artisan serve`

# Запуск тестов

Тесты работают в памяти в БД SQLite, дополнительных настроек БД не требуется

Установите пакеты РНР и запустите тесты:

`composer install`

`php artisan test`

# Комментарии к решению

### Скидки

Список артикулов и поштучные баллы (возможно, различные для разных артикулов) было бы логично задавать отдельной таблицей sales в БД. 

Однако из задания было не ясно, надо ли сосредотачиваться на возможности изменения списка скидок. Соответственно, в файле app/Services/ScoresCalculator.php набор скидок реализован хардкодом. 

В нынешней версии скидка действует ровно на один артикул, это 3005-13 (товар "Дырка от бублика") из пункта 4 задания. Поштучная скидка на дырку от бублика равна 3.

*Добавила таблицу sales. Сидер DatabaseSeeder создаёт в ней одну запись - артикул Дырки от бублика 3005-13 и соответствующие 3 поинта*