---
layout: post
title: "Analytics in SQL: Practice Your Skills"
comments: true
permalink: "analytics-sql"
---

## Example Engagement Database Design

The `dim_clients` table:

| client_id | client_name |
| --------- | ----------- |
|         0 |      iphone |
|       ... |         ... |


The `dim_action` page:

| action_id | action_name |
| --------- | ----------- |
|         0 |  front_page |
|       ... |         ... |

The `dim_users` page:

| user_id | user_name |
| ------- | --------- |
|       0 |     Steve |
|     ... |       ... |


The `fact_engagement` page:

|  date_id | user_id | client_id | action_id |
| -------- | ------- | --------- | --------- |
| 20140503 |       2 |         2 |         1 |
|      ... |     ... |       ... |       ... |

## Create these tables:

First, create the database

```sql
CREATE DATABASE analytics_example
USE analytics_example
```

Now, create the tables

```sql
CREATE TABLE dim_clients (
  client_id INT NOT NULL,
  client_name VARCHAR(30) NOT NULL,
  PRIMARY KEY (client_id)
);

CREATE TABLE dim_actions (
  action_id INT NOT NULL,
  action_name VARCHAR(30) NOT NULL,
  PRIMARY KEY (action_id)
);

CREATE TABLE dim_users (
  user_id INT NOT NULL,
  user_name VARCHAR(30) NOT NULL,
  PRIMARY KEY (user_id)
);

CREATE TABLE fact_engagement (
  date_id INT NOT NULL,
  user_id INT NOT NULL,
  client_id INT NOT NULL,
  action_id INT NOT NULL
)
```
Note, there is no PK on `fact_engagement`. This implies that a user can 
perform an action multiple times per day.
Typically, there would also be a timestamp column and the other columns would be part of a PK to 
allow for duplicate actions in a day without allowing totally duplicate rows

Now, add to tables:


https://gist.github.com/joshualande/d194f84f1ce80e1a4e2e

## Practice Analytics Questions

### (Warm Up) Compute Most Popular Action

```sql
SELECT b.action_name, COUNT(a.action_id) AS num_action
FROM fact_engagement AS a
JOIN dim_actions AS b
ON a.action_id = b.action_id
GROUP BY a.action_id
```

### Compute Most Active User 

Compute Most Active User in a Given Day

### Compute Daily Active Users (DAUs)

```sql
SELECT date_id, COUNT(distinct user_id)
FROM fact_engagement
GROUP BY date_id
```

### Compute Monthly Active Users (MAUs)

```sql
SELECT date_id, COUNT(distinct user_id)
FROM fact_engagement AS a
JOIN fact_engagement AS b
ON XXX
GROUP BY a.date_id DESC
```

###

For each client pair, compute number of users who have both clients

For each action, compute number of actions per day and number of unique users who compute that action

### Compute Top Client For Each User

For the top client for each user for each day

```sql
SELECT *
FROM fact_engagement
GROUP BY (date_id, user_id)
```

### Number of Login Days

For each user in the last month, compute the number of days that the user logged in.

Next, for each rolling 30 day period, compute the number of login days.

### Users on Multiple Clients

For each client pair, compute the
number of unique users

### Compute Signups

For each User, compute the day they signed up.
This is defined as the first day they logged in.

### Compute Churn

Compute all fraction of users who in in the first month but didn't
log in in the second month.

If you can get this to work for static dates, compute a rolling
count of churn (for every day, the fraction of users in the previous
60-30 day period who didn't log in in the last 30 days).