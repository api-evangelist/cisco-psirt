---
title: "Terraform cannot connect to APIC API in ACI Always-On Sandbox"
url: "https://community.cisco.com/t5/devnet-general-discussions/terraform-cannot-connect-to-apic-api-in-aci-always-on-sandbox/m-p/5579627#M3079"
date: "2026-10-01"
author: "hara-mc"
feed_url: "https://community.cisco.com/kxiwq67737/rss/Category?category.id=4409j-developer-home&interaction.style=forum"
---
Hello, I am using the ACI Always-On Sandbox for Terraform testing. Recently, API access appears to be unavailable although the APIC GUI is accessible. Environment: - Terraform 1.15.8 - Cisco ACI Provider 2.20.0 Observed behavior: - APIC GUI is accessible - Terraform login fails - REST API login fails - Test-NetConnection to port 443 succeeds Could you please confirm whether this is a known issue related to current DevNet Sandbox maintenance?
