---
title: Golang JWT 示例
date: 2020-02-29
---

> Golang JWT 示例


## Demo

`.env`

```env
SECRET_KEY=secret_key
```

`main.go`

```go
package main

import (
    "os"
    "fmt"
    "time"
    "github.com/joho/godotenv"
    "github.com/dgrijalva/jwt-go"
)

func init() {
    // loads values from .env into the system
    if err := godotenv.Load(); err != nil {
        fmt.Print("No .env file found")
    }
}

func main() {
    SecretString, exists := os.LookupEnv("SECRET_KEY")

    if !exists {
        return
    }

    fmt.Println(SecretString)
    Secret := []byte(SecretString)

    type MyCustomClaims struct {
        ID int `json:"id"`
        jwt.StandardClaims
    }
    
    // Create the Claims
    claims := MyCustomClaims{
        10012,
        jwt.StandardClaims{
            ExpiresAt: time.Time.AddDate(time.Now(),0, 0, 7).Unix(),
            Issuer:    "localhost",
        },
    }
    
    token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
    ss, err := token.SignedString(Secret)
    fmt.Printf("%v %v", ss, err)
}
```

<!-- more -->

`parse.go`

```
package main

import (
    "fmt"
    "os"
    "github.com/joho/godotenv"
    "github.com/dgrijalva/jwt-go"
)

func init() {
    // loads values from .env into the system
    if err := godotenv.Load(); err != nil {
        fmt.Print("No .env file found")
    }
}

func main() {
    tokenString := "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MTAwMTIsImV4cCI6MTU3NTYzMzE2MCwiaXNzIjoibG9jYWxob3N0In0.tl0obbcF9me3LALR-3uCYuGSFcxK7kXRoFXGz4dj1fk"
    
    SecretString, exists := os.LookupEnv("SECRET_KEY")

    if !exists {
        return
    }

    fmt.Println(SecretString)
    Secret := []byte(SecretString)

    type MyCustomClaims struct {
        ID int `json:"id"`
        jwt.StandardClaims
    }
    
    token, err := jwt.ParseWithClaims(tokenString, &MyCustomClaims{}, func(token *jwt.Token) (interface{}, error) {
        return Secret, nil
    })

    if claims, ok := token.Claims.(*MyCustomClaims); ok && token.Valid {
        fmt.Println(claims.ID, claims.StandardClaims.ExpiresAt)
    } else {
        fmt.Println(err)
    }
}
```

## Run

```shell
$ go run main.go 
> secret_key
> eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MTAwMTIsImV4cCI6MTU3NTYzMzE2MCwiaXNzIjoibG9jYWxob3N0In0.tl0obbcF9me3LALR-3uCYuGSFcxK7kXRoFXGz4dj1fk <nil>%
```
```shell
$ go run parse.go
> secret_key
> 10012 1575633160

```