# Telegram
# 1. Тема и целевая аудитория
### Тема
Мессенджер **Telegram** - кроссплатформенная система мгновенного обмена сообщениями, позволяющая обмениваться текстовыми, голосовыми и видеосообщениями, стикерами и фотографиями, файлами многих форматов.
### Функционал MVP
- регистрация по номеру телефона
- отправка текстовых сообщений
- отправка голосовых сообщений
- отправка фотографий
- список чатов
- история сообщений
### Целевая аудитория
Ниже представлена статистика исследования аудитории Telegram. В опросе приняли участие жители из более чем 70 стран.
Возраст целевой аудитории варьируется в диапазоне от 18 до 64 лет [[1]](https://tgstat.ru/research-2021)
Возрастная группа | Процентное соотношение
------------ | -------------
до 17 лет | 7,7%
18-24 года| 21,9% 
25-34 года| 30,6% 
35-44 года| 21,3% 
45-64 года| 18,5% 

Размер целевой аудитории: 550 млн. пользователей в месяц, 55 млн. пользователей в день. [[2]](https://www.demandsage.com/telegram-statistics/)

Общемировая география пользователей [[3]](https://www.businessofapps.com/data/telegram-statistics/):  
Регион           | Процентное соотношение
---------------- | -------------
Азия             | 38%
Европа           | 27% 
Латинская Америка| 21% 
страны MENA      | 8% 

География русскоязычных пользователей:
Страна       | Процентное соотношение
-------------| -------------
Россия       | 60%
Беларусь     | 17% 
Украина      | 13% 

# 2. Расчёт нагрузки

### Продуктовые метрики
Ежемесячная аудитория Telegram: 550 млн. пользователей.

Ежедневная аудитория Telegram: 55 млн. пользователей.

Исходя из исследования [[4]](https://siteefy.com/telegram-statistics/):

Количество отправленных сообщений в день: 15 миллиардов

Количество регистраций в день: 1.5 млн.

Средняя длительность сессии: 6 минут.

Исходя из исследования [[5]](https://www.tadviser.ru/index.php/Статья:Мессенджеры_(Instant_Messenger,_IM)#:~:text=Как%20выяснилось%2C%20в%20среднем%20пользователь,идут%20Viber%2C%20Telegram%20и%20Skype.):

Количество заходов в день: 24 

#### Хранилище данных для пользователя
Посчитаем количество сообщений в день на одного пользователя : 15 млрд. / 55 млн. = 272

Пусть:
  - Из 272 сообщений 5 будут голосовыми и 5 будут фотографиями.
  - Средняя длина одного сообщения равна 60 символам в кодировки UTF-8, где 1 символ равен 1 байту. [[6]](https://cyberleninka.ru/article/n/poliformatnyy-messendzher-kak-zhanr-2-0-na-primere-messendzhera-mgnovennyh-soobscheniy-telegram/viewer)
  - Средняя длина записи одного голосового сообщения равна 60 секундам, битрейт равен 128 kbps, тогда размер будет равен 480 кБ.
  - Средний размер одной фотографии из сообщений равен 256 кБ.
  - Сообщения хранятся 5 лет.
  - Среднее количество чатов у одного пользователя равно 15
  - Количество единовременно подгружаемых сообщений в диалоге равно 10
  - Среднее количество людей, с которыми пользователь общается в день равно 8
  - Размер сообщения в превью чата равен 30 символам
  - Название чата равно 20 символам

Чат в списке чатов состоит из аватарки чата(100 кБ), последнего сообщения (превью), названия чата.
Тогда размер одного чата равен: 100 кБ + 30 * 0.03(кБ) + 20 * 0.02(кБ) = 101.3 кБ

Тогда общий объем **сообщений**, отправляемых в день, на одного пользователя равен: 262 * 0.06 (кБ) + 5 * 480 (кБ) + 5 * 100 (кБ) = 2916 кБ

Средний размер **профиля** пользователя составляет 100 кБ (аватарка + данные о пользователе(номер телефона, уникальный идентификатор, имя).

Суммарный объем данных на одного пользователя за 5 лет: 2916(кБ) * 365 * 5 + 256 (кБ) = 5.1 гБ

#### Динамический рост

Так как в день отправляется около 15 млрд. сообщений, то можно вычислить, какой объем данных будет занят пользователями за, например, 1 год.

Расчет для фотографий и голосовых сообщений: (5 * 480 (кБ) +  5 * 100 (кБ)) * 55 млн. *  365 = 59.95 ПБ / год
Расчет для текстовых сообщений: 262 * 0.06 (кБ) * 55 000 000 *  365 = 0.29 ПБ/год

#### Сетевой трафик
При расчете сетевого трафика не будем учитывать запросы, связанные с регистрацией пользователей, так как они не создают ощутимую нагрузку на наш сервис.
Основная нагрузка приходится на отправку сообщений, загрузку списка чатов и историю сообщений.

##### Предварительные расчеты:
Посчитаем объем трафика для списка чатов на одного человека с учетом количества заходов в сутки: 24 * 15 * 101.3 кБ = 36 468 кБ
Посчитаем объем трафика для истории сообщений на одного человека с учетом  количества единовременно подгружаемых сообщений в диалоге, среднего количества людей, с которыми общается пользователь в день: 8 * 10 * 0.06 (кБ) = 4.8 кБ

Рассмотрим трафик по видам активностей:
    Тип          | Отправка (дневная аудитория 55 млн) | Отправка Гб/сек |        
   ------------- |--------------------------------------|-------------------|
   Текстовое     | 262 * 55 млн. * 0. 06 кБ             | 0.076            |
   Голосовое     | 5 * 55 млн. * 480 кБ                 | 11.6              | 
   Фотография    | 5 * 55 млн. * 256 кБ                | 6.2              |
   Список чатов  | 36 468 кБ * 55 млн.                  | 177.1|
   История сообщений  | 4.8 кБ * 55 млн.                | 0.023             |
   Итого  |                                             | 195           |


#### RPS
 
 - Отправка сообщения: 55 млн * 272 / 86400 = 173 148 RPS
 - Регистрация: 1.5 млн / 86400 = 17 RPS
 - Авторизация: 55 млн. / 86400 = 637 RPS
 - Получение списка чатов: 55 млн. * 24 / 86400 = 15 278 RPS
 - Получение истории сообщений: 55 млн. * 8 / 86400 = 5 093 RPS

   Действие                            | RPS
   ------------------------------------| ---
   Отправка сообщения                  | 173 148
   Регистрация                         | 17
   Авторизация                         | 637
   Получение списка чатов              | 15 278
   История сообщений                   | 5 093
   **Итого**                           | 194 173
#### Пиковая нагрузка

**Пиковая нагрузка для RPS**

Возьмем за основу расчетов пиковой нагрузки метрики реального сервиса, предоставленные моим ментором.
Перед началом расчета для пиковой нагрузки я воспользуюсь данными, предоставленными моим ментором.

На двух графиках изображены недельные и суточные метрики сервиса https://pulse.mail.ru. 


![image](https://user-images.githubusercontent.com/29610387/201543002-2139e6a6-27fb-4078-815c-6a924edc7036.png)

График за месяц

Как видно из недельного графика, поведение пользователей предсказуемо и циклично, из чего можно сделать вывод, что суточные метрики будут повторяться изо дня в день. Кроме того, исходя из специфики самого сервиса, можно сделать вывод, что активность пользователей будет похожей примерно в одно и то же время (ночью падать, к концу первой половины дня доходить до пика, в течение дня держаться на среднем уровне, а после к ночи постепенно падать до минимального значения).

![image](https://user-images.githubusercontent.com/29610387/201480449-f836807a-7ee7-493f-815e-3a7595c1b40e.png)

График за неделю

На суточном графике видно, что максимальное значение RPS примерно равно 9200, минимальное значение равно примерно 1400, а среднее значение примерно 5700.

Если использовать соотношения значений из предоставленного графика для расчета пиковой нагрузки в нашем сервисе, то получаем:

Средний RPS равен 194 173, тогда пиковый RPS равен 194 173 * 1.6 = 310 677, минимальный RPS равен 194 173 * 0.24 = 46 601

**Пиковая нагрузка для сетевого трафика**

Если учесть, что RPS повысилось в 1.6 раз выше среднего, то объем сетевого трафика также увеличится в 1.6 раз.
Итоговая таблица с значениями пиковых нагрузок с добавлением коэффициента запаса:
    Тип                 | Пиковое значение|Пиковое значение с коэффициентом запаса 2         |        
   ---------------------|-----------------|--------------------------------------------------|
   RPS                  | 310 677         | 621 354                                          |
   Сетевой трафик(Гб/c) | 312             | 624                                              | 
   
   
# 3. Логическая схема БД
![image](https://user-images.githubusercontent.com/29610387/202914740-93adecde-d82f-4c7f-a57a-06e6586b2d65.png)


# 4. Физическая схема БД
![image](https://user-images.githubusercontent.com/29610387/202914856-9bb55947-f1c3-4c34-be39-4832174c4db7.png)

Для хранения основных данных (чатов, сообщений и пользователей) используется база данных PostgreSQL, так как она является одной из наиболее функциональных, производительных и широко распространённых реляционных БД.
Так как один сервер PostgreSQL не выдержит планируемую нагрузку, выполним шардинг таблицы сообщений. Шардинг таблицы сообщений будем выполнять по полю chat_id.
Для более быстрого доступа к данным будет необходимо использовать индексы. Для таблицы Message индекс будет создан по полям chat_id.
Для таблиц Chat и User - по полю id.
Также можно разбивать данные на партиции по полу type в таблице Attaches.

Для хранения информации о сессиях используется база данных Redis по причине того, что она осуществляет хранение данных in-memory, имеет поддержку неблокирующей репликации master-slave и возможность организации кластера Redis cluster.

Для хранения очереди непрочитанных сообщений используется Redis. Для каждого user_id сохраняется список, содержащий информацию о сообщениях, ожидающих получения. При запросе дельты сообщений список очищается. При отправке сообщения пользователю, не находящемуся в сети, оно добавляется в список.

Для хранения файлов будет использоваться сервер с большим объемом памяти.

# 5. Выбор технологий

| Технология | Область применения                        | Мотивационная часть                                          |
| ---------- | ----------------------------------------- | ------------------------------------------------------------ |
| TypeScript | Frontend.                                 | Типизация                                                |
| React      | Библиотека Frontend                       | Библиотека с компонентным подходом для создания пользовательского интерфейса, имеет свою реализацию Virtual DOM, повыщающую производительность приложения. Благодаря высокой популярности библиотека найти разработчиков будет относительно легко.  |
| Nginx      | Web-server<br />Reverse proxy             | Быстрый веб-сервер, простая настройка                        |
| Go     |                   Backend                 | Быстро писать Backend, дает достаточно хорошую производительность на больших нагрузках. Имеет множество готовых библиотек, упрощающих жизнь разработчиков и ускоряющих процесс реализации, а также имеет встроенные горутины и каналы, которые позволяют эффективно распараллеливать производимые вычисления.
| Redis      | Сессии   и брокер сообщений                                 | In-memory БД позволяет обеспечить высокую скорость на чтение и запись |
| PostgreSQL    | Хранение пользователей, сообщений и списка его чатов | Отлично подходит для  большого объема данных, поддерживает репликацию.  |
|Amazon S3 |	Файловое хранилище	| Высокая надежность, доступность, производительностью и безопасность

# 6. Схема проекта
Балансировка: DNS и L7-балансировка Nginx.

Chat service - основной сервис, обрабатывает создание чатов, подгрузку списка чатов.

Auth service - сервис авторизации, обрабатывает запросы, связанные с авторизацией пользователей.

Message serice - сервис сообщений.

Record service - сервис, позволяющий записывать данные в базу.

Read service - сервис, позволяющий читать данные из базы.

![image](https://user-images.githubusercontent.com/29610387/202914876-67348e4b-9b98-4ed4-9e5c-c5902dccdf37.png)

# 7. Список серверов
Наш сервис в пике имеет 621 354 RPS и 624 Гб/с сетевого трафика. 

Произведем расчёт конфигурации для **Nginx**:

Согласно [[7]](https://www.nginx.com/blog/testing-the-performance-of-nginx-and-nginx-plus-web-servers/) видно, что сервер с 16 CPU способен выдержать 77 427 HTTPS PRS. Тогда, учитывая нашу пиковую нагрузку, посчитаем количество серверов: 621 354 / 77 427 ~ 9 серверов.

Учитывая объем сетевого трафика, посчитаем количество серверов с учетом сетевого трафика: 624 / 40 = 16

| Параметр   | Значение |
| :----:     |      :---|
| CPU        | 16 ядер |
| Network    | 40 Гбит/с |
| Количество | 16 |

**Микросервисы**:

Согласно [[8]](https://github.com/smallnest/go-web-framework-benchmark) сервер с данной конфигурацией выдерживает 30 000 RPS. 
30 000 RPS - это идеальная ситуация, в которой сервис, обрабатывая запросы, не теряет время на походы по сети, походы в базу данных. Если учесть эти факторы, обратившись, например, к [[9]](https://www.ashnik.com/fine-tuning-postgres-to-achieve-5000-queries-per-second/), то можно ограничить верхний предел обработки микросервисами до 5 000 RPS.

CPU: KVM Virtual CPU version(2 GHz, 4 cores)
Memory: 16G
Go: go1.18.5 linux/amd64
OS: Ubuntu 22.04.1 LTS with Kernel 5.15.0-41-generic

Тогда необходимо будет: 621 354 / 5 000 = 124 сервера

***Базы данных***

**Amazon S3**

Один бакет может хранить до 5 Тб [[10]](https://cloudian.com/blog/s3-storage-behind-the-scenes/), то, с учетом динамического роста данных за счет фотографий и голосовых сообщений из пункта 2, получим:  59.95 ПБ / 5 ТБ = 12 278 бакетов в год для хранения новых данных.

**PostgreSQL**
С учетом динамического роста данных за счет текстовых сообщений из пункта 2, придется каждый год докупать новые диски для хранения 0.29 ПБ.

PostgreSQL способен выдавать около 40 000 TPS [[11]](https://www.percona.com/blog/2017/01/06/millions-queries-per-second-postgresql-and-mysql-peaceful-battle-at-modern-demanding-workloads/).

Пусть будет 1 master и 2 slave, а также их реплики.

| CPU | RAM (ГБ) | Диск (ТБ) | Количество |
|-|-|-|-|
| 24 | 512 | 5 | 6 |

**Redis** 
Согласно официальному сайту Redis, один сервер способен выдержать 50 000 TPS [[12]](https://redis.io/docs/management/optimization/benchmarks/).

Является in-memory хранилищем, следовательно требуется большой объём оперативной памяти.

Ежедневный онлайн составляет около 55 000 000 пользователей. Если учесть, что все они будут одновременно пользоваться нашим сервисом, то потребуется 
token(64 байта) + id (4 байта) + user_id (4 байта) = 72 байта на одного пользователя. Тогда для 55 000 000: 3.68 гБ. Для более эффективной обработки и повышения надежности можно сделать 1 master и 2 slave сервера с репликами. Всего 6 серверов.

| Параметр | Значение|
| :-: | :-: |
| CPU (cores) | 2 |
| RAM (Gb) | 32 |
| HDD (Gb) | 20 |



# 8. Ссылки на источники
1. https://tgstat.ru/research-2021
2. https://www.demandsage.com/telegram-statistics/
3. https://www.businessofapps.com/data/telegram-statistics/
4. https://siteefy.com/telegram-statistics/
5. https://www.tadviser.ru/index.php/Статья:Мессенджеры_(Instant_Messenger,_IM)#:~:text=Как%20выяснилось%2C%20в%20среднем%20пользователь,идут%20Viber%2C%20Telegram%20и%20Skype.
6. https://cyberleninka.ru/article/n/poliformatnyy-messendzher-kak-zhanr-2-0-na-primere-messendzhera-mgnovennyh-soobscheniy-telegram/viewer
7. https://www.nginx.com/blog/testing-the-performance-of-nginx-and-nginx-plus-web-servers/
8. https://github.com/smallnest/go-web-framework-benchmark
9. https://cloudian.com/blog/s3-storage-behind-the-scenes/
10. https://www.ashnik.com/fine-tuning-postgres-to-achieve-5000-queries-per-second/
11. https://www.percona.com/blog/2017/01/06/millions-queries-per-second-postgresql-and-mysql-peaceful-battle-at-modern-demanding-workloads/
12. https://redis.io/docs/management/optimization/benchmarks/
