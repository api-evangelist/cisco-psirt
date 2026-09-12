---
title: "No SPACE_OCCUPANCY events from Kontakt.io Portal Beam"
url: "https://community.cisco.com/t5/internet-of-things-iot-management-automation/no-space-occupancy-events-from-kontakt-io-portal-beam/m-p/5572897#M16"
date: "2026-09-03"
author: "ch.jauslin"
feed_url: "https://community.cisco.com/kxiwq67737/rss/Category?category.id=4409j-developer-home&interaction.style=forum"
---
We are using a Kontakt.io Portal Beam as an occupancy sensor in Cisco Spaces, assigned to a room with occupancy type PEOPLE_COUNT. Our requirement is to receive a Firehose API event whenever room occupancy changes (0 → >0 and >0 → 0). We successfully receive IOT_TELEMETRY events (temperature, battery, accelerometer, illuminance, etc.) from the Portal Beam, but no SPACE_OCCUPANCY and SPACE_OCCUPANCY_CHANGE were not available in our Firehose event type selection.
