---
title: "Removing a service without restoring previous configuration"
url: "https://community.cisco.com/t5/nso-developer-hub-discussions/removing-a-service-without-restoring-previous-configuration/m-p/5578562#M8953"
date: "2026-09-25"
author: "dailycycle"
feed_url: "https://community.cisco.com/kxiwq67737/rss/Category?category.id=4409j-developer-home&interaction.style=forum"
---
Environment: NSO 6.4.9 I am seeking a way to remove our services from devices without restoring previous data. admin@ config)# no services stack_ntp rec-ntp admin@ (config)# commit dry-run cli { local-node { data devices { device cat8k-1 { config { ntp { - authentication-key 1 { - md5 { - secret aGoodKey; - type 1; - } - } - authenticate; - trusted-key 1 { - } server { + peer-list 10.10.10.10 { + key 1; + } - peer-list 172.16.0.2 { - key 1; - } - peer-list 172.16.0.11 { - key 1; - } } } } } } services { - ntp stacked_rec-ntp_cat8k-1 { - device-name cat8k-1; - } - stack_ntp rec-ntp { - device-n
