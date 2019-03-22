---
layout: post
title: NgRx Authentication
image: https://user-images.githubusercontent.com/18272237/54772584-0a59f000-4bde-11e9-83cf-613d7bac3624.png
---
NgRx authentication template. Using this post as a building block to build my official WebApp.

{: .mb-0}
`Process for creating/executing new Action for State:`
1. Write Action 
2. Define a reducer function
3. Update Component class with selector and methods to dispatch actions
4. Write an Effect

## Authentication Workflow
* Not Authenticated
* Logging In
* Returned With Valid Token

{: .mb-0}
#### `Not Authenticated`
<em>_User not authenticated_</em>

{: .table .table-sm .table-bordered .table-striped .table-hover .table-dark }
| [Auth] Check Authentication           |
| [Auth] Not Authenticated              |
| [Auth] Clear Authentication Details   |
{: .w-auto .mb-4 .text-center }

{: .mb-0}
#### `User Authenticating`
<em>User Logging in</em>

{: .table .table-sm .table-bordered .table-striped .table-hover .table-dark }
| [Auth] Check Authentication           |
| [Auth] Not Authenticated              |
| [Auth] Clear Authentication Details   |
| [Auth] Login                          |
| [Auth] Login Success                  |
| [Auth] Authenticated                  |
| [Router] Go                           |
| [Auth] Get User Info                  |
| ROUTER_NAVIGATION                     |
| [Auth] Get User Info Success          |
{: .w-auto .mb-4 .text-center }

{: .mb-0}
#### `Returning User w/Valid Token`
<em>User returns with valid token</em>

{: .table .table-sm .table-bordered .table-striped .table-hover .table-dark }
| [Auth] Check Authentication           |
| [Auth] Authenticated                  |
| [Auth] Get User Info                  |
| [Auth] Get User Info Success          |
{: .w-auto .mb-4 .text-center }





> `Check out these websites:`
>* [Angular FireBase](angularfirebase.com)
>* [FireShip](fireship.io)

***
[Auth0 NgRx](https://auth0.com/blog/ngrx-authentication-tutorial/)

[Authentication NgRx Actions](https://medium.com/@stuarttottle/angular-authentication-with-auth0-and-ngrx-e22228b04b3)

[Authentication NgRx Reference](https://mherman.org/blog/authentication-in-angular-with-ngrx/)
