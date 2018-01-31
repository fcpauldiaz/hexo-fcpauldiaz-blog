---
title: Building a Parallel Scraper 😜
date: 2018-01-31 11:31:47
tags:
    - scraper
    - python
public: true
---
#The Why

Simon Sinek always says that people first care about the why and then about the how. So here it is. A few months ago, some friends that sell products on Amazon had a business idea. They said that among Amazon sellers there is the problem that the Amazon algorithm (known as [A9](https://www.a9.com/)) can change from one day to another without previous notification and that can lower the sales significantly because the keywords configured can stop being indexed. So they have to be aware when this happens. To solve this problem the current market uses some indexing services that tells you the indexing percentage of a product, so my friends had the idea that this still was tedious and they wanted to build an automated service that will check for this everyday and only notify when the indexing percentage reaches certain configured level. So how we will do this? Scraping is part of the solution.

#The How

First we have to consider that having a business model based on scraping is very dangerous so we don't want to spend a lot of time developing this *side project* and we don't want to rely 100% on scraping. I chose python because it is great for scraping, so I for the web app I used Django. Now for the scraping I used beautiful soup and for the parallel processing I used [Celery](http://celery.readthedocs.org/), with RabbitMQ as the message broker and Redis to store temporary the results. So what Amazon does to prevent scraping? First, they block the IPs that make more than 1 request per second, so I had to use Storm Proxies to change the IP for every single request. That should be enough right? Well it isn't enough, Amazon recognises when a request doesn't have a cookie  so when multiple requests are made, they show a captcha. So is it possible to generate a cookie? Well, Amazon signs their cookies so it is quite impossible to create one or at least by me. If a captcha is shown we use an Amazon API to get the data we need, so we don't rely one hundred percent on scraping. Scraping Amazon has been quite a challenge, the service to change IP for every single request adds a delay of almost two seconds, so it is fully necessary to make it parallel because if we don't it would be two seconds for every keyword, instead it is two seconds for maximum 250 keywords or at least that's how it should theoretically. Sometimes one keyword lasts more than a minute and that's too much time for a user, so it is necessary to add timeouts and rate limit to not overload the server.

Currently we have 243,431 keywords indexed and adding around 14k per day and remember, it is one request per keyword.

If you want to check the service here it is: [Check My Keywords](https://checkmykeywords.com)
