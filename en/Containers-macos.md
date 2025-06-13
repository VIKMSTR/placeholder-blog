# Apple Container - Native Linux Containers on Mac?

When you want to work with OCI containers on Mac, you have several options for which tools to reach for.
Most people who want a convenient way to work with containers and are okay with the license will go for the traditional _Docker Desktop_. Those who might also want Kubernetes support go for _Rancher Desktop_ (which is among my favorites). (Not only) Red Hat fans are satisfied (despite occasional incompatibility with Docker) with _Podman Desktop_, and those who want something lighter and are perfectly fine with a CLI environment will use tooling like docker client + _(co)lima_.

## What's going on?

Now Apple is somewhat entering this space with their Container toolset. And watch out - I know this sounds weird coming from Apple, but it's probably really true - they released it with an open source license on GitHub  - [here](https://github.com/apple/container) and [here](https://github.com/apple/containerization). Apple fans are excited and talking about "finally native Docker/OCI container support on Mac". Okay, but what's the reality? And did hell really freeze over and Apple let the penguin into their system? And what exactly does soldier Kefalín imagine under the term "native"?

So let's calm down again, drink some water, and clarify one thing - native as in truly NATIVE without any virtualization layer - OCI containers on Mac will never happen. Deal with it.
And why? Because these containers are a "Linux thing". That means they have support directly in the Linux kernel (which btw containers share among themselves), which handles things like resource limiting using cgroups and so on. There's nothing like that in the Mac kernel.

>Small note - in principle, Mac, given its BSD-like origins, should be closer to a different containerization technology - namely "jails". This is something somewhat similar in the BSD world - except the isolation between "jails" is much greater, so they're much more secure, but also not as flexible in terms of deployment. Oh, and mainly they're not compatible with OCI containers. Just a different world. Don't ask me about them, I've never really met them.

## But containers work for me - just not natively?

So the way these "<YOURFAVORITEBRAND> Desktop" work is basically that they create some Linux virtual machine on your machine (on Windows they use the one from WSL via WSL2), which has docker/podman inside and runs those containers in it. That's why you have those sliders for number of cores and memory size in the Desktop app settings - you're assigning resources to that virtual machine :) Just like if you were creating a VM in VirtualBox. Colima does the same thing, just in CLI only - it starts a virtual machine that you then connect to with the classic docker client running on your host system.

And now Apple came along saying that maybe this isn't quite secure enough and that it consumes too much and something somewhere. And dropped the "Containerization framework" and Container CLI (very original naming) at WWDC 2025. And what does this actually do and mean? Well, it actually creates a virtual machine again, except it does it through some virtualization framework in Swift. Here's probably the confusion with the word "native" - since it's a library directly from Apple, we'll pretend it's native. But back to what's more important - this creates a virtual machine for each container separately. Let that sink in - it creates a new Linux virtual machine for each container, in which the OCI container runs.

Crazy, right?

## How is this supposed to be more secure and mainly - how is it supposed to consume less resources?

Apple boasts that these VMs are really lightweight - that there are no unnecessary libraries, no libc, they use musl (which we know from Alpine Linux) and that the overhead shouldn't be that big.
What must be acknowledged is somewhat higher security. Besides reducing the _attack vector_ by cutting out things we don't need (you can't attack me through a library I don't have in the system), the isolation between containers is suddenly much greater. When we share files with a container (mount them), we first pass the data to the virtual machine (which is shared by all containers) and only then it gets mounted directly into the container's filesystem. So some malicious container could get access to data that isn't meant for it. Here, file sharing happens directly only with the virtual machine assigned to the container.

>Note #2 - I remember the mental PAIN when I had to first share directories with VirtualBox for the given Linux virtual machine on the ancient Docker for Windows and only then mount them into containers. And of course you could only mount from these shared directories, so people typically handled this by sharing the entire D:/ drive so they wouldn't have to deal with whether the particular directory they're running the container in is accessible from VBox or not. Ugh. I'm glad that's gone.

## How do you work with it?

In reality, you work with it by first running `container system start`, which downloads the kernel, prepares virtual machine creation, networking and file sharing services, and starts the API. Then you use the CLI as you're used to with Docker: `container run`, `container exec`, `container logs`, `container ls`, ..
There's even a builder, so you can continue to normally build your images, only from `Dockerfile` but multi-platform. Plus it can of course access protected docker registries ("container registry login"), so you'll be able to push to your private docker registry server too. Btw - here's a nice guide on how to do it.

So - is it worth switching to Apple's new docker tooling? If you have _Apple Silicon_ on your machine and also the latest OS version (Stable with some limitations is enough, you don't need to download the beta Tahoe for this), then for such home experimentation it's probably fine. If you have a whole suite of containers in Docker that need to talk to each other or, God forbid, you use Docker Compose - then this doesn't make much sense for you yet and you can wait to see how it develops.

After all, it's only version 0.1.0 and Rome wasn't built in a day either.

VK, 6/11/2025
