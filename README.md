# kubequest application

The kubequest application is a basic PHP laravel 8 application with a minor version greater than 75.

It is exposed on port 80, follow an Model View Controller (MVC) standard architectural pattern and interacts with a Mysql database storage.

The application has been containerized using Docker and a docker-compose helps us run the application with its mysql database service in development.

## Environnement variables

-   APP_DEBUG: laravel application enable dbugging
-   APP_ENV: laravel application environnement
-   APP_KEY: laravel application encryption key: Used for Encrypt and decrypt data using the encrypt() and decrypt() functions, Generate secure, random strings using the Str::random() function, Sign and verify data using the hash_hmac() function, Generate unique authentication tokens using the Hash::make() function.
-   DB_CONNECTION: laravel application database connection type
-   DB_HOST: laravel application database connection host
-   DB_PORT: laravel application database connection port
-   DB_DATABASE: laravel application database connection database name
-   DB_USERNAME: laravel application database connection database user
-   DB_PASSWORD: laravel application database connection database password

## Quick start

```bash
# build the application
docker compose build
# start application
docker compose up -d
# get the docker container id
docker ps
# get into the application container
docker exec -it "<container id>" /bin/bash
# run database migrations inside the container
php artisan migrate
# run database seed (test data) inside the container
php artisan db:seed
```

## Dockerhub

[kubequest](https://hub.docker.com/r/antoineleguillou/kubequest)

## Pipelines

A pipeline has been setup in order to build and push the docker image to dockerhub. As we want to suport arm architecture based execution environnement we build multi arch plateform images (linus/amd64n, linux/arm64).

## Helm chart

A basic helm chart has been setup in order to package kubernetes resources necessary to deploy the kubequest application.

In order to respect the single responsability principle this is your responsability to create the required secret and configmap to store your environnement variables you can then reference it when deploying the chart.

It uses the following overwritable values:

```yaml
image: antoineleguillou/kubequest:2024-06-28.1
replicaCount: 1
configMapRef: kubequest
secretRef: kubequest
port: 80
```

```bash
# install or upgrade the release using a values file located at root of the current directory and the kubequest release name
helm upgrade --install kubequest ./charts/kubequest -f ./values.yaml -n "<your namespace>" --create-namespace
# uninstall the kubeuest release
helm uninstall kubequest -n "<your namespace>"
```
