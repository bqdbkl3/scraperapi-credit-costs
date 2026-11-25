# Social media scraping API: what one scrape actually costs, is the free tier enough to test, and which platforms need premium proxies?

A LinkedIn request through ScraperAPI costs 30 credits. Add JavaScript rendering and premium proxies and the same request costs 55. A plain HTML page costs 1. That spread decides your bill far more than the monthly fee does, and every social platform you care about sits at the expensive end of it.

Below: ScraperAPI's full tier lineup, the per-request credit table, how many real social records the free credits buy, where a general-purpose API loses to a platform-specific one, and the terms-of-service split between vendors that will sell you Instagram data and vendors that won't.

> **If you're testing:** the free plan gives 1,000 API credits a month and 5 concurrent threads, no card. The 7-day trial gives 5,000 credits, which is the better way to size a paid tier before you commit to one.
> 👉 [Compare ScraperAPI's plans and current per-credit rates](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## ScraperAPI's tiers, and the column that matters most

Paid tiers are metered in credits, and the per-credit rate improves as you move up. Monthly and annual billing carry different rates, so the number you see at checkout depends on both the tier and the term.

The column worth reading before you pick is the overage behaviour, because it's the difference between a bill you control and one you don't. Auto-upgrade is opt-in and moves you to the next plan when credits run out. Pay-as-you-go keeps you on the same plan at a fixed per-credit rate, up to a monthly cap you set. Only the larger tiers get the second option.

| Plan | Price | When your credits run out | Get started |
| --- | --- | --- | --- |
| Free | $0 | Requests pause until credits renew; 1,000 credits/month, 5 concurrent threads | [Free plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Hobby | Per credit, monthly or annual | Opt-in auto-upgrade to the next tier | [Hobby plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | Per credit, monthly or annual | Opt-in auto-upgrade | [Startup plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | Per credit, monthly or annual | Opt-in auto-upgrade | [Business plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | Per credit, monthly or annual | Auto-upgrade or pay-as-you-go up to your cap | [Scaling plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | Per credit, monthly or annual | Auto-upgrade or pay-as-you-go up to your cap | [Professional plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | Per credit, monthly or annual | Pay-as-you-go up to your cap | [Advanced plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | Pay-as-you-go, custom terms | [Enterprise plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |

Read that table with one practical consequence in mind. If you start on Hobby, Startup or Business and hit the ceiling mid-month, your only in-plan escape is auto-upgrade, which moves you up a tier. If you expect spiky volume from day one and want to stay on a small plan, you need Scaling or above for pay-as-you-go. Annual plans can also renew early for a fresh batch of credits.

## One request is not one credit

This is where most price comparisons fall apart. Two APIs advertising the same monthly fee can differ by 50x in how many social records you actually get, because the unit you're buying isn't the request, it's the credit, and the credit price depends on the domain and the parameters you send.

ScraperAPI's published credit costs:

| Request type | Credits per request |
| --- | --- |
| Standard page | 1 |
| Amazon, Walmart, eBay | 5 |
| Google, Bing (including subdomains) | 25 |
| LinkedIn | 30 |
| Bot-protection bypass (Cloudflare, Turnstile, DataDome, PerimeterX/HUMAN, Akamai) | +10 |
| JS rendering (`render=true`) | +10 |
| Premium proxies (`premium=true`) | +10 |
| Premium proxies + JS rendering together | +25 |
| Screenshot (`screenshot=true`) | +10 |
| Ultra premium (`ultra_premium=true`) | +30, paid plans only |
| Ultra premium + JS rendering | +75, paid plans only |
| Geotargeting, sessions, device type, output format, auto-parsing | 0 |

Two things follow. First, the parameters that don't cost extra are the ones that make output usable: `autoparse=true`, `output_format`, `country_code`, `session_number` and `wait_for_selector` are all free. Turning on structured output costs you nothing; turning on a headless browser costs 10 credits.

Second, social targets stack. A LinkedIn page behind Cloudflare, fetched with rendering and residential proxies, is 30 for the domain plus 25 for the proxy-and-render combination plus 10 for the bypass. Whether that lands at 55 or 65 depends on whether the bypass fires, which is exactly why ScraperAPI gives you two ways to check before you spend: the Domain Cost Estimator in the dashboard (also available as an API endpoint) returns the credit cost for any URL with your parameters applied, and every response carries the actual charge in the `sa-credit-cost` header. You can also set `max_cost` per scrape so a single expensive URL can't quietly eat the month.

Budget with the estimator rather than with the plan's headline credit count. If your targets need rendering and premium proxies, divide your allowance by 25 before you decide whether a tier is big enough.

## How far the free credits actually go

The free plan's 1,000 credits and the trial's 5,000 credits sound generous until you apply the multipliers. At 25 credits per rendered, proxied request, 1,000 credits is 40 requests. On LinkedIn with the same settings, it's 18. The 5,000-credit trial buys 5,000 plain pages, 200 rendered-and-proxied pages, or about 90 LinkedIn records under the heavier configuration.

That's not a criticism of ScraperAPI specifically; it's what happens when a platform loads its feed with JavaScript and defends it with bot detection. Any vendor charging by consumption lands in the same place. It does mean you should pick your trial targets deliberately: run the trial against the platform you actually care about, at the settings you'll use in production, and check the `sa-credit-cost` header on the first few responses.

For comparison, the free tiers elsewhere in this market:

| Vendor | Free allowance | How it renews |
| --- | --- | --- |
| ScraperAPI | 1,000 credits/month plus a 7-day 5,000-credit trial | Monthly, no card required |
| Bright Data | 5,000 credits/month (its page values this at roughly $7.50) | Resets on the 1st, unused credits do not roll over |
| ScrapingBee | 1,000 credits | One-time, no card required |
| SocialCrawl | 100 credits | One-time |

Bright Data's number is the largest, and its renewal rule is also the least forgiving: monthly reset, no rollover, and once the credits are gone requests error out until the 1st unless you've pre-deposited funds.

If your social workload is rendered feeds at any volume, the honest test is one week against your real targets. 👉 [Start ScraperAPI's 7-day, 5,000-credit trial](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Platform coverage: general-purpose vs dedicated endpoints

ScraperAPI is a general scraping API. In its published credit table, exactly one social domain gets its own rate: LinkedIn, at 30 credits. Everything else goes through the standard request path with whatever rendering and proxy surcharges your target requires. There is no per-platform endpoint catalogue with separate `profiles`, `posts` and `comments` objects.

That matters if you want structured social records without writing parsers. Two competitors have built that layer out.

Bright Data's Social Media Scraper APIs cover 10 platforms through 67 dedicated endpoints, one per data type and input method. The per-platform table adds up to 67; the same document's summary line says 68.

| Platform | Endpoints | What you can collect |
| --- | --- | --- |
| Facebook | 15 | Pages, posts, comments, profiles, events, reels, marketplace listings, company reviews |
| Instagram | 8 | Profiles, posts, reels, comments |
| LinkedIn | 10 | Profiles, companies, posts, jobs, people discovery |
| TikTok | 12 | Profiles, posts, comments, TikTok Shop products |
| Reddit | 4 | Posts by URL, subreddit or keyword, plus comments |
| X (Twitter) | 5 | Posts and profiles by URL, username or profile array |
| YouTube | 10 | Videos, channels and comments by URL, keyword, hashtag or podcast |
| Pinterest | 1 | Profiles and pins |
| Quora | 1 | Posts and answers |
| Vimeo | 1 | Videos and channels |

Each endpoint is either *collect by URL* (you supply canonical URLs, you get structured records) or *discover by keyword, username, category or hashtag* (you supply a query, you get matching URLs and records). The two chain: discover a channel by keyword, then feed those channel URLs into the video endpoint, then feed the video URLs into the comments endpoint.

ScrapingBee splits coverage into three tiers instead. TikTok, TikTok Search and YouTube are dedicated scrapers returning clean JSON. X, Facebook and Pinterest go through its general Web Scraping API with `render_js=true` and `premium_proxy=true`. Instagram, LinkedIn and Reddit it lists as not recommended, on the grounds that scraping them violates their terms of service. Its plans run from $49 for 250,000 credits and 10 concurrent requests up to $599 for 8,000,000 credits and 200 concurrent requests, with a one-time 1,000-credit free tier.

## The terms-of-service question, stated plainly

Vendors in this market take opposite positions, and you should know which one you're buying.

Bright Data sells dedicated Instagram, LinkedIn and Reddit scrapers and describes itself as GDPR and CCPA compliant, with ISO 27001 and SOC 2 controls and a compliance review process. Those are the company's own statements about its own operations. ScrapingBee states that scraping Instagram, LinkedIn and Reddit breaks those platforms' terms of service and does not recommend it. Both positions are commercially motivated and neither settles the legal question for your jurisdiction or your data type.

What's verifiable: public data scraping is generally lawful in many jurisdictions, the law varies by country and by data category, and most platforms restrict scraping contractually even where the underlying activity is legal. That means an account ban or a breach-of-contract claim is possible regardless of who sold you the API. Check the platform's terms and your own GDPR or CCPA obligations before you put anything into production.

## When this isn't the purchase you want

Three cases where you should keep your money:

- **You need two YouTube channels a month.** Manual export or the platform's own API is cheaper than any subscription, including a free one you have to babysit.
- **You need structured profiles, posts and comments out of the box across ten platforms.** ScraperAPI returns HTML unless you parse it or use its auto-parsing, which is free but not the same thing as a per-platform endpoint with a fixed schema. Bright Data's endpoint catalogue or ScrapingBee's three dedicated scrapers are closer to that shape.
- **You need pay-as-you-go overage on a small plan.** Hobby, Startup and Business only offer auto-upgrade. If spiky usage on a low base is your pattern, the tier you pick has to be Scaling or higher.

Conversely, if your work is mostly LinkedIn plus a long tail of smaller sites, the 30-credit LinkedIn rate is the entire pricing question, and the general-purpose model is an advantage: one API key, one billing line, no per-platform contracts.

## Picking a tier without overbuying

Match the plan to the request type, not to the credit count.

- **Standard pages only, low volume:** the free plan's 1,000 credits a month is 1,000 requests. Move to the smallest paid tier only when you need more than 5 concurrent threads or a larger allowance.
- **Rendered social feeds (TikTok, X) at volume:** assume 25 credits per request and size the tier from that. 100,000 rendered requests a month needs 2.5 million credits, not 100,000.
- **LinkedIn-heavy collection:** 30 credits per request before surcharges. Run a handful of URLs through the Domain Cost Estimator first; the difference between a page that needs a bypass and one that doesn't is 10 credits every time.
- **Bursty workloads:** Scaling or above, so pay-as-you-go is available and a spike doesn't silently move you up a plan.

Before you check out, confirm two things on the pricing page: the current per-credit rate for the term you're choosing, since monthly and annual differ, and whether auto-upgrade is on. It's opt-in, and leaving it on is how people end up on a tier they didn't plan to buy.

👉 [Pick your ScraperAPI tier and check the current per-credit rate](https://www.scraperapi.com/pricing/?fp_ref=coupons)
