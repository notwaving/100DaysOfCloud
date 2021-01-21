# Fightin' 🔥🔥🔥 and Wranglin' 🤠

## Cloud Research

This morning I woke up to this...
![exposed-key](/Journey/068/exposed-key.png)
...because _someone_ (i.e. _not me_) decided to commit our keys to GitHub (on an AWS account with MY credit card details on it). Thankfully times have changed, and the _right_ bot found it and put our key in quarrantine till it could be sorted out

Last thing I've done today is revoke permissions for _someone_ who keeps pushing directly to the master branch - and therefore our live site. Or at least I've created a rule for the branch in GitHub...

Meanwhile, I put some slides together and rehearsed a short presentation for how I deployed our React app to AWS - my first time actually talking out loud about cloud architecture!

Didn't realise that CloudFront only updates the cache once per 24 hours. But because someone was simultanously cowboying their broken code straight into the prod bucket

![don't do this](https://media.giphy.com/media/11VKF3OwuGHzNe/giphy.gif)

it took more time to diagnose and fix the problem. CloudFront cache is now invalidated. Good old Cloud Resume Challenge!

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1352044412587675648?s=20)
