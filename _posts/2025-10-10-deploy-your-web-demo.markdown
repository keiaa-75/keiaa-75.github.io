---
title: "Deploy your Web Demo"
description: Want to share your in-progress web app with teammates or clients?
author: keiaa
date: 2025-10-10 16:50:00 +0800
categories: [Notes, Development]
redirect_from: /web-demo/
tags: [web, technology, software, development, resources, foss]
---

You've built something cool. A quiz app, a full-stack project dashboard, a dynamic website, you name it. Now, you want to show it to your teacher, your groupmates, or even your family. But when you Google "how to put my website online," you get buried under terms like CI/CD, Docker, VPS, and DNS records.

## The usual way

> This is gross oversimplification. Though these are common main points for such deployments, real-world tendencies are more complex.
{: .prompt-info }

Trust me when I say it gets more deeper. Here's what most guides tell you to do:

- [x] Develop your web app (of course, what's there to do)
- [ ] Set up a CI/CD pipeline or containerize it with Docker
- [ ] Rent or get a public server (on AWS, DigitalOcean, etc.)
- [ ] Buy a custom domain
- [ ] Point that domain to your server

Hits close to home? We don't really need to go that deep. Not for a demo, not for testing, and definitely not for a class presentation. It's an overkill, a classic case of over-engineering. You're in school, not running a startup (yet). 

Of course, there ought to be a simpler path.

## Here comes Cloudflare

Cloudflare offers a free service called TryCloudflare that lets you expose your local web server to the internet instantly, with no setup, no payment, and no domain needed.

It works using Cloudflare Tunnels, powered by a lightweight program called `cloudflared`.

**How it works (in plain English)**

1. You run your web app on your computer (e.g., `http://localhost:8080`).
2. You start `cloudflared`, which opens a secure tunnel to Cloudflare's network.
3. Cloudflare gives you a temporary public URL like `https://fancy-cat-123.trycloudflare.com`.
4. Anyone with that link can view your app, even if they're on a different network.

Your computer never opens a port to the public internet. All traffic flows securely through Cloudflare. No firewall changes necessary to get across.

## Deploy your demo

**1. Install `cloudflared`**

Open Windows Terminal (or Command Prompt in Windows 10) and run the following command.

```powershell
winget install --id Cloudflare.cloudflared
```

**2. Launch the tunnel**

Run your application on `localhost`. Then, in a new terminal tab, run:

```powershell
cloudflared tunnel --url http://localhost:YOUR_PORT
```

Replace `YOUR_PORT` with the actual port that your project is running on (e.g., `8080`).

**3. Grab your public URL**

After a few seconds, you'll see a line like:

```text
INF Requesting new quick Tunnel on trycloudflare.com...
INF +--------------------------------------------------------------------------------------------+
INF |  Your quick Tunnel has been created! Visit it at (it may take some time to be reachable):  |
INF |  https://pal-reviews-texture-means.trycloudflare.com
```

> Keep the terminal window open. Closing it shuts down the tunnel!
{: .prompt-warning }

You may now copy that URL and share it with anyone! 

## Frequently asked questions

**❓ Is TryCloudflare really free?**

Yes! No account, no credit card, no trial period. It's a public service from Cloudflare for developers to test quickly.

**❓ Is my computer safe?**

Very much so. Your server only makes outbound connections to Cloudflare. No one can directly access your machine. However, be careful of handling personal data and what you expose through your API.

**❓ Will it work with my framework?**

Yes! React, Vue, Svelte, Flask, Django, Express, PHP, plain HTML, you name it. It should be fine with anything that runs on `localhost` and speaks HTTP.

**❓ How long does the link last?**

Supposedly, until you close the terminal. The tunnel dies when `cloudflared` stops. Each time you run it, you get a new random URL.

**❓ What if too many people visit it?**

TryCloudflare allows up to 200 concurrent requests. For a class demo or peer review? More than enough. If you hit the limit, visitors see a `429 Too Many Requests` error.

## See more

- [Cloudflare Tunnel command-line client on GitHub](https://github.com/cloudflare/cloudflared)
- [Localtunnel, an alternative tunnel service, on GitHub](https://github.com/localtunnel/localtunnel)
- [My project's deployment automation script in Powershell](https://github.com/keiaa-75/voiz/blob/main/deploy.ps1)