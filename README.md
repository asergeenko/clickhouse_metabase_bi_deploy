# Развертывание BI-решения
## Подключение Clickhouse к Metabase

Я воспользовался pre-release jar драйвера Clickhouse из [официального репозитория](https://github.com/enqueue/metabase-clickhouse-driver) и подключил к docker-конейнеру согласно [инструкции](https://github.com/enqueue/metabase-clickhouse-driver#mount-plugins-directory).

## Набор данных

В качестве набора данных я использовал [анонимизированными данными Яндекс-Метрики](https://clickhouse.com/docs/ru/getting-started/example-datasets/metrica/), который доступен через JDBC на сервере [Clickhouse Playground](https://play.clickhouse.com/)

### Настройка подключения

    host: play-api.clickhouse.com *(play-api.clickhouse.tech, указанный в инструкции, не работает)*
    port: 8443
    database: datasets
    user: playground
    password: clickhouse
    Включить SSL

### Описание набора данных

**hits** - все действия, выполняемые всеми пользователями на всех веб-сайтах, охватываемых сервисом Яндекс.Метрика (около 10 дней в марте 2014 года)

**visits** - вместо отдельных действий содежит предварительно созданные сеансы

### Работа с Metabase

Попробуем простой запрос и bar chart по полю **IsMobile** таблицы **visits**.

<img src="https://github.com/asergeenko/clickhouse_metabase_bi_deploy/blob/main/screenshots/visits_ismobile.jpg?raw=true" alt="Визиты с мобильных устройств" />

Как видно, подавляющее большинство действий совершается с настольных устройств (данные за март 2014 года).

Теперь воспользуемся кнопокой **Summarize** в интерфейсе Metabase и посмотрим на количество сеансов в зависимости от времени начала сеанса **StartTime** (сгруппируем по часу):

<img src="https://github.com/asergeenko/clickhouse_metabase_bi_deploy/blob/main/screenshots/visits_start_time_summarize.jpg?raw=true" alt="Час начала сессии" />

На кнопках столбцов в интерфейсе таблиц помимо сортировки и фильтрации можно, к примеру, построить распределение по какому-то полю. Сделаем это для продолжительности сессии (**Duration**):

<img src="https://github.com/asergeenko/clickhouse_metabase_bi_deploy/blob/main/screenshots/visits_duration_distribution.jpg?raw=true" alt="Продолжительность сессии" />

Те же данные в виде сводной таблицы (**pivot table**):

<img src="https://github.com/asergeenko/clickhouse_metabase_bi_deploy/blob/main/screenshots/visits_duration_pivot.jpg?raw=true" alt="Продолжительность сессии (сводная таблица)" />

В заключение, посмотрим на зависимость продолжительности сессии от возраста и пола:

<img src="https://github.com/asergeenko/clickhouse_metabase_bi_deploy/blob/main/screenshots/visits_age_sex_duration.jpg?raw=true" alt="Зависимость продолжительности сессии от возраста и пола" />

Очевидно, что возраст 0 означает неуказанный или неизвестный возраст.


