---
layout: post
title: NgRx Authentication
image: https://user-images.githubusercontent.com/18272237/54772584-0a59f000-4bde-11e9-83cf-613d7bac3624.png
---
NgRx authentication template frontend. This is to be used with backend server that uses JWT for authentication. Auth Guard, Router-Store, and Token Interceptor used. 

## Authentication Workflow
* Check if token exist
    * if token exist, check token expiration
    * else token does not exist, return not authenticated
* Check if token is expired
    * if not expired, return authenticated
    * else expired, try to refresh token
* Check if authenticated
    * if authenticated, insert token into header for API calls
    * else not authenticated, route to login?


{: .mb-1}
### `Actions`

{: .table .table-sm .table-bordered .table-striped .table-hover .table-dark }
| ACTION | DESCRIPTION |
| [Auth] Check Authentication           | Check if token exist |
| [Auth] Authenticated                  | Token exists |
| [Auth] Not Authenticated              | Token does not exist |
| [Auth] Clear Authentication Details   | Remove all info from localStorage |
| [Auth] Check Session                  | Check is token is expired |
| [Auth] Session Success                | Token is valid |
| [Auth] Session Expired                | Token is expired |
| [Auth] Refresh Session                | Attempt to Refresh Token |
| [Auth] Refresh Session Success        | Successful refresh token |
| [Auth] Refresh Session Failure        | Failure refresh token |
| [Auth] Login                          | Attempt to login |
| [Auth] Login Success                  | Login successful |
| [Auth] Get User Info                  | Attempt to get user info |
| [Auth] Get User Info Success          | Successful user info request |
{: .w-auto .mb-4 .text-center }


{: .mb-1}
### `Backend API Request Routes`

{: .table .table-sm .table-bordered .table-striped .table-hover .table-dark }
|   URL | HTTP Verb   |   Action  | Response    |
|   http://localhost:8000/ping  |   GET    |   Sanity Check    |    Server Status   |
|   http://localhost:8000/api-login  |   POST    |   Login User    |    Access & Refresh Tokens |
|   http://localhost:8000/api-signup  |   POST    |   Register New User    |    Access & Refresh Tokens |
|   http://localhost:8000/api-refresh  |   POST    |   Refresh Token/Session    |   Access Token    |
|   http://localhost:8000/api-logout  |   DELETE    |   Logout User    |    200 HTTP Response   |
|   http://localhost:8000/api-user  |   GET    |   View User Data    | User Data   |
{: .w-auto .mb-4 .text-center }

{: .mb-0}
## Scenarios
Scenarios for users request to home page:
{: .mb-0}
* No Token
* Expired Token
* Valid Token
* Logging In
* Signing Up

##### User with valid token:  
* Check for token - `[Auth] Check Authentication Status`  
* Has token - `[Auth] Authenticated`  
* Check token - `[Auth] Check Session Status`  
* Valid token - `[Auth] Session Status Success`  
* Route to dashboard - `[Route] Dashboard [ ? ]`  
* Use token to call api - `[Auth] Get User Info`  

##### User login:  
* Check for token - `[Auth] Check Authentication Status`  
* No token - `[Auth] Not Authenticated`  
* Remove item from localStorage - `[Auth] Clear Authentication Details`  
* Route to login - `[Route] Login [ ? ]` 
* Check authentication credentials - `[Auth] Login`  
* Success login - `[Auth] Login Success`   
* Has token - `[Auth] Authenticated`  
* Route to dashboard - `[Route] Dashboard [ ? ]`  
* Use token to call api - `[Auth] Get User Info`  


##### User signup:  
* Check for token - `[Auth] Check Authentication Status`  
* No token - `[Auth] Not Authenticated`  
* Remove item from localStorage - `[Auth] Clear Authentication Details`  
* Route to Login - `[Route] Login [ ? ]`  
* Route to Signup - `[Route] Register [ ? ]`  
* Check signup credentials - `[Auth] Signup`  
* Success signup - `[Auth] Signup Success`

{: .text-primary}  
##### choose one below:  

* Has token - `[Auth] Authenticated`  
* Route to dashboard - `[Route] Dashboard [ ? ]`  
* Use token to call api - `[Auth] Get User Info`  

{: .text-warning} 
##### or  

* Route to login - `[Route] Login [ ? ]` 
* Check authentication credentials - `[Auth] Login`  
* Success login - `[Auth] Login Success`   
* Has token - `[Auth] Authenticated`  
* Route to dashboard - `[Route] Dashboard [ ? ]`  
* Use token to call api - `[Auth] Get User Info`   



> `Check out these websites:`
>* [Angular FireBase](http://angularfirebase.com)
>* [FireShip](http://fireship.io)

***
[Auth0 NgRx](https://auth0.com/blog/ngrx-authentication-tutorial/)

[NgRx Router-Store](https://github.com/ngrx/router-store)

[Authentication NgRx Actions](https://medium.com/@stuarttottle/angular-authentication-with-auth0-and-ngrx-e22228b04b3)

[Understanding NgRx Effects](https://medium.com/@tanya/understanding-ngrx-effects-and-the-action-stream-1a74996a0c1c)

[Authentication NgRx Reference](https://brianflove.com/2017/04/10/angular-reactive-authentication/)

[Authentication NgRx Reference](https://mherman.org/blog/authentication-in-angular-with-ngrx/)
