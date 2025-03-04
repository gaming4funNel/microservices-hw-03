# Домашнее задание к занятию «Микросервисы: подходы»

Вы работаете в крупной компании, которая строит систему на основе микросервисной архитектуры.
Вам как DevOps-специалисту необходимо выдвинуть предложение по организации инфраструктуры для разработки и эксплуатации.


## Задача 1: Обеспечить разработку

Предложите решение для обеспечения процесса разработки: хранение исходного кода, непрерывная интеграция и непрерывная поставка. 
Решение может состоять из одного или нескольких программных продуктов и должно описывать способы и принципы их взаимодействия.

Решение должно соответствовать следующим требованиям:
- облачная система;
- система контроля версий Git;
- репозиторий на каждый сервис;
- запуск сборки по событию из системы контроля версий;
- запуск сборки по кнопке с указанием параметров;
- возможность привязать настройки к каждой сборке;
- возможность создания шаблонов для различных конфигураций сборок;
- возможность безопасного хранения секретных данных (пароли, ключи доступа);
- несколько конфигураций для сборки из одного репозитория;
- кастомные шаги при сборке;
- собственные докер-образы для сборки проектов;
- возможность развернуть агентов сборки на собственных серверах;
- возможность параллельного запуска нескольких сборок;
- возможность параллельного запуска тестов.

Обоснуйте свой выбор.

---

GitLab CI/CD — Основной инструмент для хранения исходного кода и обеспечения процессов CI/CD
GitLab предоставляет полный стек для управления разработкой программного обеспечения, включая систему контроля версий, процессы непрерывной интеграции и поставки, облачное хранилище и поддержку Docker-контейнеров.

Обоснование выбора:
- Облачная система: GitLab предлагает как облачную версию (GitLab.com), так и возможность установки на собственные серверы (GitLab Self-Hosted), что даёт гибкость в зависимости от требований компании.

- Система контроля версий Git: GitLab полностью поддерживает Git как систему контроля версий, что соответствует требованию.

- Репозиторий на каждый сервис: В GitLab можно создать отдельные проекты (репозитории) для каждого микросервиса. Это упрощает организацию кода и позволяет лучше управлять версиями и процессами разработки.

- Запуск сборки по событию из системы контроля версий: В GitLab CI/CD процессы могут быть автоматически запущены при каждом событии из Git (например, при создании коммита, merge request или tag).

- Запуск сборки по кнопке с указанием параметров: GitLab поддерживает возможность ручного запуска пайплайнов с параметрами. Пользователь может выбрать необходимые параметры, такие как ветка, окружение, конфигурация сборки и т.д.

- Настройки для каждой сборки: В GitLab CI/CD для каждого пайплайна можно указать свои настройки через .gitlab-ci.yml файл, который будет специфичен для каждого проекта или конфигурации.

- Создание шаблонов для различных конфигураций сборок: GitLab позволяет создавать шаблоны конфигураций пайплайнов. Эти шаблоны можно переиспользовать в нескольких проектах, что упрощает настройку процессов CI/CD.

- Безопасное хранение секретных данных: GitLab предоставляет встроенный механизм для безопасного хранения секретных данных через Variables. Эти данные шифруются и могут быть доступны в процессе сборки только тем пайплайнам, которые требуют их использования.

- Несколько конфигураций для сборки из одного репозитория: GitLab позволяет создавать различные пайплайны для разных конфигураций сборки. Это может быть реализовано через разные stages или через использование динамических переменных в конфигурации пайплайнов.

- Кастомные шаги при сборке: GitLab CI/CD позволяет настраивать кастомные шаги в пайплайне, что удовлетворяет требованию кастомизации сборки. Можно задавать любые команды или скрипты на каждом этапе пайплайна (build, test, deploy).

- Docker-образы для сборки: GitLab поддерживает Docker. В каждом пайплайне можно указать собственный образ для выполнения сборки, что обеспечивает гибкость для различных проектов.

- Агенты сборки на собственных серверах: GitLab предоставляет возможность развертывания агентов сборки (GitLab Runners) как в облаке, так и на собственных серверах, что дает контроль над инфраструктурой и ресурсами.

- Параллельные сборки и тесты: GitLab поддерживает параллельное выполнение сборок и тестов. Пайплайны могут быть разделены на независимые этапы, которые будут выполняться одновременно, что ускоряет процесс.

---

## Задача 2: Логи

