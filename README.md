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

**hits** - все действия, выполняемые всеми пользователями на всех веб-сайтах, охватываемых сервисом Яндекс.Метрика

**visits** - вместо отдельных действий содежит предварительно созданные сеансы

### Работа с Metabase

Попробуем простой запрос и bar chart по полю **IsMobile** таблицы **visits**.

<img src="https://github.com/asergeenko/clickhouse_metabase_bi_deploy/blob/main/screenshots/visits_ismobile.jpg?raw=true" alt="Визиты с мобильных устройств" />

Как видно, подавляющее большинство действий совершается с настольных устройств.

Теперь воспользуемся кнопокой **Summarize** в интерфейсе Metabase и посмотрим на количество сеансов в зависимости от времени начала сеанса **StartTime** (сгруппируем по часу):

<img src="https://github.com/asergeenko/clickhouse_metabase_bi_deploy/blob/main/screenshots/visits_start_time_summarize.jpg?raw=true" alt="Визиты с мобильных устройств" />
