# Workana Hiring challenge

Hi!

We are looking for great PHP developers to join our team. Instead of going through a ~~boring~~ long interview process, we decided that code often speaks for itself. If you're up to the challenge, please take a couple of hours play with this problem and submit your solution.

## The problem

Workana is built around a PHP framework and given it's a fairly large application, we have a baseline of around ~125ms. When we launched our chat, we needed a way to retrieve the current user's friends list. This endpoint (`/chat/friends`) needed to be blazing fast, as it would be called thousands of times per hour. So we removed this endpoint from our application á la *microservice*. We wrote this code (yes, this was code used in production), but it was far from perfect, and far from being testable. Our `index.php` contains a single routine with lots of dependencies and too much knowledge.

## Get up and running

To run this code you need:
  - PHP 5.6
  - [Composer](getcomposer.org)
  - Redis

Then:
  - Clone this repo: `git clone git@github.com:Workana/hiring_challenge.git`.
  - Run `composer install`.
  - Copy the `.env.example` file to `.env` and fill in your configuration.
  - Start a local PHP Server with: `php -S localhost:8080 -t webroot`
  - Create some keys on Redis. You can see the commands with the content [here](samples/redis_commands). A faster alternative is to run `php create_redis_keys.php`.
  - Make your first successful request with: `curl -XGET --cookie 'app=hash' localhost:8080`.

## How it works

### Request/Response cycle

It receives a request with a cookie from a logged in user. With that cookie we retrieve the current user's session which contains the user ID.
That user ID is used to retrieve the user's current friends list stored in Redis. This key contains a serialized instance of `app\domain\chat\FriendsList`. That instance is then filled with presence status of the relevant users (also stored in Redis).

### Possible outcomes

- [HTTP 200 OK](samples/response_200): request was valid, return the JSON representation of your `FriendList`.
- [HTTP 403 Not Authorized](samples/response_403): request was not authorized due to a missing cookie, an invalid session or a bad referrer domain.
- [HTTP 404 Not found](samples/response_404): friends list is not available.
- [HTTP 500 Internal Server Error](samples/response_500): bad app configuration or Redis is down.

### Required domain knowledge

You shouldn't pay much attention to what `app\domain\chat\FriendsList` class does. It's just a data transport object. Those cache keys are generated by other processes.

## What we would like you to do?

Download this repo. Code it your way. Think how would you solve this problem using modern PHP components without a lot of overhead.

- Microframeworks could be used, but we don't believe are a must given the simplicity of the task at hand.
- Some Request/Response interfaces for standardization would be nice :-)
- Unit testing is mandatory.
- Although we love other languages too, we prefer if you stick to PHP, as it is Workana main language.

<p align="center"><a href="https://camo.githubusercontent.com/b59cb77c4263f7da915a5deb6006ff9c5d62ac25/68747470733a2f2f7170682e69732e71756f726163646e2e6e65742f6d61696e2d71696d672d6135653565336334363135666135363461373231343338646637363732663039" target="_blank" class="rich-diff-level-one"><img src="https://camo.githubusercontent.com/b59cb77c4263f7da915a5deb6006ff9c5d62ac25/68747470733a2f2f7170682e69732e71756f726163646e2e6e65742f6d61696e2d71696d672d6135653565336334363135666135363461373231343338646637363732663039" alt="challenge" data-canonical-src="https://qph.is.quoracdn.net/main-qimg-a5e5e3c4615fa564a721438df7672f09" style="max-width:100%;"></a></p>

## Submission

 Please don't submit Pull Requests. After you're done, please send an email to [federico+hiring@workana.com](mailto:federico+hiring@workana.com) with the link to your fork so we can start talking =)

Thanks a lot and happy coding!

## Disclaimer

THIS IS NOT FREE WORK. We have already found a solution to this problem. Your proposal will only be used to evaluate the quality of your work and resourcefulness of your solution.
