# How to deploy?
You should have ruby, `bundler` gem installed

```
git clone https://github.com/ramonrails/ramonrails-rest-json-auth-atm-demo
cd ramonrails-rest-json-auth-atm-demo
bundle install
rails db:create db:migrate
rails server
```
you have a rails server running with REST API, to accept commands with JSON payload
now open another terminal to run the following commands

# Rails console
## create admin with email and password

`User.create!(email: 'ramonrails@gmail.com' , password: 'changeme123' , password_confirmation: 'changeme123', isadmin: true)`

## create user with card and pin

`User.create!(email: 'user@gmail.com' , card: 'card1234' , pin: '1234')`

authenticate using REST API with JSON payload
example result: `{"auth_token":"eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJleHAiOjE1NzY4Njk3MzZ9.u9n-b_GKonsd-htsk12bTf8eamJVxGvAjCdhoXmnTvI"}`

## get admin token
example: `eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjo1LCJleHAiOjE1NzY4NzIyNzh9.T0jaBIk2bzRN4q5quBowbD-pPLlJ7hJ_6lSbgiU_LCE`

`curl -H "Content-Type: application/json" -X POST -d '{"email":"ramonrails@gmail.com","password":"changeme123"}' http://localhost:3000/authenticate`

## get user token
example: `eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjo1LCJleHAiOjE1NzY4NzIxMzV9.mu6MHYzjtr-S2qSVHnlmyeOchIwc-6HNKSn58z3RG20`

`curl -H "Content-Type: application/json" -X POST -d '{"card":"card1234", "pin":"1234"}' http://localhost:3000/authenticate`

## fetch teller list (not authorised)

`curl http://localhost:3000/tellers`

## fetch with auth token - user

`curl -H "Authorization: eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjo1LCJleHAiOjE1NzY4NzIxMzV9.mu6MHYzjtr-S2qSVHnlmyeOchIwc-6HNKSn58z3RG20" http://localhost:3000/tellers`

## fetch with auth token - admin

`curl -H "Authorization: eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjo1LCJleHAiOjE1NzY4NzIyNzh9.T0jaBIk2bzRN4q5quBowbD-pPLlJ7hJ_6lSbgiU_LCE" http://localhost:3000/tellers`

## deposit

`curl -H "Authorization: eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjo1LCJleHAiOjE1NzY4NzIxMzV9.mu6MHYzjtr-S2qSVHnlmyeOchIwc-6HNKSn58z3RG20" -H "Content-Type: application/json" -X POST -d '{"denomination":"2000","notes":"20"}' http://localhost:3000/tellers`

## withdrawal

`curl -H "Authorization: eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjo1LCJleHAiOjE1NzY4NzIxMzV9.mu6MHYzjtr-S2qSVHnlmyeOchIwc-6HNKSn58z3RG20" -H "Content-Type: application/json" -X POST -d '{"denomination":"2000","notes":"-10"}' http://localhost:3000/tellers`

## fetch balance (only admin)

`curl -H "Authorization: eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjo1LCJleHAiOjE1NzY4NzIyNzh9.T0jaBIk2bzRN4q5quBowbD-pPLlJ7hJ_6lSbgiU_LCE" http://localhost:3000/tellers`

what about the headers in the requirement?
This is curl/REST/API. Just add them in the command/URL

```
-H "access-control-allow-headers: Origin, X-Requested-With, Content-Type, Accept"
-H "access-control-allow-methods: GET, POST, PUT"
-H "access-control-allow-origin:*"
-H "server: cloudflare-nginx"
```
