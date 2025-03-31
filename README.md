# Product Feed Ingester 
Author: Tom Strissel

This is a command-line PHP application that imports product data from a CSV file into a MySQL database.


Thank you for taking the time to look at my submission! It's been some time since I've used Docker/Symfony/DDD principles,
and it feels great to get refreshed with it. If I were writing a backend PHP application at scale today, these are the frameworks
I would either start a greenfield application with, or be in the process of refactoring into.

---

## Features

- CSV ingestion and validation
- Domain-driven architecture (DDD)
- ON DUPLICATE KEY UPDATE persistence in MySQL
- File-based logger (easily swappable with Kibana or other services)
- Clean separation of input/output/persistence layers
- 100% testing code coverage with PHPUnit including test database testing
- Containerized with Docker and Docker Compose

---

## Getting Started

### Prerequisites

- Docker & Docker Compose

### Installation

1. Unzip the project
2. Spin up containers
   ```bash
   docker-compose up --build -d
   ```
3. Run composer install (see note in Future Iterations below!)
   ```bash
      docker exec -it product_ingester composer install
4. Start the ingester
    ```bash
   docker exec -it product_ingester composer start
   ```

## Running Tests (includes Coverage)
Simply run:
```bash
  docker exec -it product_ingester composer test
```


## Architecture & Design
Domain-Driven Design (DDD) structure

Symfony Console & Dependency Injection container

Explicit abstractions for:

- Input Layer (CSV today, Kafka or HTTP tomorrow)

- Persistence Layer (MySQL now, GCP ready)

- Logging Layer (logfile now, Kibana-ready)

## Business Logic Assumptions
- Products being ingested should be updated if they are re-ingested with the same GTIN, which are unique identifiers
- The Company may wish to scale and acquire new markets with new languages; therefore, a picklist of languages was created to better handle new languages and keep the product table cleaner

## Future Iterations
- Have composer install run successfully on docker-compose up (was having an issue with this when testing zipping/unzipping the project worked for submission), hence why I added the extra step in the instructions to make sure it worked :)
- Use PHP8.4 and have consistent use of constructor property promotion (this is actually new for me and I just did it for a couple files just to see how it works)
- Implement Redis caching on the language picklist lookup in  `ValidationService.php` to reduce runtime and overload on the SQL server. We used redis at my previous company, along with memcached. I would have to see which one is more appropriate.
- Implement batch insert/update for products to further reduce load on the server
- Extend the ProductRepositoryInterface for alternative persistence layer solutions (GCP, etc)
- Extend the ImportService to import from Kafka or an alternative data stream)
- Better practices of .env usage for variables, especially testing related ones, as it pertains to these kinds of apps
- Consider a better way to set up the testing database from the original one, as this required manual creation via `init.sql`, but for the purposes of this test, this was okay.
- Kibana Logging for logs and Grafana for DB performance
- Additional URL validation
- Image type validation
- Consideration of currency. In different markets you may have different currencies, in which case you may want to indicate a currency on a product page and reflect that in a database. in that case I would have an additional field on the product level to indicate currency, and possibly another picklist for the currency symbol/name/etc. The `Product` entity would probably have some `Money` attribute in that case. 





   
   
