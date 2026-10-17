# Appie.run MCP server

Find Reddit threads and draft replies, track LinkedIn profiles and leads, write on-brand articles, run AI chatbots and create trackable short links from Cursor, Claude or any other [Model Context Protocol](https://modelcontextprotocol.io) client.

[Appie.run](https://appie.run) is an AI marketing stack that learns your brand from your website. This hosted MCP server gives your AI client the same brand context, so the content and answers it produces sound like you.

## Try it free

New Appie.run accounts get **500 free credits**, no card needed. That's enough for about 5 light articles or 500 chatbot replies. Read tools never spend credits.

## Quick start

1. [Sign up at Appie.run](https://appie.run/users/sign_up) and open your project.
2. Go to **API** in the project sidebar and generate a key (it starts with `esk_`). Only project admins can do this.
3. Add the server to your MCP client config:

```json
{
  "mcpServers": {
    "appie": {
      "type": "streamable-http",
      "url": "https://appie.run/mcp",
      "headers": { "Authorization": "Bearer YOUR_APPIE_API_KEY" }
    }
  }
}
```

The same config is in [`examples/mcp-config.json`](examples/mcp-config.json). Never commit a real key.

## Grok Build plugin

This repo is also a Grok Build plugin. [`.mcp.json`](.mcp.json) connects to `https://appie.run/mcp` and sends your key from the `APPIE_API_KEY` environment variable as a Bearer token. Set it before starting Grok:

```bash
export APPIE_API_KEY=esk_your_key
```

The plugin ships no code, hooks or skills. Its only network endpoint is `https://appie.run/mcp`, and its only credential is `APPIE_API_KEY`.

## What you can do

- **Find Reddit conversations worth joining.** Save keywords and competitors, list matching Reddit threads, and draft a reply for any thread. Appie never posts to Reddit; you paste the reply yourself.
- **Track LinkedIn profiles and leads.** Add the LinkedIn profiles you want to track and list the leads, with email and phone when a lookup has found them.
- **Write articles in your voice.** Start a light (~500 words) or deeply researched (~3,000 words) article using an author profile trained on your writing samples, then check its status until the draft is ready.
- **Manage brand context.** Read and update the project Brain: your website, about text, brand voice, key messages, ideal customer and competitors.
- **Build and query chatbots.** Create a chatbot, train it on text, URLs, sitemaps or your Appie articles, and ask it questions.
- **Create and track short links.** Make links on `s.appie.run` and see clicks and unique visitors for the last 30 days.

### Try asking your AI client

- "Write a light article on onboarding emails for SaaS in our brand voice, and tell me when it's done."
- "Create a support chatbot and train it on our sitemap."
- "Make a short link for our pricing page and show me last month's clicks."
- "Find Reddit threads where people ask for alternatives to our competitors, and draft a reply for the best one."
- "Track this LinkedIn profile and list the latest leads."

## Tools

21 tools across short links, articles, the Brain, chatbots, Reddit and LinkedIn.

| Tool | What it does | Cost |
|---|---|---|
| `create_short_link` | Creates a short link on s.appie.run, with an optional custom code | 1 credit |
| `link_stats` | Clicks, uniques and a 30-day series for one link | Free |
| `create_article` | Starts a light or deeply researched article in an author's voice (async) | 100 or 500 credits |
| `article_status` | Status and content of one article | Free |
| `list_authors` | Lists author profiles and their samples | Free |
| `create_author` | Creates an author, optionally with a first writing sample | See docs |
| `add_author_sample` | Adds a text or blog-URL sample to an author (up to 5) | See docs |
| `get_brain` | Reads the project's brand context | Free |
| `update_brain` | Updates selected brand context fields | See docs |
| `list_chatbots` | Lists chatbots and their training status | Free |
| `create_chatbot` | Creates a chatbot with optional prompt, model and temperature | Free |
| `add_chatbot_content` | Adds text, URL, sitemap or article knowledge to a chatbot | See docs |
| `chatbot_status` | Training status and recent sources for one chatbot | Free |
| `ask_chatbot` | Sends a message to a chatbot and returns its reply | 1 credit |
| `get_reddit_settings` | Reads Reddit keywords, competitors and reply guidance (seeded from the Brain if nothing is saved) | Free |
| `update_reddit_settings` | Sets Reddit keywords, competitors or reply guidance (up to 25 items per list) | Free |
| `list_reddit_mentions` | Lists Reddit threads for saved keywords and competitors, with the latest reply draft | Free to list; a scan runs at most once a day for 10 credits |
| `draft_reddit_reply` | Drafts a reply for one Reddit thread; you post it yourself | 2 credits |
| `get_linkedin_profiles` | Lists the LinkedIn profiles the project tracks | Free |
| `set_linkedin_profile` | Adds or updates a tracked LinkedIn profile | Free to save; scheduled scans cost 30 credits |
| `list_linkedin_leads` | Lists leads, with email and phone once a lookup has found them | Free to list; lookups cost 5 credits per lead |

Every tool result includes `credits_remaining`. Writes spend the project's credits first, then the project owner's.

## Permissions and errors

A key can only call tools for apps the person who generated it is allowed to use. Project owners can use every app. Regenerating a key revokes the old one immediately.

| Error | Meaning |
|---|---|
| HTTP 401 | Missing or revoked key |
| `app_not_allowed` | The key's owner can't use that app |
| `insufficient_credits` | Not enough credits (includes `required` and `available`) |
| `author_profile_required` / `writing_sample_required` | Add an author with a writing sample before `create_article` |
| `not_found` | The link or article isn't in this project |

## Agent Skills

This repo also ships five agent skills in `skills/`:

- `appie-reddit-marketing`: find Reddit threads for your keywords and competitors and draft replies you post yourself
- `appie-linkedin-growth`: track LinkedIn profiles and list leads
- `appie-article-writer`: light or deeply researched articles in your voice
- `appie-chatbot`: build, train and chat with project chatbots
- `appie-url-shortener`: short links and click stats

Install them with:

```bash
npx skills add appierun/appie-mcp
```

Or in Claude Code:

```
/plugin marketplace add appierun/appie-mcp
/plugin install appie-skills@appie
```

## Documentation

Full reference: [appie.run/docs/mcp](https://appie.run/docs/mcp)

## License

MIT, see [LICENSE](LICENSE).
