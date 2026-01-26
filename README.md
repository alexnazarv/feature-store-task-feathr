## Result

**Feathr не подходит как Feature Store или как Seamantic Layer**

1. Не поддерживает spark в cluster mode, только local или cloud (Databricks, Azure)
1. Не поддерживает описание датасетов в yaml или json
1. Я не нашел как достать код сконфигурированной feature-store-таблицы в Spark DataFrame API или Spark SQL формате для последующего использования в своей Spark сессиию. Чтобы самому указать настройки материализации, например формат iceberg
1. Не умеет читать iceberg из коробки
1. Единственная опция прод базы для Feature Registry это mssql
1. Работает только на python <= 3.10

## How to reproduce the test
1. Have `Docker` and `git` installed
1. Build feathr docker image
    ```bash
    cd ~ && \
    git clone git@github.com:feathr-ai/feathr.git \
    cd feathr && \
    docker build -f FeathrRegistry.Dockerfile -t feathr-ui-and-registry-v1 . && \
    cd .. && \
    rm -rf feathr
    ```
1. Clone this repo
    ```bash
    git clone git@github.com:alexnazarv/feature-store-task.git
    ```
1. Open this repo in IDE
    ```bash
    code feature-store-task
    ```
1. Run in terminal (for uv)
    ```bash
    uv sync && source .venv/bin/activate
    ```
1. Run docker compose
    ```bash
    docker compose -f docker-compose-minio.yaml up -d
    ```

### Clean up
1. Docker compose down
    ```bash
    docker compose down -v
    ```

## TO DO
1. [x] Сбилдить Feathr Registry & UI в docker image  
1. [x] Сделать docker compose HDFS + Feathr  
1. [!] Возможность использовать Feathr как Semantic Layer  
    1. [!] Получить план запроса feature таблицы из Feathr
    1. [!] Выполнить материализацию, используя план запроса Feathr, в своей spark session  
1. [ ] Регистрация features в Registry после кастомной материализации
1. [ ] Переиспользование features
    1. [ ] Импортировать код зарегистрированных features
    1. [ ] Определить DerivedFeature на импортированных из Registry
    1. [ ] Зарегистрировать новую DerivedFeature в Registry
1. [ ] Возможность описать dataset в yaml или HOCON
1. [ ] Использовать MSSQL как Registry DB
1. [ ] Разделить Registry и UI на два image
1. [x] Сделать тесты на minio без dev container и HDFS

## Additional information

### Run official quickstart
```bash
docker run -it --rm -p 8889:8888 -p 3000:80 -p 7082:7080 -e GRANT_SUDO=yes -v $(pwd)/feathr_data:/home/jovyan/work/product_recommendation feathrfeaturestore/feathr-sandbox:releases-v1.0.0
```

- Feature Registry UI: `localhost:3000`
- Notebook guide: `localhost:8889`

### How to reproduce the test with dev container and hdfs

1. Have `Docker`, `Docker Compose`, and `VS Code` installed
1. Build feathr docker image
    ```bash
    cd ~ && \
    git clone git@github.com:feathr-ai/feathr.git \
    cd feathr && \
    docker build -f FeathrRegistry.Dockerfile -t feathr-ui-and-registry-v1 . && \
    cd .. && \
    rm -rf feathr
    ```
1. Clone this repo
    ```bash
    git clone git@github.com:alexnazarv/feature-store-task.git
    ```
1. Open this repo in VS Code
    ```bash
    code feature-store-task
    ```
1. Type `CMD + Shift + P` and choose `Dev Containers: Rebuild Without Cache and Reopen in Container`
    ![alt text](images/dev_container_rebuild_and_run.png)
1. Run inside Dev Container terminal
    ```bash
    uv sync && source .venv/bin/activate
    ```

### Clean up
1. Close remote connection
1. Docker compose down
    ```bash
    docker compose down -v
    ```
