---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - javascript

toc_footers:
  - <a href='/#subscribe'>Sign Up for a Developer Key</a>
  - <a href='/'>Go to Homepage</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Sports Odds API
---

# Overview

## Introduction

Welcome to the Sports Odds API! You can use our REST API to access up to date betting odds for multiple American leagues across dozens of bookmakers.

We have language examples in Shell, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right. Requests can be made in any language, even ones not in the examples.

## Leagues Supported

We currently support odds and results from 4 leagues. When filtering with the `league` parameter please use the abbreviation code.

Abbreviation | Full Name | Sport
------------ | --------- | -----
NBA | National Basketball Association | Basketball
NFL | National Football League | American Football
MLB | Major League Baseball | Baseball
NHL | National Hockey League | Hockey

## Query Parameters

All endpoints contain query parameters that users can use to filter results. Unless otherwise stated all string parameters are not case sensitive and do not require an exact match. Below are some descriptions of common parameters and how to use them.

Parameter | Description | Example
--------- | ----------- | -------
league | Uses the 3 letter abbreviation code | `NBA`, `NFL`,
season | Uses the year. Does not require exact match | `2021`, `2022-23`
home_team | Uses the 3 letter team code | `ARI`, `CHA`
away_team | Uses the 3 letter team code | `ARI`, `CHA`
dates | Uses `YYYY-mm-dd` format. Can filter by multiple dates with a comma separated list. | `2020-08-02`, `2020-07-24,2020-07-25`

<aside class="warning">
All <code>dates</code> are in UTC time zone.
</aside>

##  Pagination

> An example of pagination for our `/seasons` endpoint:

```json
{
    "count":373,
    "next":"https://www.sports-odds-api.com/seasons?page=3",
    "previous":"https://www.sports-odds-api.com/seasons",
    "results":[
        {
            "league":"NBA",
            "year":"2017-18",
            "start_date":"2017-09-30",
            "end_date":"2018-07-01"
        },
        ...
    ]
}
```

Sports Odds API uses limit offset pagination. Our endpoints are automatically paginated to return 20 responses at once. To get the next and previous set of results you can use the urls returned in the `next` and `previous` fields if they exist. We also return the total count of results in `count` on every request.


# Authentication

## Token Authentication

> To authorize, use this code:

```python
import requests

response = requests.get(url, headers={"Authorization": "Token <API_KEY>"})
```

```shell
curl "api_endpoint_here" \
  -H "Authorization: Token <API_KEY>"
```

```javascript
fetch(url, {
  method: "GET",
  headers: {"Authorization": "Token <API_KEY>"}
})
```

> Make sure to replace &lt;API_KEY&gt; with your API key.

