## Go Graphql Starter

This project aims to use [neelance/graphql-go](https://github.com/neelance/graphql-go) to build a starter web application. This project would be continuously under development. Pull request is welcome. 

#### RoadMap:
- [x] Integrated with sqlx
- [x] Integrated with graphql-go
- [x] Use go-bindata to generate Go code from .graphql file
- [ ] Use psql
- [x] Integrated with dataloader
- [x] Add authentication & authorization
- [ ] Add unit test cases
- [ ] Support subscription
- [ ] Support web-socket notification and messaging

#### Structure
    go-graphql-starer
    │   README.md
    │   test.db             --- the temporary testing database for demo
    │   server.go           --- the entry file
    │   Config.toml         --- the configuration file for setting server parameter
    │   Dockerfile
    │   Gopkg.lock          --- generated file from dependency management tool, dep
    │   Gopkg.toml          --- generated file from dependency management tool, dep
    │   graphiql.html       --- the html for graphiql which is used for testing query & mutation
    └───config              --- configuration utilities like db
    └───data                --- storing the sql data patch for different version
    │   └───1.0             --- storing sql data patch for version 1.0
    │      └───...          --- sql files
    └───handler             --- the handler used for chaining http request like authentication, logging etc.
    └───loader              --- implementation of dataloader for caching and batching the graphql query
    └───model               --- the folder putting struct file
    └───resolver            --- implementation of graphql resolvers
    └───schema              --- implementation of graphql resolvers
    │   │   schema.go       --- used for generate go code from static graphql files inside 'type' folder
    │   │   schema.graphql  --- graphql root schema
    │   └───type            --- folder for storing graphql schema in *.graphql format
    │       └───...         --- graphql schema files in *.graphql format
    └───service             --- services for users, authorization etc.
    └───util                --- utilities

#### Usage:

1. Install go-bindata
    ```
    go get -u github.com/jteeuwen/go-bindata/...
    ```

2. Setup GOPATH

    For example: MacOS
    ```
    export GOPATH=/Users/${username}/go
    export PATH=$PATH:$GOPATH/bin
    ```

3. Run the following command at root directory to generate Go code from .graphql file
    ```
    go-bindata -ignore=\.go -pkg=schema -o=schema/bindata.go schema/...
    ```

    OR

    ```
    go generate ./schema
    ```
    There would be bindata.go generated under `schema` folder


4. Start the server
    ```
    go build server.go
    ```
    
#### Graphql Example:

Test in graphiql by the following endpoint

```
localhost:3000
```

Basically there are two graphql queries and one mutation

Query:
1. Get an user by email
2. Get user list by cursor pagination

Mutation:
1. Create an user

For user list query, you need to be authenticated in order to use it.
Authentication is not required for other operations.

In order to perform authentication/login, you need to create a user by graphql mutation first

Then you could submit your email and password by Basic Authorization Header with the following endpoint using POST method
```
localhost:3000/login
```

After that, you would get an access token(jwt)
You can change the Authorization of request header in `graphiql.html` and restart the server to see the effect of authentication using token

#### Test:

- Run Unit Tests
    ```
    go test
    ```
    
#### Reference

-[neelance/graphql-go](https://github.com/neelance/graphql-go)

-[tonyghita/graphql-go-example](https://github.com/tonyghita/graphql-go-example)
