# Catalog Service

## Useful Commands

| Gradle Command	         | Description                                   |
|:---------------------------|:----------------------------------------------|
| `./gradlew bootRun`        | Run the application.                          |
| `./gradlew build`          | Build the application.                        |
| `./gradlew test`           | Run tests.                                    |
| `./gradlew bootJar`        | Package the application as a JAR.             |
| `./gradlew bootBuildImage` | Package the application as a container image. |

After building the application, you can also run it from the Java CLI:

```bash
java -jar build/libs/catalog-service-0.0.1-SNAPSHOT.jar
```

## Container tasks

Run Catalog Service as a container

```bash
docker run --rm --name catalog-service -p 8080:8080 catalog-service:0.0.1-SNAPSHOT
```

### Container Commands

| Docker Command	              | Description       |
|:-------------------------------:|:-----------------:|
| `docker stop catalog-service`   | Stop container.   |
| `docker start catalog-service`  | Start container.  |
| `docker remove catalog-service` | Remove container. |

## Kubernetes tasks

### Create Deployment for application container

```bash
kubectl create deployment catalog-service --image=catalog-service:0.0.1-SNAPSHOT
```

### Create Service for application Deployment

```bash
kubectl expose deployment catalog-service --name=catalog-service --port=8080
```

### Port forwarding from localhost to Kubernetes cluster

```bash
kubectl port-forward service/catalog-service 8000:8080
```

### Delete Deployment for application container

```bash
kubectl delete deployment catalog-service
```

### Delete Service for application container

```bash
kubectl delete service catalog-service
```

## REST API

| Endpoint	      | Method   | Req. body  | Status | Resp. body     | Description    		   	     |
|:---------------:|:--------:|:----------:|:------:|:--------------:|:-------------------------------|
| `/books`        | `GET`    |            | 200    | Book[]         | Get all the books in the catalog. |
| `/books`        | `POST`   | Book       | 201    | Book           | Add a new book to the catalog. |
|                 |          |            | 422    |                | A book with the same ISBN already exists. |
| `/books/{isbn}` | `GET`    |            | 200    | Book           | Get the book with the given ISBN. |
|                 |          |            | 404    |                | No book with the given ISBN exists. |
| `/books/{isbn}` | `PUT`    | Book       | 200    | Book           | Update the book with the given ISBN. |
|                 |          |            | 201    | Book           | Create a book with the given ISBN. |
| `/books/{isbn}` | `DELETE` |            | 204    |                | Delete the book with the given ISBN. |

## Running a PostgreSQL Database

Run PostgreSQL as a Docker container

```bash
docker run --name polar-postgres \
    -e POSTGRES_USER=user \
    -e POSTGRES_PASSWORD=password \
    -e POSTGRES_DB=polardb_catalog \
    -p 5432:5432 \
    -d postgres:14.1
```

### Container Commands

| Docker Command	                     | Description       |
|:------------------------------------|:-----------------:|
| `docker stop polar-postgres`        | Stop container.   |
| `docker start polar-postgres`       | Start container.  |
| `docker remove polar-postgres`      | Remove container. |

### Database Commands

Start an interactive PSQL console:

```bash
docker exec -it polar-postgres psql -U user -d polardb_catalog
```

| PSQL Command	              | Description                                    |
|:---------------------------|:-----------------------------------------------|
| `\list`                    | List all databases.                            |
| `\connect polardb_catalog` | Connect to specific database.                  |
| `\dt`                      | List all tables.                               |
| `\d book`                  | Show the `book` table schema.                  |
| `\quit`                    | Quit interactive psql console.                 |

From within the PSQL console, you can also fetch all the data stored in the `book` table.

```bash
select * from book;
```
The following query is to fetch all the data stored in the `flyway_schema_history` table.

```bash
select * from flyway_schema_history;
```