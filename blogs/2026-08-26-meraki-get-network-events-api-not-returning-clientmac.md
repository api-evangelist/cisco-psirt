---
title: "Meraki Get Network Events API - not returning clientMac"
url: "https://community.cisco.com/t5/network-platform-api/meraki-get-network-events-api-not-returning-clientmac/m-p/5571400#M9697"
date: "2026-08-26"
author: "PWJPW"
feed_url: "https://community.cisco.com/kxiwq67737/rss/Category?category.id=4409j-developer-home&interaction.style=forum"
---
We have an org with 15 networks. We're calling the following endpoint to pull some wireless event detail (client associations): https://developer.cisco.com/meraki/api-v1/get-network-events/ However, one a few of our networks (but not all), the clientMac is null on the returned data, whereas on most networks, its populated as documented. Before I raise with support, does anyone know of any reason why?