Предложите решение для обеспечения сбора и анализа логов сервисов в микросервисной архитектуре.
Решение может состоять из одного или нескольких программных продуктов и должно описывать способы и принципы их взаимодействия.

Решение должно соответствовать следующим требованиям:
- сбор логов в центральное хранилище со всех хостов, обслуживающих систему;
- минимальные требования к приложениям, сбор логов из stdout;
- гарантированная доставка логов до центрального хранилища;
- обеспечение поиска и фильтрации по записям логов;
- обеспечение пользовательского интерфейса с возможностью предоставления доступа разработчикам для поиска по записям логов;
- возможность дать ссылку на сохранённый поиск по записям логов.

Обоснуйте свой выбор.

--- 

Для обеспечения сбора и анализа логов сервисов в микросервисной архитектуре я предлагаю решение на базе Elastic Stack (ELK), состоящего из Elasticsearch, Logstash, Filebeat и Kibana. Это широко распространённое решение, подходящее для централизованного сбора, хранения и анализа логов.

Компоненты решения:
1. Filebeat — Агент для сбора логов
Filebeat — это лёгкий агент, который устанавливается на каждом хосте для сбора логов и отправки их в центральное хранилище.

Обоснование выбора:
- Сбор логов из stdout: Filebeat легко настраивается для чтения логов, отправляемых в stdout. Это минимизирует требования к приложениям, так как большинство микросервисов и контейнерных решений (Docker, Kubernetes) пишут логи в stdout/stderr.

- Гарантированная доставка логов: Filebeat обеспечивает надёжную доставку логов, используя буферизацию и поддержку backpressure, что гарантирует, что логи не будут потеряны в случае временной недоступности центрального хранилища.

- Лёгкий агент: Filebeat потребляет минимальные ресурсы и может быть легко развёрнут на любом хосте или контейнере, что делает его отличным выбором для микросервисных архитектур.

2. Logstash — Платформа для обработки и агрегации логов
Logstash — это инструмент для централизованной агрегации логов с возможностью их фильтрации, трансформации и нормализации перед отправкой в хранилище.

Обоснование выбора:
- Гибкость обработки логов: Logstash позволяет настроить фильтры для нормализации и обогащения данных из логов. Например, можно разбивать логи на поля, добавлять метаданные (например, имя хоста, имя сервиса), чтобы упростить анализ.

- Поддержка различных форматов данных: Logstash может принимать логи в разных форматах, таких как JSON, Syslog и т.д., что упрощает интеграцию с разными сервисами.

- Интеграция с Filebeat и Elasticsearch: Logstash напрямую принимает данные от Filebeat и передаёт их в Elasticsearch для индексации, обеспечивая эффективную доставку логов в центральное хранилище.

3. Elasticsearch — Централизованное хранилище и поисковая система
Elasticsearch — это распределённая поисковая система и хранилище данных, которая обеспечивает быстрый доступ к большим объёмам логов и поддерживает сложные запросы для анализа.

Обоснование выбора:
- Централизованное хранилище логов: Elasticsearch обеспечивает хранение всех логов в одном месте с распределением данных по кластерам, что гарантирует надёжность и масштабируемость.

- Поиск и фильтрация логов: Elasticsearch поддерживает мощные поисковые и фильтровочные запросы на основе Lucene. Это позволяет разработчикам легко находить нужные записи логов по различным критериям (меткам, времени, сообщениям и т.д.).

- Гарантированная доставка логов: Elasticsearch обеспечивает надёжность хранения данных за счёт распределённой архитектуры с репликацией данных.

---

## Задача 3: Мониторинг

Предложите решение для обеспечения сбора и анализа состояния хостов и сервисов в микросервисной архитектуре.
Решение может состоять из одного или нескольких программных продуктов и должно описывать способы и принципы их взаимодействия.

Решение должно соответствовать следующим требованиям:
- сбор метрик со всех хостов, обслуживающих систему;
- сбор метрик состояния ресурсов хостов: CPU, RAM, HDD, Network;
- сбор метрик потребляемых ресурсов для каждого сервиса: CPU, RAM, HDD, Network;
- сбор метрик, специфичных для каждого сервиса;
- пользовательский интерфейс с возможностью делать запросы и агрегировать информацию;
- пользовательский интерфейс с возможностью настраивать различные панели для отслеживания состояния системы.

Обоснуйте свой выбор.

---

1. Prometheus — Система мониторинга и сбора метрик
Prometheus — это популярное решение для мониторинга, разработанное с учётом микросервисных архитектур. Оно поддерживает сбор метрик с различных хостов и сервисов и является удобным для гибкой настройки.

