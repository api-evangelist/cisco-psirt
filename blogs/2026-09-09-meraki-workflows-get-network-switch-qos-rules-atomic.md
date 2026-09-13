---
title: "Meraki - Workflows - Get Network Switch QoS Rules atomic"
url: "https://community.cisco.com/t5/devnet-general-discussions/meraki-workflows-get-network-switch-qos-rules-atomic/m-p/5573890#M3060"
date: "2026-09-09"
author: "5ayasheem"
feed_url: "https://community.cisco.com/kxiwq67737/rss/Category?category.id=4409j-developer-home&interaction.style=forum"
---
Direct question: Does the "Get Network Switch QoS Rules" atomic return the QoS rules in the order in which they are processed by the MS/switch? Situation: I'm in a scenario where i need to compare QoS rules across all switch networks, that includes the rules themselves as well as their order. I can use the "Get Network Switch QoS Rules" atomic to check for the rules' parameters & the "Get Network Switch QoS Rules Order" in order to check for the order through the returned IDs.
