---
title: "Nexus 3548P-XL: BIOSINFO Checksum Error Persists in L2 Mode"
url: "https://community.cisco.com/t5/nexus-devices/nexus-3548p-xl-biosinfo-checksum-error-persists-in-l2-mode/m-p/5579134#M566"
date: "2026-09-29"
author: "hsjung"
feed_url: "https://community.cisco.com/kxiwq67737/rss/Category?category.id=4409j-developer-home&interaction.style=forum"
---
Hello, The following log message is occurring on a Cisco Nexus 3548P-XL (N3K-C3548P-XL) switch: %KERN-3-SYSTEM_MSG: biosinfo checksum failed expected ff Got c - kernel We referred to the Cisco TAC documentation related to CSCve63984 and performed the recommended workaround: configure terminal fan speed set 60 fan speed default end However, the same log message continues to appear after performing the workaround. The affected switch is currently running NX-OS 10.3(6) and is operating as an L2 switch in a production environment. Interestingly, we are also using the same Nexus 3548P-XL model with