Sports Odds API token authentication to authorize access. You can register a new API key [here](/#subscribe).

For clients to authenticate, the token key should be included in the <code>Authorization</code> HTTP header. The key should be prefixed by the string literal "Token", with whitespace separating the two strings. For example:
`Authorization: Token <API_KEY>`

<aside class="notice">
You must replace <code>&lt;API_KEY&gt;</code> with your personal API key.
</aside>

## Rate Limits

Our free API key has a rate limit of 20 requests per day.

For increased rate limits please email sportsbetapi@zohomail.com with the email associated with the api key.

# Game Information

## Seasons

```python
import requests

response = requests.get("www.sports-odds-api.com/seasons", headers={"Authorization": "Token <API_KEY>"})
```

```shell
curl "www.sports-odds-api.com/seasons" \
  -H "Authorization: Token <API_KEY>"
```

```javascript
fetch("www.sports-odds-api.com/seasons", {
  method: "GET",
  headers: {"Authorization": "Token <API_KEY>"}
})
```

> The above command returns JSON structured like this:

```json
{
    "count":373,
    "next":"https://www.sports-odds-api.com/seasons?page=2",
    "previous":null,
    "results":[
        {
            "league":"NBA",
            "year":"2022-23",
            "start_date":"2022-10-02",
            "end_date":"2023-06-24"
        },
        {
            "league":"NHL",
            "year":"2022-23",
            "start_date":"2022-07-23",
            "end_date":"2023-07-01"
        },
        ...
    ]
}
```

This endpoint retrieves all seasons we support sorted by most recent. These seasons can be used in later queries using the year. Not all seasons have games or odds history.

<aside class="notice">
Some seasons contain one year (<code>2021</code>), others contain start and end year (<code>2022-23</code>). This is based on what the leagues use.
</aside>

### HTTP Request

`GET https://www.sports-odds-api.com/seasons`

### Query Parameters

Parameter | Description | Examples
--------- | ----------- | --------
league | Filters the season by the league associated with it. | `NBA`, `NFL`
year  | Filters the awards by the year associated with it. | `2021`, `2022-23`

## Teams

```python
import requests

response = requests.get("www.sports-odds-api.com/teams", headers={"Authorization": "Token <API_KEY>"})
```

```shell
curl "www.sports-odds-api.com/teams" \
  -H "Authorization: Token <API_KEY>"
```

```javascript
fetch("www.sports-odds-api.com/teams", {
  method: "GET",
  headers: {"Authorization": "Token <API_KEY>"}
})
```

> The above command returns JSON structured like this:

```json
{
    "count":124,
    "next":"https://www.sports-odds-api.com/teams?page=2",
    "previous":null,
    "results": [
        {
            "league": "NHL",
            "full_name": "Anaheim Ducks",
            "short_name": "Ducks",
            "code": "ANA",
            "city": "Anaheim"
        },
        {
            "league": "NFL",
            "full_name": "Arizona Cardinals",
            "short_name": "Cardinals",
            "code": "ARI",
            "city": "Arizona"
        },
        ...
    ]
}
```

This endpoint retrieves all teams we support sorted by alphabetical order. The team codes can be used as a url parameter to filter in other endpoints.

### HTTP Request

`GET https://www.sports-odds-api.com/teams`

### Query Parameters

Parameter | Description | Examples
--------- | ----------- | --------
league | Filters the team by the league associated with it. | `NBA`, `NFL`
full_name  | Filters the team by full name. | `Cardinals`, `Panthers`
city  | Filters the team by city. | `New York City`, `Boston`
code  | Filters the team by code. | `ARI`, `CAR`

## Games

```python
import requests

response = requests.get("www.sports-odds-api.com/games", headers={"Authorization": "Token <API_KEY>"})
```

```shell
curl "www.sports-odds-api.com/games" \
  -H "Authorization: Token <API_KEY>"
```

```javascript
fetch("www.sports-odds-api.com/games", {
  method: "GET",
  headers: {"Authorization": "Token <API_KEY>"}
})
```

> The above command returns JSON structured like this:

```json
{
    "count":4539,
    "next":"https://www.sports-odds-api.com/games?page=2",
    "previous":null,
    "results": [
        {
            "id": "468848dde10fffe805f171b808b0e38c587b88f71168371a0d6340b94ef7c5b0",
            "game_time": "2023-01-08 05:00:00",
            "home_team": "ATL",
            "away_team": "TB",
            "season": "2022",
            "league": "NFL",
            "game_type": "regular_season",
            "status": "not_started",
            "home_score": 0,
            "away_score": 0
        },
        ...
    ]
}
```

This endpoint retrieves games ordered by latest date. Game scores and statuses are updated every ~5 minutes.

<aside class="warning">
Some games far in advance may have their <code>game_time</code> as a placeholder. Please check closer to the game day for the accurate game time.
</aside>

The possible values for `status` and `game_type` are shown below in the query parameter examples.

<aside class="notice">
<code>game_time</code> is returned in UTC timezone.
</aside>

### HTTP Request

`GET https://www.sports-odds-api.com/games`

### Query Parameters

Parameter | Description | Examples
--------- | ----------- | --------
season | Filters games by the season associated with it. | `2022`, `2022-23`
league  | Filters games by the league associated with it. | `NBA`, `NFL`
home_team  | Filters games by the home_team associated with it. | `ATL`, `TB`
away_team  | Filters games by the away_team associated with it. | `ATL`, `TB`
dates  | Filters games by the date of game_date. Can have multiple separated comma dates. | `2022-07-24`, `2022-01-01,2022-02-01`
status  | Filters games by the status. | Allowed Values: `not_started`, `in_progress`, `final`, `cancelled`, `postponed`
game_type  | Filters games by the game_type. | Allowed Values: `preseason`, `regular_season`, `postseason`

# Odds

## Future Awards Names

```python
import requests

response = requests.get("www.sports-odds-api.com/futures/awards", headers={"Authorization": "Token <API_KEY>"})
```

```shell
curl "www.sports-odds-api.com/futures/awards" \
  -H "Authorization: Token <API_KEY>"
```

```javascript
fetch("www.sports-odds-api.com/futures/awards", {
  method: "GET",
  headers: {"Authorization": "Token <API_KEY>"}
})
```

> The above command returns JSON structured like this:

```json
{
   "count":2,
   "next":null,
   "previous":null,
   "results":[
      {
         "name":"2022-23 NBA MVP",
         "league":"NBA",
         "season":"2022-2023"
      },
      {
         "name":"2022-23 NBA Rookie of the Year",
         "league":"NBA",
         "season":"2022-2023"
      }
   ]
}
```

This endpoint retrieves all future award names we have. These award names then can be used to query futures odds under `name`.

### HTTP Request

`GET https://www.sports-odds-api.com/futures/awards`

### Query Parameters

Parameter | Description | Examples
--------- | ----------- | --------
name | Filters the awards by its name. | `MVP`, `Championship`
league | Filters the awards by the league associated with it. | `NBA`, `NFL`
season | Filters the awards by the season associated with it. | `2021`, `2022-23`

## Future Odds

```python
import requests

response = requests.get("www.sports-odds-api.com/futures", headers={"Authorization": "Token <API_KEY>"})
```

```shell
curl "www.sports-odds-api.com/futures" \
  -H "Authorization: Token <API_KEY>"
```

```javascript
fetch("www.sports-odds-api.com/futures", {
  method: "GET",
  headers: {"Authorization": "Token <API_KEY>"}
})
```

> The above command returns JSON structured like this:

```json
{
   "count":901,
   "next":"https://www.sports-odds-api.com/futures/?page=2",
   "previous":null,
   "results":[
      {
         "name":"2022-23 NBA Western Conference - To Win",
         "bet_provider":"Bet Rivers NY",
         "odds":250,
         "full_name":"Golden State Warriors"
      },
      {
         "name":"2022-23 NBA Western Conference - To Win",
         "bet_provider":"Consensus",
         "odds":250,
         "full_name":"Golden State Warriors"
      },
      {
         "name":"2022-23 NBA Western Conference - To Win",
         "bet_provider":"UnibetNJ",
         "odds":250,
         "full_name":"Golden State Warriors"
      },
      ...
   ]
}
```

This endpoint retrieves future awards odds sorted from the lowest odds.

### HTTP Request

`GET https://www.sports-odds-api.com/futures`

### URL Parameters

Parameter | Description | Examples
--------- | ----------- | --------
name | Filters the odds by the award name. Use the Future Awards endpoint above to get list of names. | `2022-23 NBA MVP`, `NFL Comeback Player`
full_name | Filters the odds by the recipient name. Can be a team or a player. | `Luka`, `Pistons`
league | Filters the odds by the league associated with the awards. | `NBA`, `NFL`
season | Filters the odds by the season associated with the awards. | `2021`, `2022-23`
bet_provider | Filters the odds by the bookmaker providing the odds. | `MGM`, `Caesars AZ`
min_odds | Integer. Minimum bet odds that should be returned. | `-110`, `2000`
max_odds | Integer. Maximum bet odds that should be returned. | `-110`, `2000`

## Game Odds

```python
import requests

response = requests.get("www.sports-odds-api.com/odds", headers={"Authorization": "Token <API_KEY>"})
```

```shell
curl "www.sports-odds-api.com/odds" \
  -H "Authorization: Token <API_KEY>"
```

```javascript
fetch("www.sports-odds-api.com/odds", {
  method: "GET",
  headers: {"Authorization": "Token <API_KEY>"}
})
```

> The above command returns JSON structured like this:

```json
{
    "highest_count":12294,
    "overall_total":34744,
    "next":"https://www.sports-odds-api.com/odds?limit=20&offset=20",
    "previous":null,
    "results": [
        {
            "id":"00004fa0d1f6634dfbaaf2a2f74f067b26da77d52d370b0ab6d2a9e82a975a24",
            "game": {
                "id": "bb2219c0aa920b3a7397c1232157523c3d47cef86da95d6ecbd0bc21386be3b3",
                "game_time": "2022-06-22 22:40:00",
                "home_team":"FLA",
                "away_team":"COL",
                "season":"2022",
                "league":"MLB",
                "game_type":"regular_season",
                "status":"final",
                "home_score":7,
                "away_score":4
            },
            "bet_provider":"DraftKings NJ",
            "away_odds":1200,
            "home_odds":-3000,
            "type":"money_line"
        },
        {
            "id":"00004fa0d1f6634dfbaaf2a2f74f067b26da77d52d370b0ab6d2a9e82a975a24",
            "game":{
                "id":"bb2219c0aa920b3a7397c1232157523c3d47cef86da95d6ecbd0bc21386be3b3",
                "game_time":"2022-06-22 22:40:00",
                "home_team":"FLA",
                "away_team":"COL",
                "season":"2022",
                "league":"MLB",
                "game_type":"regular_season",
                "status":"final",
                "home_score":7,
                "away_score":4
            },
            "bet_provider":"DraftKings NJ",
            "away_odds":1000,
            "home_odds":-2100,
            "away_spread":"2.500",
            "home_spread":"-2.500",
            "type":"point_spread"
        },
        {
            "id":"d0d9642bdddc4649b5ec16b3efd2a5a657010ff379808f4cfed20812dd503eee",
            "game":{
                "id":"bb2219c0aa920b3a7397c1232157523c3d47cef86da95d6ecbd0bc21386be3b3",
                "game_time":"2022-06-22 22:40:00",
                "home_team":"FLA",
                "away_team":"COL",
                "season":"2022",
                "league":"MLB",
                "game_type":"regular_season",
                "status":"final",
                "home_score":7,
                "away_score":4
            },
            "bet_provider":"SugarHouse",
            "total":"8.500",
            "over_odds":107,
            "under_odds":-139,
            "type":"over_under"
        }
        ...
    ]
}
```

This endpoint retrieves single game betting odds. Contains money lines, point spreads and over/unders.

### HTTP Request

`GET https://www.sports-odds-api.com/odds`

### URL Parameters

Parameter | Description | Examples
--------- | ----------- | --------
id | Filters the odds by `id` of the bet. Used to find the same bet again. | `468848dde10fffe805f171b808b0e38c587b88f71168371a0d6340b94ef7c5b0`
game_id | Filters the odds by `id` of the corresponding game. Use the Games endpoint to find the game ids. | `468848dde10fffe805f171b808b0e38c587b88f71168371a0d6340b94ef7c5b0`
home_team | Filters odds by the home_team associated with it. | `ATL`, `TB`
away_team | Filters odds by the away_team associated with it. | `ATL`, `TB`
league | Filters the odds by the league associated with the awards. | `NBA`, `NFL`
season | Filters the odds by the season associated with the awards. | `2021`, `2022-23`
bet_provider | Filters the odds by the bookmaker providing the odds. | `MGM`, `Caesars AZ`
bet_type | Filters the odds by the type of bet. | Allowed Values: `money_line`, `point_spread`, `over_under`
