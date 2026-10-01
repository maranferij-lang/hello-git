# Discoverability of a Streamlit Community Cloud app (CVmax) in Google and AI answer engines: SEO, GEO and a low-cost plan (as of Oct 2026)

Method note: research done 2026-10-01. The network sandbox blocked direct fetches of docs.streamlit.io, discuss.streamlit.io, developers.google.com, arxiv.org, vercel.com, searchenginejournal.com and all *.streamlit.app hosts. So: (a) the Streamlit docs were read from their GitHub source (raw.githubusercontent.com/streamlit/docs), which is the canonical source of docs.streamlit.io; (b) forum, Google and paper content comes from search-result snippets, not full-page reads, and is marked as such where it matters; (c) I could NOT probe what HTML a *.streamlit.app URL actually returns to Googlebot/GPTBot. Treat claims about the exact bot-facing HTML as unverified.

## 1. Does Streamlit Community Cloud serve crawlable HTML/meta to search engines? Official settings? Sleep effects?

### Takeaway
Officially yes for Google/Bing: Streamlit says all *public* Community Cloud apps are indexed "on a weekly basis", with the meta title taken from `st.set_page_config(page_title=...)` and a description that search engines tend to pull from `st.header`/`st.text` at the top of the app. There is no official setting for a meta description, robots, sitemap, OG tags or structured data; the only levers are: public visibility, custom subdomain (pick early), page_title, and the top-of-page text. For non-Google AI crawlers that do not run JavaScript, what they receive from a WebSocket-rendered Streamlit app is undocumented and probably thin, so the app itself should not be relied on as the source AI assistants cite.

