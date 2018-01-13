![Image](/instagram.png)
<h1 align="center"> Auto like Instagram pics </h1>  

[![Build Status](https://travis-ci.org/brennan-v/auto-like-my-gf-insta-pics.svg?branch=dev)](https://travis-ci.org/brennan-v/auto-like-my-gf-insta-pics)  

Bot to automatically like your girlfriend's/friend's Instagram posts, and notify you on your Slack channel.

### Practical use cases

 - You don't have time to check social media and want to automate your likes for that special person in your life.
 - You are in a relationship. Your girlfriend is constantly nagging you for not being one for the first to like her Instagram pics*.

 *if this is a reoccurring issue, you should probably end that relationship.

How does it work?
================
This script uses the Instagram API to check every 15 minutes (via cronjob) for any new Instagram post for a paticular `USER_ID_TO_MONITOR`.  

**If a new a post is found**: it likes the post and sends a notification to your configured Slack channel using Slack Webhooks.

Prerequisites
===================
In order for this to all work, you'll need to do the following:

1. Go to https://www.instagram.com/developer/ and signup for developer access.
2. Register a new client (application). Make sure to set the company URL/redirect URI as "http://localhost".
3. Copy the following URL and replace the client_id with yours to generate a new access token. Afterwards just press enter to load the response page.

https://api.instagram.com/oauth/authorize/?client_id=CLIENT_ID_GOES_HERE&redirect_uri=http://localhost&scope=basic+likes+public_content&response_type=token

4. The response page will load an error (that's fine) but look at the address bar that will contain a new URL with your access_token. (Looks something like this - http://localhost/#access_token=###############)
5. Copy the access_token value into your .env file or the heroku enviroment config. This will be used as the value for `INSTAGRAM_ACCESS_TOKEN`.
6. Use the following website to get the instagram id of the person you want to auto-like. This will be used as the value for `USER_ID_TO_MONITOR`.

https://smashballoon.com/instagram-feed/find-instagram-user-id/  

7. Get your Slack Channel Webhook URL [Slack Webhooks](https://api.slack.com/incoming-webhooks).

Installation
===============

 - `git clone https://github.com/gulzar1996/auto-like-my-gf-insta-pic`
 - `npm install`
 - create a `.env` file with the following values (or rename the included .env.example to .env)
`INSTAGRAM_ACCESS_TOKEN = 'Random.String.123ABCD.EFG'`  
`USER_ID_TO_MONITOR = '123456789' `  
`SLACK_INCOMING_WEBHOOK_URL = 'http://URL.com' `

This file will assure that your keys are secured away from the index.js file is untouched.
 - `npm start` (run the app)

- After successful start, go to this URL to like the most recent instagram post.

     GET -> http://localhost/run

Deploy to Heroku
================
*You'll need to have the Heroku CLI installed*
 - Download the repo via zip or git clone.
 - `cd auto-like-my-gf-insta-pic`
 - `heroku login` (username/password)
 - `heroku create` (add heroku to project)
 - `git push heroku master` (deploy to heroku)
 - `heroku config:set INSTAGRAM_ACCESS_TOKEN=Random.String.123ABCD.EFG` (set access token)
 - `heroku config:set USER_ID_TO_MONITOR=123456789` (set user id to monitor)
 - `heroku config:set SLACK_INCOMING_WEBHOOK_URL=http://URL.com` (set slack webhook url)
 - `heroku ps:scale worker=1` (start dyno worker)

 Heroku will generate a url for you

      http://<HEROKU_URL>.herokuapp.com/run

Setting up Cron Job
===================

 - create an account [cron-job.org](https://cron-job.org/en/)
 - create a cronjob
 - paste the url `http://<HEROKU_URL>.herokuapp.com/run` in address
 - schedule every `15 mins`

 Using node-cron (local cron)
 ===============
 - `npm start` (run the app)
 - `node cron.js &` create a node-cron that sends GET to the app every 15 min
 - `ps` to list background processes
 - `kill <process id>` to stop the node-cron


Docker Setup
===================

 - Docker >= 17.x, docker-compose >= 1.x
 - Specify environment values in docker-compose.yml.
 - Run `docker-compose up`

 ## TODO
- [ ] Twitter Support
- [ ] Like only pictures of Gf/Bf (face recognition)


 Thanks
=================

Inspired from https://github.com/cyandterry/Like-My-GF. Code written in JS from scratch.