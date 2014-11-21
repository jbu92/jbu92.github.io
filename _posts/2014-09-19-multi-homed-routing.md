---
layout: article
title: "Multi-Homed Routing"
description: ""
category: 
tags: []
comments: true
excerpt: "File that under 'huh. well, now I know how to do THAT.'"
---

#The Situation
We had a machine sitting on three different subnets, which needed to be pingable via all of them. Under its current setup, it was only pingable from one IP address at a time. I had to fix it.

The interfaces are set up as follows: eth0 is on the 172.16.0.0/19 subnet, eth1 is on 172.16.32.0/19, and eth2 is on 172.16.64.0/19. Our addressing convention uses 254 as the gateway.

#The Fix
The fix is super-simple, and there's a half-dozen blog posts exactly like this on how to achieve this end goal. For some reason, those posts are much longer than this one.

The current default gateway is 172.16.63.254, so we'll leave that alone.

We will, however, add 2 new routing tables to /etc/iproute2/rt_tables.

{% highlight bash %}
echo "250	eth0table" >> /etc/iproute2/rt_tables
echo "249	eth2table" >> /etc/iproute2/rt_tables
{% endhighlight %}

Now, we add a couple new routes:
{% highlight bash %}
ip route add default gw 172.16.31.254 dev eth0 table eth0table
ip route add default gw 172.16.95.254 dev eth2 table eth2table
{%endhighlight%}
And then a couple new routing rules:

{% highlight bash %}
ip rule add from 172.16.0.0/19 table eth0table
ip rule add from 172.16.64.0/19 table eth2table
{%endhighlight%}

And that's really all there is to it. Of course, for persistence, we then throw those rules and routes into /etc/sysconfig/rule-eth[0,2] and route-eth[0,2], but the idea here is really just to remind myself how to do this a year from now when I think "oh man, I did that once..."
