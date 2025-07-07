# Self-hosting with Cloudflare Tunnels

Ok, I think we all know that feeling when you have some RasPi or similar thing lying around somewhere in a drawer. I mean, it's not complete junk, no powerbeast either - but it went through some life cycle like: let me try to make XY -> buy RasPi -> ok, it works, but it's kinda slow, this and that is missing -> Oh hey, a new RasPi version came out -> buy new RasPi with more RAM (the old one goes into the drawer among cables and electronic mess).

Or you're just looking for some other use for your homelab / mini computer. Just like me. So I decided to turn it into hosting for my own software.

That means having my SW available publicly and reachable from anywhere in the world. Simply everywhere else than on localhost or home LAN.

## Isn't it easier to pay for a cloud VPS?

Well yeah, but I don't want to burn too much money with it - I already have the little RasPi4, it won't get me broke on energy costs either and I'm updating it regularly anyway, so it doesn't make sense to pay for cloud just for a few apps.

## Ok, so VPN then?

Hold up! VPN is definitely an option. I'd have to set up the router to either handle the VPN server or tunnel a port to the RasPi where I'd handle it.
Then there's the security issue - in principle, through VPN I get access to the entire LAN network, unless I do some hardcore networking magic with VLANs. Plus I'd have to install VPN clients on my devices. Meh. But something like Wireguard did cross my mind too, not gonna lie.

## So how about that Cloudflare?

The first thing (which I'd already done before for a different reason) was transferring the DNS server and domain from Vedos (where I have the domain registered) to Cloudflare. That's pretty easy (you add domain and click through it) and now I can add more subdomains.

### Step 1: Set up cloudflare tunnel

In Zero Trust in the Cloudflare dashboard, I click on a new tunnel, it generates some registration token for me.

On the RasPi I download the cloudflared binary and enable it as a systemd service, so it runs in the background as a daemon. With the command from tunnel settings, I register the tunnel.

So now I have a tunnel that handles traffic between RasPi and Cloudflare infrastructure.

### Step 2: Adding application to Cloudflare

In Zero Trust / Access / Applications I add a new Application. I click on Self-hosted and go to settings.

Sure, first I name it. Then I do `+ Add public hostname` - there I create a subdomain for my Cloudflare-managed domain. This is where it'll all be accessible. So something like `dev.mydomain.com` or come up with something original.

Next comes the super advantage of Cloudflare - security. So that not just anyone sneaks in there, I definitely recommend setting up authentication. There's classic OAuth, so you can connect login through Google, Github, etc. in _Manage login methods_. Each method is set up differently, but you have instructions on what to click and set up where. Then you just select your newly created login method and if it's the only one selected, you can check to skip the method selection screen during login (you can easily have multiple login options available for one application).

Don't forget to also set up a policy in _Access Policies_ for who should actually be allowed into that application. I put my email there, so OAuth login token with my email authorizes it and lets it through to the application. Access with different email gets a boot.

You can play around with other settings too, but this minimum should be enough for secure access. We're almost there.

### Step 3: **Tie it all together**

We go back into Networks / Tunnels and select our tunnel and hit _"Edit"_.
At the top there's a _"Public hostnames"_ tab. You click the blue button to add another public hostname. There you just set up the pairing of your application under the given subdomain (that `dev.mydomain.com`) and the address from the perspective of the other end of the tunnel (on RasPi). So like `http://localhost:8080` or whatever.

Here's a note that will save you time and nerves - **forget about HTTPS on the RasPi side, Cloudflare will take care of the certificate.** I tried to set up Caddy with HTTPS and it was absolutely unnecessary PAIN. In the sense that certificates were fighting each other, I had to add exceptions to Caddy configuration, etc, etc.

## Done?

Yeah, that's how it works for me. I have two subdomains on one tunnel, distinguished by port, on one I run Home Assistant, on the other reverse proxy for my applications. Running on RasPi through Docker with bridged network.

## What does it cost?

I have this whole workflow for free. Sure, it's quite likely that if I had lots of domains or generated heavy traffic, it would probably cost some buck already, but I'll probably never reach that. Plus I don't even need any super detailed dashboards about traffic, of which there are also a few behind the paywall.

The only thing it costs you is the fact - that it's actually vendor-lock. You're using services and infrastructure of a third party and when they decide tomorrow to cut off the number of tunnels or applications for non-paying users, you can't do anything about it.

On the other hand - it's probably the easiest and safest way to set up this own server. Look - I'm not saying it's the best thing in the world, just that the "price/time/performance" ratio is awesome and if you have bigger demands, only then look for something else.


VK, 07/06/2025
