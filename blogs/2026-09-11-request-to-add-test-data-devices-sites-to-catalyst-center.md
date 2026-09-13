---
title: "Request to add test data (devices, sites) to Catalyst Center Always-On"
url: "https://community.cisco.com/t5/devnet-sandbox/request-to-add-test-data-devices-sites-to-catalyst-center-always/m-p/5574342#M12718"
date: "2026-09-11"
author: "fandre207"
feed_url: "https://community.cisco.com/kxiwq67737/rss/Category?category.id=4409j-developer-home&interaction.style=forum"
---
Hi Sandbox team, I'm using the Always-On Catalyst Center sandbox at sandboxdnac2.cisco.com to learn and test the Intent API (via Postman) with the standard devnetuser credentials. Authentication and API connectivity are working correctly — I can generate a token via /dna/system/api/v1/auth/token and successfully call GET endpoints. However, the instance currently appears to have no meaningful test data: /dna/intent/api/v1/network-device returns an empty list (no devices in inventory) /dna/intent/api/v1/site returns only the default Global site, with no additional hierarchy Additionally, the de