Обоснование выбора:
- Сбор метрик со всех хостов: Prometheus использует механизм pull, то есть собирает метрики, запрашивая их у хостов и сервисов по HTTP, через определённые endpoints (экспортеры). Это позволяет гибко подключать новые источники данных без необходимости настраивать отправку метрик.

- Сбор метрик состояния ресурсов хостов (CPU, RAM, HDD, Network): Prometheus поддерживает использование различных экспортеров, таких как Node Exporter, который собирает детализированные метрики с физических или виртуальных машин, такие как загрузка процессора, использование оперативной памяти, дискового пространства и сети.

- Сбор метрик потребляемых ресурсов для каждого сервиса (CPU, RAM, HDD, Network): Prometheus может собирать метрики не только с хостов, но и с контейнеров. Например, для контейнеризованных сервисов, запущенных в Docker или Kubernetes, можно использовать cAdvisor или Kubernetes Metrics Server, которые собирают метрики на уровне контейнеров и подов.

- Сбор специфичных метрик для каждого сервиса: В микросервисной архитектуре каждый сервис может предоставлять свои специфичные метрики через HTTP-эндпоинты. Например, с помощью библиотеки Prometheus client разработчики могут настраивать кастомные метрики (количество запросов, время отклика, использование внутренних ресурсов и т.д.), которые будут собираться Prometheus.

- Гибкая система оповещений: В дополнение к сбору метрик Prometheus поддерживает создание Alert Rules, которые позволяют настраивать автоматические уведомления о критических состояниях системы. Например, можно настроить предупреждения о высоком использовании CPU или RAM, сбоях в работе сервисов и других аномалиях.

2. Grafana — Система визуализации метрик
Grafana — мощная платформа для визуализации метрик и создания пользовательских панелей управления (dashboards). Она тесно интегрируется с Prometheus и другими источниками данных.

Обоснование выбора:
- Пользовательский интерфейс с запросами и агрегированием информации: Grafana предоставляет гибкий интерфейс для создания запросов к различным источникам данных, включая Prometheus. Пользователи могут легко агрегировать данные, настраивать фильтры, применять различные преобразования и визуализировать результаты.

- Пользовательский интерфейс с возможностью настраивать различные панели: Grafana позволяет настраивать панель управления для отслеживания метрик состояния системы в реальном времени. Пользователь может самостоятельно настраивать различные панели для разных аспектов системы — метрики ресурсов хостов, производительность отдельных микросервисов, метрики на уровне контейнеров и т.д.

- Широкий набор визуализаций: Grafana поддерживает различные типы визуализаций — графики, гистограммы, диаграммы, таблицы и т.д. Это позволяет гибко представлять данные и делать их более понятными для анализа.

- Настраиваемые дашборды: В Grafana можно создавать динамические дашборды, которые позволяют пользователям легко менять параметры отображения метрик в реальном времени (например, выбор временных интервалов, фильтрация по сервисам или хостам).

- Поддержка оповещений: Grafana может интегрироваться с Prometheus Alertmanager или использовать собственные механизмы оповещений, отправляя уведомления по электронной почте, в Slack или другие системы при превышении критических значений метрик.

---

## Задача 4: Логи * (необязательная)

Продолжить работу по задаче API Gateway: сервисы, используемые в задаче, пишут логи в stdout. 

Добавить в систему сервисы для сбора логов Vector + ElasticSearch + Kibana со всех сервисов, обеспечивающих работу API.

### Результат выполнения: 

docker compose файл, запустив который можно перейти по адресу http://localhost:8081, по которому доступна Kibana.
Логин в Kibana должен быть admin, пароль qwerty123456.


## Задача 5: Мониторинг * (необязательная)

Продолжить работу по задаче API Gateway: сервисы, используемые в задаче, предоставляют набор метрик в формате prometheus:

- сервис security по адресу /metrics,
- сервис uploader по адресу /metrics,
- сервис storage (minio) по адресу /minio/v2/metrics/cluster.

Добавить в систему сервисы для сбора метрик (Prometheus и Grafana) со всех сервисов, обеспечивающих работу API.
Построить в Graphana dashboard, показывающий распределение запросов по сервисам.

### Результат выполнения: 

docker compose файл, запустив который можно перейти по адресу http://localhost:8081, по которому доступна Grafana с настроенным Dashboard.
Логин в Grafana должен быть admin, пароль qwerty123456.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
