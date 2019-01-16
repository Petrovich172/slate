---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - Go

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Ais_ms docs API! You can use our API to access endpoints, which can get information on various services.

You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/lord/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

In a case, your user is already registered you have to get your token key. Otherwise you have to register first.

<aside class="notice">
You must replace 'token key' <code>"Authorization: Bearer 'token key' "</code> with your personal token key.
</aside>

## Get token

```golang
(*gr).POST("/doauth", jwtAuth.LoginHandler)
```

` POST: Get token key for existing user "mortal" `
<aside>curl -X POST -d '{"username":"mortal", "password":"mortal"}' / 
-H "Content-Type: application/json" http://localhost:5829/api/doauth'</aside>

## Register new user

```golang
(*gr).POST("/registerNewUser", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), rolemodel.CreateUser())
```

` POST: Register new user "login11" with password "parol", role "2", department "id" and name "Maga122" `
<aside>curl -X POST http://localhost:5829/api/registerNewUser --header / 
"Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MzgwNDg4NjgsImxvZ2luIjoib2hteWhlYWQiLCJvcmlnX2lhdCI6MTUzNzk2MjQ2OCwicm9sZSI6MiwidXNlcl9pZCI6IjgwZTg0YzljLWU0MzAtNDM5MS1hNzM0LTkyYWUwYmMwNzFlMiJ9.bZ6O3O-YavOXRiiGzdyi8D5zLGzRvKEbFHJFpu1pcAg" / 
-d '{"NewUserName":"login11","NewUserPassword":"parol","NewUserRole":2,"Access":"Authorization","DepID":"e5d66151-5176-4abc-bf89-9756f303a521","Name":"Maga122"}'</aside>

## Edit user

```golang
(*gr).POST("/editUser", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), rolemodel.EditUser())
```

` POST: Edit user "login" (old login), set new login "login1", new password "paroll", new role "2", department "id" and name "Maga" `
<aside>curl -X POST http://localhost:5829/api/editUser --header /
 "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkZXBfaWQiOiJlNWQ2NjE1MS01MTc2LTRhYmMtYmY4OS05NzU2ZjMwM2E1MjEiLCJleHAiOjE1Mzg0NjY4NDksImxvZ2luIjoib2hteWhlYWQiLCJvcmlnX2lhdCI6MTUzODM4MDQ0OSwicm9sZSI6MiwidXNlcl9pZCI6IjgwZTg0YzljLWU0MzAtNDM5MS1hNzM0LTkyYWUwYmMwNzFlMiJ9.Hvp2YrKCTDjJnhfUD5TfebIDp_fk8wD5cz1PeXEWhA4" /
 -d '{"old_username":"login","NewUserName":"login1","NewUserPassword":"paroll","NewUserRole":2,"Access":"Authorization","DepID":"e5d66151-5176-4abc-bf89-9756f303a521","Name":"Maga"}'</aside>

## Check your role

```golang
(*gr).GET("/authrole", jwtAuth.MiddlewareFunc(), auth.GetRole)
```

` GET: Check your auth role `
<aside>curl -X GET  http://localhost:5829/api/authrole --header "Authorization: Bearer 'your token' "</aside>

## Refresh token

```golang
(*gr).GET("/refreshtoken", jwtAuth.MiddlewareFunc(), jwtAuth.RefreshHandler)
```

` GET: Refresh your token `
<aside>curl -X GET  http://localhost:5829/api/refreshtoken --header "Authorization: Bearer 'your token' "</aside>

## Get list of all users

```golang
(*gr).POST("/getUsers", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), rolemodel.GetUsers())
```

` POST: Get list of all users (for admin), users of your department for head and your data only for regular user `
<aside>curl -d "content-Type: application/json" -X POST http://localhost:5829/api/getUsers /
--header "Authorization: Bearer 'your token'"</aside>

```golang
(*gr).GET("/downloadcsv", jwtAuth.MiddlewareFunc(), utils.GetUsersCSV)
```

` GET: Get csv list of all users (for admin), users of your department for head and your data only for regular user `
<aside>curl -X GET  http://localhost:5829/api/downloadcsv --header "Authorization: Bearer 'your token' "</aside>

# NGPT data

Routes of ground transportation service

`https://niikeeper.com/msAPI/v1.0.0/ngpt/{geo, topo}/{busLanes, trolleybusLanes,tramLanes}`

<aside>curl -X GET  https://niikeeper.com/msAPI/v1.0.0/ngpt/geo/busLanes --header "Authorization: Bearer 'your token' "</aside>

## geojsons

```golang
geojsons.GET("/busLanes", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), general.GetNgpt("А", 0))
geojsons.GET("/trolleybusLanes", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), general.GetNgpt("Тб", 0))
geojsons.GET("/tramLanes", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), general.GetNgpt("Тм", 0))
geojsons.GET("/stops", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), general.GetNgptStops(0))
```
Successful query example:

`
{
  «type»: «FeatureCollection»
  «features»: [
    {
      «id»: Идентификатор рейса НГПТ (integer),
      «type»: «Feature»,
      «geometry»: {
        «type»: «MultiLineString»,
        «coordinates»: Трёхмерный массив ([][][]float64)
      },
      «properties»: {
        «routeName»: название маршрута НГПТ,
        «routeDirection»: направление маршрута (0 — прямое, 1 — обратное),
        «routeFullname»: полное название маршрута НГПТ,
        «transportType»: тип транспортного средства на данном маршруте,
        «typeTS»: класс транспортного средства,
        «releaseTs»: выпуск ТС,
        «isMain»: является ли рейс основным для маршрута (1 — да),
        «agencyName»: наименование перевозчика,
        «stops»: Перечень остановок НГПТ (массив integer) для данного рейса.
      }
    }
  ]
}
`
## topojsons

```golang
topojsons.GET("/busLanes", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), general.GetNgpt("А", 1))
topojsons.GET("/trolleybusLanes", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), general.GetNgpt("Тб", 1))
topojsons.GET("/tramLanes", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), general.GetNgpt("Тм", 1))
topojsons.GET("/stops", jwtAuth.MiddlewareFunc(), auth.CasbinCheckerMiddleware(e), general.GetNgptStops(1))
```

Successful query example:

`{
  «type»: «FeatureCollection»
  «features»: [
    {
      «id»: Идентификатор рейса НГПТ (integer),
      «type»: «Feature»,
      «geometry»: {
        «type»: «MultiLineString»,
        «coordinates»: Трёхмерный массив ([][][]float64)
      },
      «properties»: {
        «routeName»: название маршрута НГПТ,
        «routeDirection»: направление маршрута (0 — прямое, 1 — обратное),
        «routeFullname»: полное название маршрута НГПТ,
        «transportType»: тип транспортного средства на данном маршруте,
        «typeTS»: класс транспортного средства,
        «releaseTs»: выпуск ТС,
        «isMain»: является ли рейс основным для маршрута (1 — да),
        «agencyName»: наименование перевозчика,
        «stops»: Перечень остановок НГПТ (массив integer) для данного рейса.
      }
    }
  ]
}
`