# VPS CN2 GIA Hong Kong: Is Evoxt's Hong Kong Node Real CN2 GIA, What Genuine GIA Costs, and How to Check Return Routes Before You Pay

Evoxt's Hong Kong VPS starts at **$2.99/month** for 1 vCore, 512 MB RAM and 5 GB NVMe on a 1 Gbps port, and nothing on Evoxt's Hong Kong page or its pricing page claims CN2, CN2 GT, or any China-optimized return routing. If a verified 59.43 return path at 9pm is what you came for, Evoxt is not that product, and $2.99 will not buy you out of the evening collapse you are trying to escape. What Evoxt sells is cheap, high-clock KVM capacity across 16 locations with a gigabit port and free weekly offsite backups. The rest of this page covers the full plan table with verified prices, what real GIA costs at the entry level elsewhere, and the mtr checks that tell you which of the two you are holding.

> **Quick pick:** Decide one thing first. If your users sit in mainland China and your evening traffic cannot afford packet loss, budget for a genuine GIA line and expect to pay for it. If you need a fast, cheap Hong Kong box for a small site, an API endpoint, a bot, or a staging environment, take **VM-1 (1 vCore, 2 GB RAM, 20 GB NVMe, $5.99/month)**. It doubles the RAM of the $2.99 plan for $3 more and leaves room for a real stack. 👉 [Deploy VM-1 and choose Hong Kong at checkout](https://bit.ly/EvoXt)

## Evoxt Hong Kong plans and prices

All 11 plans currently listed on Evoxt's pricing page are below. Every one runs on KVM with a CPU clock of up to 6.0 GHz, a 1 Gbps port, 1 IPv4 plus IPv6, and a free weekly offsite backup. Prices are USD, billed monthly.

| Plan | vCPU · RAM · NVMe | Monthly transfer | Price | Order |
| --- | --- | --- | --- | --- |
| VM-0.5 | 1 vCore · 512 MB · 5 GB | 250–500 GB | $2.99/mo | [Deploy VM-0.5](https://bit.ly/EvoXt) |
| VM-0.75 | 1 vCore · 1 GB · 10 GB | 250–750 GB | $4.99/mo | [Deploy VM-0.75](https://bit.ly/EvoXt) |
| VM-1 | 1 vCore · 2 GB · 20 GB | 500–1,000 GB | $5.99/mo | [Deploy VM-1](https://bit.ly/EvoXt) |
| VM-1.5 | 2 vCores · 2 GB · 20 GB | 500–1,500 GB | $6.95/mo | [Deploy VM-1.5](https://bit.ly/EvoXt) |
| VM-2 | 2 vCores · 4 GB · 30 GB | 1,000–2,000 GB | $11.99/mo | [Deploy VM-2](https://bit.ly/EvoXt) |
| VM-3 | 4 vCores · 4 GB · 30 GB | 1,000–3,000 GB | $14.99/mo | [Deploy VM-3](https://bit.ly/EvoXt) |
| VM-4 | 4 vCores · 8 GB · 60 GB | 2,000–4,000 GB | $23.99/mo | [Deploy VM-4](https://bit.ly/EvoXt) |
| VM-6 | 8 vCores · 8 GB · 60 GB | 2,000–5,000 GB | $29.99/mo | [Deploy VM-6](https://bit.ly/EvoXt) |
| VM-8 | 8 vCores · 16 GB · 80 GB | 3,000–6,000 GB | $47.99/mo | [Deploy VM-8](https://bit.ly/EvoXt) |
| VM-12 | 16 vCores · 16 GB · 80 GB | 3,000–8,000 GB | $60.95/mo | [Deploy VM-12](https://bit.ly/EvoXt) |
| VM-16 | 16 vCores · 32 GB · 100 GB | 5,000 GB–10 TB | $95.99/mo | [Deploy VM-16](https://bit.ly/EvoXt) |

The transfer column carries a range because the pricing page publishes two quota tiers for the same plans. VM-1 gets 1,000 GB in the higher tier and 500 GB in the lower one. Quotas are tied to the location you deploy into, so confirm the Hong Kong figure on the order form rather than assuming the larger number.

Things that matter outside the table:

- **Same plan, 16 locations.** Hong Kong, Osaka, Tokyo, Seoul, Kuala Lumpur, Jakarta, Sydney, Los Angeles, New York, Montreal, London, Paris, Amsterdam, Frankfurt, Warsaw, Zurich. You pick the region at deploy time.
- **Annual billing is 12× monthly, with no discount.** VM-0.5 works out to $35.88/year, VM-1 to $71.88/year. Prepaying a year saves nothing on the sticker price, which makes monthly the sensible start if you have not yet tested the route.
- **Extra IPv4:** $3/month each, ordered separately from the console.
- **Panel features included at no extra cost:** monitoring, firewall rules you can set without SSH, VM cloning, sub-accounts, VNC console, IP reassignment between servers.

## CN2 GIA, CN2 GT, and plain BGP: what changes at 9pm

CN2 is China Telecom's next-generation backbone, AS4809, using the 59.43.x.x range and physically separate from the 163 backbone (AS4134) that carries most consumer traffic. The tier you buy decides how much of that separation you actually get.

**CN2 GIA (Global Internet Access)** keeps traffic on 59.43 nodes end to end, outbound and return. **CN2 GT (Global Transit)** uses CN2 only for the international segment and drops back onto 202.97 nodes inside mainland China, so packets still queue with ordinary consumer traffic on the domestic leg. **BGP** is a different approach: the data center buys from several carriers and picks paths dynamically, and the result is only as good as the bandwidth contracts behind it. A BGP box built on plain 163, 4837 and CMI performs nothing like one built on GIA, 9929 and CM2, even though both pages say "BGP optimized."

Checking takes one command on the VPS itself, looking at the return path rather than the outbound one:

1. Run `mtr -r -c 50` against a mainland IP, or `traceroute` if mtr is not installed.
2. Continuous 59.43 nodes in the return path means GIA. Any 202.97 hop means 163 is in the mix, which makes it GT or a half-measure with GIA outbound and 163 coming back.
3. For China Unicom, look for 218.105 or 210.51. For China Mobile, look for 223.120 or 221.176.

A "1 Gbps port" line on a spec sheet says nothing about any of this. Port speed is the size of the pipe at the data center; route quality is who carries the packets between that pipe and your users. Evoxt advertises the former and makes no claim about the latter, which is the honest reading of its product pages.

## The peak-hour test that decides whether you overpaid

Evening congestion between 20:00 and 23:00 is the whole reason GIA costs what it costs. Published comparisons put 163 packet loss above 20% during the peak, CN2 GT around 5%, and CN2 GIA under 0.5%. A 3% to 5% loss rate is already enough to break a video call, drop remote desktop sessions, and make pages time out. That is the mechanism behind the familiar complaint that a server benchmarks fast at noon and turns unusable after dinner.

Latency figures vary by source and by province, so treat them as ranges. Hong Kong GIA lands somewhere between 25 and 60 ms to mainland users, with Guangdong at the low end. Hong Kong GT sits around 45 to 70 ms. Los Angeles GIA comes in at 130 to 180 ms.

Whichever provider you shortlist, ask for a test IP and run the trace twice: once during the day, once between 20:00 and 22:00. A daytime trace tells you the route exists. The evening trace tells you whether it survives load. If a seller will not give you a test IP, that answers a different question for you.

## What "CN2 GIA" costs elsewhere, and why the spread is 20×

Three verified reference points show how little the label alone tells you:

| Offering | Specs | Port | Price |
| --- | --- | --- | --- |
| BandwagonHost Hong Kong CN2 GIA (HKHK_8) | 2 GB RAM, 2 cores, 40 GB NVMe, 500 GB transfer | 1 Gbps | $89.99/mo or $899.99/yr |
| BandwagonHost CN2 GIA-E, Los Angeles | 1 GB RAM, 20 GB SSD, 1 TB transfer | 2.5 Gbps | $169.99/yr |
| DP Nexus Hong Kong, labeled CN2 GIA | 512 MB RAM, 1 core, 5 GB SSD, 500 GB transfer | 1 Gbps | $3.89/mo |
| Evoxt Hong Kong (no GIA claim) | 1 GB RAM, 1 vCore, 10 GB NVMe | 1 Gbps | $4.99/mo |

The BandwagonHost Hong Kong entry plan forces all three carriers back through CN2 GIA and sits in a single Hong Kong facility with no migration. Its annual price equals ten months of monthly billing, a real prepay discount. Evoxt's annual price equals twelve months. Neither fact makes one better; they tell you what kind of contract you are signing.

The $3.89 plan and the $89.99 plan both print "1 Gbps" and "CN2 GIA" on the page. What separates them is how much premium transit the operator actually bought, and how the box behaves when the whole country logs on at eight. Hong Kong GIA capacity is allocated in small slices, which is why most genuine GIA plans cap ports at 10 to 30 Mbps and prices climb steeply above 50 Mbps. A gigabit port paired with a GIA label at $3.89 a month is a claim worth testing with mtr before you renew anything.

Evoxt sits outside that argument entirely: it does not sell GIA, does not price like GIA, and should not be judged against it. At $4.99/month it costs roughly one eighteenth of the BandwagonHost Hong Kong entry plan. You are buying CPU clock, NVMe and a big port, not a guaranteed evening route. 👉 [Check Evoxt's current Hong Kong pricing and quotas](https://bit.ly/EvoXt)

## Which Evoxt plan matches which job

**A single small site, a proxy endpoint, DNS, uptime monitoring, a Telegram bot:** VM-0.5 at $2.99 is enough, and VM-1 at $5.99 is the comfortable version. Neither needs a gigabit port, and neither will notice the absence of GIA.

**WordPress with a cache layer, a small API, a game server for a handful of players:** VM-1.5 or VM-2. The 6.0 GHz clock helps here more than core count does, because PHP and most game servers care about single-thread speed. 2 GB of RAM is the practical floor for a database plus a web server without swap thrash.

**Docker with several services, a small database, a build runner:** VM-4 with 8 GB and 60 GB of disk. Below that you will be pruning images monthly.

**A team running a China-facing product where the 20:00 to 23:00 window is revenue:** none of these. Buy a plan with a documented GIA return path, get the test IP first, and accept a 10 to 30 Mbps port as the normal trade for that routing. A 20 Mbps GIA line will hold a remote desktop session through the evening; a 1 Gbps port on a congested route will not.

**A personal blog with readers who drop by whenever:** a $2.99 Hong Kong box is already more than the job requires. Paying $89.99 a month to shave 40 ms off a page load nobody is timing is money you will not recover.

## Limits worth knowing before you pay

Evoxt's pricing page does not publish a refund window. Confirm the terms with support in writing before you prepay a year, especially since annual billing carries no discount to soften a mistake.

The plans are location-agnostic at purchase, so Hong Kong is a choice you make on the deploy screen, not a separate SKU. Whether an existing VM can be relocated to another region afterwards is not stated on the product or pricing pages, so ask before you build something you might want to move.

Support is self-managed. You get a clean Linux or Windows install, a panel, and a VNC console; you do not get WordPress installation help or application troubleshooting.

On the GIA side of the market, DDoS handling is a real constraint rather than a footnote. Premium China transit is capacity-limited enough that operators commonly null-route an attacked IP instead of absorbing the flood, which takes the server offline for the duration. If your workload attracts attacks, ask any provider how they handle it before you commit.

## FAQ

**Can I move a VM between locations later?** Evoxt sells the same plan in 16 regions and lets you choose at deploy. Relocation of a running VM is not documented on the pages reviewed, so confirm with support.

**Does Evoxt take Alipay?** Yes. Alipay, PayPal, credit card and cryptocurrency are all accepted.

**Do I get a dedicated IPv4?** Each VM includes 1 IPv4 and 1 IPv6. Additional IPv4 addresses cost $3/month.

**How long is the refund period?** No refund window is published on the pricing page. Ask before paying annually.

**Will a Hong Kong IP unlock streaming services?** Evoxt does not document streaming unlock behavior. Do not buy on that assumption.

**Do I need CN2 GIA for a personal blog?** No. GIA buys stable latency under evening load, which matters for real-time applications and revenue-bearing China traffic. A blog with a CDN in front does fine on an ordinary route.

## How to decide

If your workload is a site, an endpoint, a bot, or a dev box and your users can tolerate normal evening congestion, take Evoxt and start at the low end: VM-0.5 for a single tiny service, VM-1 for anything with a database, VM-4 once containers appear. Monthly billing costs the same per month as annual, so test for a month before committing. 👉 [Start with VM-0.5 at $2.99/month](https://bit.ly/EvoXt)

If your users are in mainland China and the 20:00 to 23:00 window decides whether they stay, Evoxt is the wrong purchase at any price, and no amount of RAM compensates for a return path that falls back to 202.97. Buy a plan that sells GIA explicitly, request the test IP, and run the trace at night before you pay.

Two things to confirm before either order goes through: the Hong Kong transfer quota your plan actually receives, and the refund terms in writing. 👉 [Pick a plan and choose Hong Kong at deploy](https://bit.ly/EvoXt)
