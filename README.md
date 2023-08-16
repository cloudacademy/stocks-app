# Stocks App
Microservices Stock Application

![Stocks App](/docs/stocks.png)

## Build Instructions

1. Download dependencies:

    ```
    yarn install
    ```

2. Build UI package:

    ```
    yarn build
    ```

3. Build UI container image:

    ```
    docker build -t cloudacademydevops/stocks-app:v2 .
    ```

## Container Launch Instructions

The UI container image can be launched using the following commands:

- Starts the container instance with Nginx proxy_pass enabled for the `/api/stocks/csv` path, useful for demonstrating service discovery on downstream components etc:

    ```
    docker run --rm -it --name stock-app --env NGINX_APP_APIHOSTPORT=api.cloudacademy.terraform.local:8080 -p 8080:8080 cloudacademydevops/stocks-app:v2
    ```

- Starts the container instance **without** the Nginx proxy_pass enabled for the `/api/stocks/csv` path:

    ```
    docker run --rm -it --name stock-app -p 8080:8080 cloudacademydevops/stocks-app:v2
    ```
