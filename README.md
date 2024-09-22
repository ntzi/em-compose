# Get started

- Clone services
    ```bash
    mkdir eventManager && 
    cd eventManager && 
    git clone git@github.com:ntzi/em-compose.git && 
    git clone git@github.com:ntzi/em-gateway-service.git && 
    git clone git@github.com:ntzi/em-events-service.git && 
    git clone git@github.com:ntzi/em-fans-service.git
    ```
- Start Docker containers
    ```bash
    cd em-compose &&
    docker compose up -d
    ```
- Test the services from Postman
    - Import the Postman collection `Event-Manager-2024.postman_collection.json`
    - Import the Postman environment `Event-Manager-2024.postman_environment.json`
    - Login and replace token in the environment variables (if needed)
    - Use `GetOne` request to get the relevant fans of an event based on the artists.


# How it works

There are 3 services in the system:
- Gateway service
- Events service
- Fans service

All services are connected to a Postgres database.\
All services use JWT authentication for service to service communication.\
Gateway service also uses JWT authentication for user authentication.

**Gateway service** is the entry point of the system. It is responsible for routing the requests to the appropriate service. It also handles the authentication and authorization of the users.\
When the service starts, it seeds the database with some data (`startLocal.sh`).\
It provides the following endpoints:
- `POST /api/v1/auth/login` to login
- `POST /api/v1/auth/register` to logout
- `GET /api/v1/events/:eventId/relevant-fans` to get the relevant fans of an event based on the artists

**Events service** is responsible for managing the events. It provides the following endpoints:
- `GET /api/v1/events/:eventId/artists` to get the artists of an event

**Fans service** is responsible for managing the fans. It provides the following endpoints:
- `GET /api/v1/fans/relevant-artists/:artistIds` to get the relevant fans of a list of artists


# Test
- Start test containers

    ```
    docker compose -f docker-compose.test.yml up -d
    ```

- Then watch test changes
    ```
    docker logs -f gateway-service-test
    ```
    For interactive test-watch use 
    ```
    npm run test-local:watch
    ```

- Or get test coverage
    ```
    npm run test
    ```
