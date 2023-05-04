# Documentation for using the Backendus repository

## The server is up and running on [mow-e.me:8080](http://mow-e.me:8080)

### Recommended software for experimenting with the websockets and endpoints

-   [Postman](https://www.postman.com/product/what-is-postman/)
-   [Insomnia](https://insomnia.rest/products/insomnia) - alternative to Postman

# Using the server

-   You can receive the token by using Swagger(see below)
-   Use these credentials through the login endpoint with these credentials:
    -   User:
    ```json
    {
        "username": "user",
        "password": "pass" 
    }
    ```
    -   Admin:
    ```json
    {
        "username": "admin",
        "password": "pass" 
    }
    ```

***

## Endpoints

-   [Swagger](http://mow-e.me:8080/swagger-ui/index.html)
    -   It will always have the updated list of endpoints
    -   It will tell you how your requests should look like
    -   It will let you try the endpoints


-   Via Postman
    -   Login\
        ![postman-login-endpoint.png](images%2Fpostman-login-endpoint.png)
    -   Hello World\
        ![postman-hello-endpointus.png](images%2Fpostman-hello-endpointus.png)

***

##  Websockets -> Stomp protocol

### Mower Team

-   Authentication
    -   When establishing the connection with server we need to specify we need to specify in the header:\
    `"x-auth-token" : "YOUR TOKEN"`
    -   Connect to address: `http://mow-e.me:8080/websocket`

-   To push the coordinates
    -   Make sure you did not miss the previous authentication step
    -   Send to `/app/coordinate`

    ```json
    {
        "mowerId": "MOWER_ID",
        "x": "x",
        "y": "y",
        "time": "cast your timestamp to int",
        "state": "STATE_CODE",
        "extra": "if collission specify your TEMP_IMAGE_ID(int) else ''"
    }
    ```      

-   To send images
    -   Divide your image into 4kb chunks
    -   Send them to `/app/images/add`

    ```json
    {
        "id": "this is the TEMP_IMAGE_ID(int) we specified earlier in 'extra' property when sending the coordinate where the collission happened",
        "chunkAmount": "specify the amount of chunks you divided your image in",
        "chunkOffset": "the number of the chunk you are sending right now P.S. Start from 0",
        "data": "chunk in bytearr encoded in base64"
    }
    ``` 
 
***

### Mobile Team

-   Authentication
    -   When establishing the connection with server we need to specify we need to specify in the header:\
    `"x-auth-token" : "YOUR TOKEN"`
    -   Connect to address: `http://mow-e.me:8080/websocket`

-   To subscribe to your mowers exclusive private topic where he posts his adventure path
    -   Make sure you did not miss the previous authentication step
    -   Subscribe to `/mower/{MOWER_ID}/queue/coordinate`
    -   Currently you can only receive the coordinates like this:

    ```json
    {
        "mowerId": "e193c17a-9c4e-4e3b-b2bc-f7a8a31a42b0",
        "x": 1.0,
        "y": 2.0,
        "time": 1683234033,
        "state": "START",
        "extra": "",
        "stateId": 0
    }
    ```
    -   You can not receive the images yes -> In progress
    -   You can not receive the old mowing sessions -> In progress

***

## Database access

-   [H2-console](http://mow-e.me:8080/h2-console)
    - This is our database.
    - Currently, it is accessible remotely.
    - To be able to access the h2-console make sure to
      - The credentials are: `JDBC URL: jdbc:h2:file:./data/test;AUTO_SERVER=TRUE` , `username: admin`, `password: admin`\
      ![h2-console.png](images%2Fh2-console.png)


   -   The database interface of h2-console is pretty intuitive since it is SQL\
     ![h2-console-gui.png](images%2Fh2-console-gui.png)
