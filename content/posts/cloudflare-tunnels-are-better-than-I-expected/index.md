---
title: Cloudflare Tunnels are better than I expected
keywords: Cloudflare, Tunnel, Cloudflared, Home Assistant, Cloudflare Tunnels
date: 2024-04-19
description: "I found a new feature of Cloudflare tunnels that makes it super easy to expose services within your network, without having to change any configurations."
draft: false
lastmod: 2024-04-20
---
# Cloudflare Tunnels are better than I expected
I was first introduced to the concept of tunnels in one of my Big Data and Cloud Computing classes by the professor, using Ngrok to expose a service running in my local laptop so that my classmates could access it by simply using a link, without the need of complex configurations.

This was very impressive to me, because I knew that if you wanted to expose anything from your local computer, you would have to configure firewall rules and, more importantly, change configurations in your router to open the required ports and send the data the correct way, like for setting up a Minecraft server. And that wouldn't even work in a situation like mine where the public IP of my router isn't the same as the public IP that websites detect from me; the ISP sets an internal "public" IP to the router but in reality it is still within an list of private IPs that the ISP then forwards your requests to. So you are stuck in a private NAT. Or something like that, I'm not an expert on the subject but that's the basic idea on why exposing a server via port forwarding is pretty much impossible here. For more information, you can read about it here (in spanish): https://www.tapatalk.com/groups/forodetelevisionporcable/izzi-deja-de-dar-ip-homologada-t55605.html

So the ability to just bypass all the need for that configuration was amazing to me, and I was looking forward to using Ngrok to expose my services whenever I would finish Uni and have time to make my server and mess around with anything I wanted.

Fast forward and by the time I finish Uni and build my server, Ngrok is now a paid service only. Not very surprising since that seems to be the trend now: start with a free service, get users and trust, start an enterprise tier, remove the free tier. Not surprising but disappointing, and I was left looking for other tunnel service that would be a good fit for me.

By this point, I already had my domain with Cloudflare, and I learned about cloudflared or Cloudflare Tunnels, which is a tunnel service that allow you to use subdomains within your domain as the entry point for the service you are exposing with your tunnel. This was a perfect fit for me, since I was already paying for the domain and having tunnels comes at no additional cost, plus I like the aesthetics of having a service being a subdomain instead of a path in your URL (I like service.domain.com instead of domain.com/service)

Setting them up is super easy too, you just have to go to the page for Cloudflare Tunnels, configure the tunnel with the information you want, and just paste the resulting command in the terminal of the host that has the service you want to expose. This is a quick post so I won't go into detail about the process of installing Cloudflare tunnels since there are already tons of resources, but let me know if you would like a guide from me in that matter.

## The amazing part
However, I did make an oversimplification in the previous paragraph, since you only really need to install Cloudflare tunnels into a machine within the network you want to expose, not necessarily the host that has the server. This seems like a weird distinction to make, but what this means is that you can expose ANY service from the network just by installing the tunnel into one of the machines in the server, and without having to touch the different services in order to expose them as a subdomain in your domain.

And it really is just that simple:
1. In the page for cloudflare tunnels you select the tunnel and click the "Configure" button
![Image](</Pasted image 20240420152326.png>)
2. Then click on "Public Hostname"
![Image](</Pasted image 20240420151805.png>)
3. In this screen you can see the active subdomains with tunnel. To create a new one, click on "Add a public hostname"
![Image](</Pasted image 20240420151952.png>)
4. And then just specify the URL that you use to access that service within the network
![Image](</Pasted image 20240420152211.png>)

This is an incredible abstraction because it allows you to ignore the way the service is set up and it's specific configurations, you just have to specify how you access it and it magically gets tunneled.

I have a Home Assistant instance running in my server. The way it is set up is that I have my Proxmox server. This server has a Linux Container (LXC) that is running Portainer, and with Portainer I'm running a container that is running the instance of Home Assistant Core. As you can see, it isn't a very straightforward configuration, and I was really wondering how I could install the tunnel to expose the home assistant instance and configure it considering the way it is set up, since doing a manual configuration is quite tricky.

However, by just specifying the URL I access Home Assistant from I could completely ignore the complicated configuration and in a blink of an eye I had exposed my Home Assistant instance. And I was so impressed by how intuitive it was that I just HAD to talk about it.

Note: to expose Home Assistant with Cloudflare Tunnels you need to configure the trusted proxies of Home Assistant, more info here: https://community.home-assistant.io/t/cloudflare-400-bad-request-error/326047/6

---

So in conclusion, Cloudflare may very well be a CIA op, but with incredible services like these I can't help but keep using it.