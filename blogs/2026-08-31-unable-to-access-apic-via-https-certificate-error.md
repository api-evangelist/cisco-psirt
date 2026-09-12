---
title: "Unable to access APIC via HTTPS — certificate error"
url: "https://community.cisco.com/t5/controllers/unable-to-access-apic-via-https-certificate-error/m-p/5572294#M3389"
date: "2026-08-31"
author: "tano.patrick1"
feed_url: "https://community.cisco.com/kxiwq67737/rss/Category?category.id=4409j-developer-home&interaction.style=forum"
---
I am connected to the DevNet Sandbox VPN and can reach the APIC at 10.10.20.14. Direct HTTPS access was successfully tested using: curl.exe -vk https://10.10.20.14 The APIC responds successfully with: HTTP/1.1 200 OK Server: Cisco APIC I also tested: curl.exe -vk https://10.10.20.14/api/aaaLogin.json The APIC responds with: HTTP/1.1 400 Bad Request Failed to parse login request This is expected since the request was sent as GET without login credentials. Therefore, VPN connectivity, routing, TCP/443 and the APIC HTTPS service are working.