### Cited Findings
- Official doc ("SEO and search indexability"): "When you deploy a public app to Streamlit Community Cloud, it is automatically indexed by search engines like Google and Bing on a weekly basis." Users can find it by searching the subdomain or the app's title. — [Streamlit docs source on GitHub](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/share-your-app/indexability.md) (rendered at [docs.streamlit.io](https://docs.streamlit.io/deploy/streamlit-community-cloud/share-your-app/indexability))
- Official tips are exactly four: make the app public; choose a custom subdomain early; choose a descriptive app title; "customize" the meta description. "All public apps hosted on Community Cloud are indexed by search engines. If your app is private, it will not be indexed." — [Streamlit docs source](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/share-your-app/indexability.md)
- Subdomain timing: if you change the subdomain later "your app may be indexed multiple times... your old URL will result in a 404 error", so choose the custom subdomain at deploy time. — [Streamlit docs source](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/share-your-app/indexability.md)
- Title: "By default, the meta title of your app is the same as the title of your app... you can customize the meta title... by setting the st.set_page_config parameter page_title". — [Streamlit docs source](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/share-your-app/indexability.md)
- Description is not directly settable. Streamlit only says: "From our observations, search engines seem to favor the content in both st.header and st.text over st.title. If you put a description at the top of your app under st.header or st.text, there's a good chance search engines will use this for the meta description." (Note the hedged wording: an observation, not a mechanism.) — [Streamlit docs source](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/share-your-app/indexability.md)
- Checking: use `site:<subdomain>.streamlit.app` in Google. — [Streamlit docs source](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/share-your-app/indexability.md)
- Subdomain rules: a custom subdomain must be "between 6 and 63 characters"; names that are taken or contain "restricted words" are rejected. (Confirms why "cvmax" failed.) — [Streamlit docs source: app-settings.md](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/manage-your-app/app-settings.md)
- The indexability feature was announced in late 2022 on the forum ("Indexability for Streamlit apps", thread id 33492); it is an old feature and the doc wording has not materially changed since. — [Streamlit forum announcement](https://discuss.streamlit.io/t/indexability-for-streamlit-apps/33492) (not fetched; date inferred from thread id/era, unverified)
- Root problem for self-hosted Streamlit (forum, via search snippet): Google shows the title "Streamlit" and the description "You need to enable JavaScript to run this app" because Googlebot cannot render apps that talk to the backend over WebSockets; Community Cloud apps are said to get proper titles/descriptions after indexing; the advanced fix is to prerender and serve that to Googlebot. — [Streamlit forum: "You need to enable JavaScript to run this app"](https://discuss.streamlit.io/t/you-need-to-enable-javascript-to-run-this-app/37516); [Streamlit forum: Updating Title/Description in Google Search](https://discuss.streamlit.io/t/updating-title-description-of-app-in-google-search/61447) (snippets only)
- GitHub issue #7648 (opened 1 Nov 2023, now closed) asked for an `st.meta()`-style API for custom metadata; the requester notes only Community Cloud apps get proper metadata while self-hosted apps show generic "Streamlit" metadata. No native meta-description/OG API resulted from it per the visible issue page. — [GitHub issue #7648](https://github.com/streamlit/streamlit/issues/7648)
- Workaround threads for meta descriptions exist (editing Streamlit's static `index.html` in site-packages), which only works where you control the server, i.e. not on Community Cloud. — [Forum: Adding a meta description](https://discuss.streamlit.io/t/adding-a-meta-description-to-your-streamlit-app/17847); [Forum: How to edit index.html](https://discuss.streamlit.io/t/how-to-edit-index-html/55906) (snippets only)
- Sleep: apps with no traffic for 12 hours go to sleep; visitors see a sleeping page and must click "Yes, get this app back up!" (anyone with view access can wake it). — [Streamlit docs: Manage your app](https://docs.streamlit.io/deploy/streamlit-community-cloud/manage-your-app) (via search snippet)
- Third-party (2026) claims: as of April 2025 pushing commits no longer wakes a sleeping app, and the traffic counter follows real browser sessions, not HTTP hits ("A curl against a sleeping app returns 200 while the app stays asleep"). — [Fastero blog (2026)](https://fastero.com/blog/why-your-streamlit-app-keeps-sleeping) (snippet only; third-party, unverified against official docs)
- Embedding: public apps can be iframed with `?embed=true` and auto-embedded via oEmbed in Notion/Medium/Ghost; extra `embed_options` (hide toolbar, theme, etc.). — [Streamlit docs source: embed-your-app.md](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/share-your-app/embed-your-app.md)

### Inferences
- For CVmax the app URL can rank for its own brand/title query in Google (navigational search like "CVmax KSE"), but it will not rank for generic intent queries ("покращити резюме", "AI CV checker") because the crawlable content is minimal and unstructured. Treat the app as the destination, not the content.
- Bot traffic presumably does not count as a "visit" (if the Fastero claim is right), so crawlers hitting a sleeping app may get the sleep page rather than app text. That would mean a rarely-used app can be indexed with sleep-page text. This is plausible but unverified; check with Google Search Console's URL Inspection or `site:` after deployment.
- The private GitHub repo does not block indexing; only app visibility (public vs private) matters per the docs.
- Practical page-level actions: `page_title` like "CVmax - free AI CV improver for KSE students"; first visible element an `st.header`/`st.text` one-sentence description in Ukrainian + English with the key terms (резюме, CV, KSE, безкоштовно).

### Gaps
- Could not inspect the actual HTML that *.streamlit.app serves to Googlebot, GPTBot, OAI-SearchBot, PerplexityBot (egress blocked). Unknown whether Streamlit prerenders for bots, injects a `<title>`/`<meta name="description">` server-side, or relies on Googlebot's JS rendering.
- Could not read *.streamlit.app robots.txt, so whether AI crawlers are allowed or blocked on the shared domain is unknown.
- No official statement found on how sleeping affects crawling, and no 2025-2026 changes to Community Cloud SEO settings found (no sitemap, no robots control, no description field, no "make discoverable" toggle beyond public/private).

## 2. Custom domain for a Community Cloud app, and free/cheap alternatives

### Takeaway
Community Cloud does not support pointing your own domain at an app; you only get a custom *.streamlit.app subdomain (6-63 chars). The standard workaround is a separate static landing page (with real HTML content, meta tags, schema, FAQ) that links to, or iframes, the app. Because CVmax's repo is private, use Cloudflare Pages (free, works with private repos) or a separate public repo on GitHub Pages; a domain is optional.

### Cited Findings
- Community Cloud only lets you set a custom subdomain (e.g. myapp.streamlit.app), not your own top-level domain; common workaround is a static site (GitHub Pages, Netlify, own hosting) embedding the app via iframe with `embed=true`; navigation features may not work perfectly and the browser URL won't update. — [Streamlit forum: custom domain without .streamlit.app](https://discuss.streamlit.io/t/custom-domain-without-streamlit-app/120343); [Streamlit forum: Streamlit Cloud + Custom Domain?](https://discuss.streamlit.io/t/streamlit-cloud-custom-domain/28077) (search-snippet summaries)
- Official subdomain rules (6-63 chars, restricted words). — [Streamlit docs source: app-settings.md](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/manage-your-app/app-settings.md)
- A forum thread titled "Custom Domain menu missing in App Settings (Community Cloud)" exists (2025-2026 era by thread id); content not read, but nothing found indicating official custom-domain support was launched. — [Streamlit forum thread 120316](https://discuss.streamlit.io/t/custom-domain-menu-missing-in-app-settings-community-cloud/120316) (title only; unverified)
- GitHub Pages: on GitHub Free it works only from public repositories; private-repo Pages require Pro/Team/Enterprise. The published site is public either way. — [GitHub Docs / community discussion](https://github.com/orgs/community/discussions/167331); [SmartScope 2026 summary](https://smartscope.blog/en/Tips/GitHub/github-pages-private-repository/)
- Cloudflare Pages: supports private and public GitHub repos, gives a free `<project>.pages.dev` subdomain, free plan without limits on sites/requests/bandwidth (per third-party guides). — [HackerNoon guide](https://hackernoon.com/deploy-your-personal-web-page-with-hugo-cloudflare-and-github-100percent-for-free); [DEV: private portfolio with GitHub + Cloudflare](https://dev.to/sanchitkd/how-i-built-a-fully-private-portfolio-using-github-and-cloudflare-for-free-3e3b) (secondary sources)
- Alternative: move the app to a host that supports custom domains (third-party "deploy beyond Community Cloud" guides); this costs money/effort. — [livemy.app 2026 guide](https://livemy.app/blog/deploy-streamlit-app) (vendor blog)

### Inferences
- Best low-cost architecture: a tiny static landing page (plain HTML or a static-site generator) on Cloudflare Pages or GitHub Pages (separate public repo containing only the landing page, so the app code stays private). It holds: H1 + one-paragraph description, how it works, privacy note (what happens to uploaded CVs), FAQ, KSE context, screenshots, team/contacts, last-updated date, and a big "Open CVmax" button to the *.streamlit.app URL. Linking is preferable to iframing for the primary CTA (iframe breaks URL/navigation; iframed content is not credited to the landing page in search anyway).
- Also add the app link to every place that has crawlable HTML (landing page, GitHub profile README of a public repo, KSE pages) so Google discovers and associates the brand.

### Gaps
- Netlify/Vercel free-tier terms for 2026 were not verified (vercel.com blocked). Cloudflare Pages facts come from secondary guides, not Cloudflare docs.
- Whether Cloudflare has folded Pages into Workers (and any changed free limits) in 2025-2026 was not verified.

## 3. Evidence-based GEO practices in 2025-2026 (what works vs speculative)

### Takeaway
The strongest, best-sourced levers are ordinary: be crawlable as server-rendered HTML (non-Google AI crawlers do not run JavaScript), be indexed and rank in Google/Bing (AI Overviews/AI Mode are built on core Search; ChatGPT search and Perplexity draw on web search indexes), and get mentioned on third-party sites that AI engines cite heavily (Reddit, YouTube, Wikipedia, LinkedIn, editorial media). Content-level GEO tactics (adding statistics, quotations, citations) have lab evidence but weak real-world evidence. llms.txt is effectively unused by AI crawlers and ignored by Google. Schema.org is not required for AI features but harmless and useful for rich results.

### Cited Findings
Google's official position (May 2026 guide + "AI features and your website"):
- No additional requirements or special optimizations to appear in AI Overviews or AI Mode; they are rooted in core Search ranking and quality systems, so SEO best practices apply. — [Google: AI features and your website](https://developers.google.com/search/docs/appearance/ai-features); [Google: AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) (via snippets; page not fetched)
- "You don't need to create new machine readable files, AI text files, markup, or Markdown to appear in generative AI search"; Google Search ignores llms.txt (neither helps nor harms). — [Google AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide); coverage: [Search Engine Journal](https://www.searchenginejournal.com/googles-new-ai-search-guide-calls-aeo-and-geo-still-seo/575026/), [GIGAZINE, 18 May 2026](https://gigazine.net/gsc_news/en/20260518-google-guide-optimizing-generative-ai/) (snippets)
- Structured data is "not required for generative AI search, and there's no special schema.org markup you need to add", though still useful for rich-result eligibility. — [Google AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) (snippet)

JavaScript and AI crawlers:
- Vercel + MERJ analysis (Dec 2024) of 500M+ crawler requests: GPTBot, ClaudeBot and PerplexityBot fetch but do not execute JavaScript (ChatGPT crawler fetches JS files in ~11.5% of requests, Claude in ~23.8%, but runs none); Google's Gemini path benefits from Googlebot's rendering. — [Vercel blog: The rise of the AI crawler](https://vercel.com/blog/the-rise-of-the-ai-crawler) (not fetched; numbers from secondary summaries: [SEOBRO 2026](https://seobro.com/blog/do-ai-crawlers-execute-javascript/), [Search Optimo 2026](https://searchoptimo.com/blog/do-ai-crawlers-render-javascript)). Note: some summaries call "Google-Extended" a crawler that renders JS; Google-Extended is a robots.txt control token, not a separate crawler, so that phrasing is inaccurate.

llms.txt:
- Ahrefs (May 2026, 137,210 domains): ~97% of llms.txt files received zero bot requests; of ~38,000 domains with a valid file only ~1,100 got any traffic to it. — [byFaro summary of Ahrefs data](https://byfaro.ai/blog/ai-crawlers-ignore-llmstxt-97-of-the-time-so-stop-making-one); [Aria Shaw 2026](https://ariashaw.com/does-llms-txt-actually-work) (secondary; Ahrefs original not fetched)
- Log study of 500M+ AI bot visits over 90 days: only 408 targeted /llms.txt; a 48-day server-log study saw zero llms.txt hits from GPTBot, ClaudeBot, PerplexityBot. No major AI company has committed to reading it, and no citation lift was found. — [Limy 2026 guide](https://limy.ai/blog/llms-txt-in-2026-the-full-guide); [Wislr log analysis](https://www.wislr.com/articles/ai-bot-behavior-log-analysis/) (vendor/industry sources)

Third-party mentions and cited domains:
- Ahrefs study of 75,000 brands: branded web mentions correlate 0.664 with AI Overview visibility vs 0.218 for backlinks and 0.392 for branded search volume; a Dec 2025 follow-up across ChatGPT/AI Mode found YouTube mentions correlate ~0.737 (correlational, not causal). — [blckalpaca summary of Ahrefs study](https://blckalpaca.at/en/knowledge-base/seo-geo/geo-generative-engine-optimization/brand-mentions-vs-backlinks-the-ahrefs-75k-brand-study); [CiteFlow](https://www.citeflow.io/blog/brand-mentions-vs-backlinks) (secondary)
- Muck Rack: 82% of 1M+ AI-cited links came from earned media. — cited in [CiteFlow](https://www.citeflow.io/blog/brand-mentions-vs-backlinks) (secondary)
- Peec AI (30M sources): Reddit is the most-cited domain overall, then YouTube, LinkedIn; Wikipedia leads in ChatGPT (13.15% vs Reddit 11.97%); Reddit is ~46.7% of Perplexity's top-source citations. — [Search Engine Land on Peec AI study](https://searchengineland.com/ai-search-engines-cite-reddit-youtube-and-linkedin-most-study-473138); [Peec AI](https://peec.ai/blog/top-domains-cited-by-ai-search-analysis-based-on-30m-sources)

Academic GEO:
- Aggarwal et al., "GEO: Generative Engine Optimization", KDD 2024 (IIT Delhi/Princeton/Georgia Tech/AI2): GEO-bench of 10,000 queries; adding citations, quotations and statistics boosted source visibility by up to ~40% in generative engine responses; keyword stuffing did not help. — [arXiv 2311.09735](https://arxiv.org/abs/2311.09735); [Princeton record](https://collaborate.princeton.edu/en/publications/geo-generative-engine-optimization/) (abstract/snippet level; keyword-stuffing result from my knowledge of the paper, not re-verified here)
- Critical survey (Martinez, July 2026, 45 studies, Nov 2023-Jul 2026): the foundational gains hold only when a source is already in the retrieved context; "no reviewed technique shows a stable, longitudinal, cross-platform causal effect on organic discoverability"; topical relevance and context position are the most reproducible levers; generic heuristics transfer poorly; citation-oriented rewrites can impair retrieval. — [arXiv 2607.14035](https://arxiv.org/abs/2607.14035) (via snippet)

### Inferences
Evidence tiers for CVmax:
- Strong/official: crawlable static HTML page with clear facts; indexed in Google (Search Console) and Bing (Bing Webmaster Tools; ChatGPT search has historically leaned on Bing, not re-verified here); third-party mentions on frequently cited platforms.
- Moderate (lab/correlational): factual, specific content (numbers, e.g. "free, 0 UAH; takes ~1 minute; supports PDF/DOCX; built at KSE in 2026"), clear Q&A FAQ, quotable one-sentence definitions ("CVmax is a free AI tool from KSE students that ...").
- Weak/speculative: llms.txt (no measurable use); special "GEO schema"; AI-tool directory listings as a citation lever (no data found).
- Schema.org `SoftwareApplication`/`WebApplication` + `FAQPage` JSON-LD on the landing page: cheap and harmless, but do not expect AI-citation gains from it (Google says not required).
- A Streamlit-rendered page is the worst case for GPTBot/PerplexityBot (no JS execution) so any content you want AI to quote must live on a static page.

### Gaps
- Could not verify which search index ChatGPT search uses in 2026 (Bing vs own OAI-SearchBot index) or Perplexity's current crawler behaviour beyond the Vercel data.
- No evidence found for whether Product Hunt / "AI tools directories" listings produce AI citations for small tools.
- No studies found on GEO for Ukrainian-language queries specifically.

## 4. Channels for a Ukrainian student audience vs search

### Takeaway
For Ukrainian 18-29s, Telegram is the dominant channel (~90% use), with TikTok rising (~34%) and YouTube significant; for a niche tool aimed at one university, direct distribution (KSE Telegram chats/channels, career centre, student organisations, Instagram) will drive far more users than search in the first months. Search/AI visibility is a slow, compounding secondary channel, mostly for brand ("CVmax") queries and for being recommended when someone asks an AI about CV tools.

### Cited Findings
- Internews Ukraine 2025: 86% of Ukrainians get news via social platforms; Telegram used by 81% of respondents; for following public channels, 62% Telegram, 32% YouTube, 22% Facebook; smartphone news use 96% among 18-35s. — [Internews Ukraine 2025 release](https://internews.ua/en/opportunity/media_trust_consumption_2025_release)
- Among 18-29-year-olds Telegram is the most popular (90.5%) and TikTok grew by more than 9 points over a year to 34.3%. — [Internews Ukraine 2025 release](https://internews.ua/en/opportunity/media_trust_consumption_2025_release) (via snippet; this youth figure may come from another survey cited in results; verify)
- Other 2025 polls: half of Ukrainians rely on Telegram as their primary news source. — [Ukrainska Pravda, 25 Sep 2025](https://www.pravda.com.ua/eng/news/2025/09/25/7532494/); [dev.ua on Ipsos 2025](https://dev.ua/en/news/telegram-became-the-main-source-of-news-for-ukrainians-in-2025-ipsos-study)
- Reddit and YouTube are the most AI-cited domains (see section 3), so a Reddit post/YouTube demo can serve both discovery and AI citation. — [Search Engine Land](https://searchengineland.com/ai-search-engines-cite-reddit-youtube-and-linkedin-most-study-473138)

### Inferences
- Priority for discovery: (1) KSE official/student Telegram channels and chats, career centre newsletter; (2) a short Instagram Reels/TikTok demo ("before/after CV in 60 seconds"); (3) a DOU forum post or article (DOU is the main Ukrainian IT community and is indexed and likely crawlable; good for both Google and AI citations); (4) a KSE news page or student-project mention (university domains carry authority); (5) a YouTube demo (best correlation with AI mentions per Ahrefs); (6) LinkedIn posts by team members; (7) optionally Reddit (r/Ukraine-adjacent or r/resumes) and Product Hunt, mainly for English-language/AI-citation value.
- Telegram channel posts are partly indexed by search engines via t.me/s/ public previews, but their AI-citation value is unclear (no data found).

### Gaps
- No data found on how KSE students specifically discover tools, or on DOU's share of AI citations.
- Did not verify whether t.me public channel pages are cited by ChatGPT/Perplexity.

## 5. Domain/name: is a *.streamlit.app subdomain fine, or is a custom domain worth it?

### Takeaway
A *.streamlit.app subdomain is fine for launch and is indexed; pick the final name now (renaming later creates duplicate indexing and 404s). A custom domain cannot be attached to the app itself, so buying one only makes sense for the landing page; it improves memorability/trust and lets you keep the brand if you ever move hosts, but gives no direct ranking boost. If buying, cheapest sensible options are a .com.ua (no trademark needed) or a gTLD; .ai is expensive and has no SEO edge.

### Cited Findings
- Subdomain must be 6-63 chars; choose early to avoid double indexing and old-URL 404s. — [Streamlit docs source: app-settings.md](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/manage-your-app/app-settings.md); [indexability.md](https://github.com/streamlit/docs/blob/main/content/deploy/community-cloud/share-your-app/indexability.md)
- Google treats some ccTLDs (incl. .ai, .io) as generic with no geographic signal; TLD choice does not directly affect rankings, though some spam-associated new TLDs may face lower user trust. — [Search Engine Land: Google treats .ai as gTLD](https://searchengineland.com/google-now-treats-ai-domains-as-generic-top-level-domains-427770); [Search Engine Land: domain extensions and SEO](https://searchengineland.com/domain-extensions-seo-454524); [Google: multi-regional sites](https://developers.google.com/search/docs/specialty/international/managing-multi-regional-sites)
- .com.ua is open to anyone, no trademark/legal entity required (unlike second-level .ua), run by Hostmaster; foreign-registrar prices seen: ~$30/yr (Regery), EUR 41.50 (EuroDNS). — [Regery](https://regery.com/en/domains/zone/com.ua); [EuroDNS](https://www.eurodns.com/domain-extensions/com.ua-domain-registration); [Wikipedia: .ua](https://en.wikipedia.org/wiki/.ua)

### Inferences
- Name choice for the subdomain: "cvmax-ai" or "getcvmax" are both fine; a brand-first name ("cvmax-kse" or "cvmax-ai") that matches the page_title and landing-page H1 helps entity consistency across Google and AI answers. Avoid changing it after launch.
- Free path that already covers most of the benefit: landing page at cvmax.pages.dev (or <org>.github.io/cvmax) + app at cvmax-ai.streamlit.app. Buy a domain only if (a) the project will live beyond the course, or (b) you need printed/QR material and want a short memorable URL; then point it at the landing page, not the app.
- ccTLD .com.ua gives a Ukraine geo signal, which suits a KSE-only audience, but limits English/international reach slightly; a gTLD is neutral.

### Gaps
- Ukrainian registrar prices for .com.ua (likely cheaper than foreign resellers) not verified; .app pricing not checked (.app is HSTS-preloaded, requiring HTTPS, which Cloudflare/GitHub Pages provide).
- No evidence found quantifying user trust differences between *.streamlit.app and a custom domain.

### Prioritised low-cost action plan (synthesised from the findings above; ordering is my judgement)
1. Now, free: finalise the subdomain (e.g. cvmax-ai.streamlit.app) and keep the app public; set `page_title` to a descriptive brand title; put a one-sentence UA/EN description in `st.header`/`st.text` at the very top. (Sections 1, 5)
2. Free: build a static landing page (Cloudflare Pages from the private repo, or a separate public GitHub Pages repo) with real HTML: what CVmax is, who it is for (KSE students), how it works, privacy/data handling, FAQ in Q&A form, specific facts, team, date; `<title>`, meta description, OG tags, `SoftwareApplication` + `FAQPage` JSON-LD; prominent link to the app. (Sections 2, 3)
3. Free: register the landing page in Google Search Console and Bing Webmaster Tools; submit sitemap; check `site:` for both URLs. (Sections 1, 3)
4. Free, highest-leverage for users: launch posts in KSE Telegram channels/chats, career centre, student orgs; short Reels/TikTok demo. (Section 4)
5. Free, for search/AI citation: get mentions on crawlable third-party pages: KSE news/student-projects page, DOU post, a YouTube demo, LinkedIn posts, optionally Reddit and Product Hunt. (Sections 3, 4)
6. Skip: llms.txt as a visibility lever (no evidence of use), "GEO schema" myths, keyword stuffing. (Section 3)
7. Optional, ~$10-30/yr: buy a domain for the landing page only if the project is meant to persist. (Section 5)
