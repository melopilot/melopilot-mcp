<p align="center">
  <img src="assets/piano-pilot-icon-512.png" alt="Piano Pilot" width="96" height="96">
</p>

<h1 align="center">Piano Pilot MCP server</h1>

<p align="center">
  Connect your <a href="https://melopilot.com">Piano Pilot</a> account to Claude, ChatGPT and other AI assistants.
</p>

---

Piano Pilot is Melopilot's piano practice app for Android, iOS and the web. Its MCP server lets an AI assistant you choose work with your account. It can find songs at your level, explain today's practice plan, look at your progress and scores, and make practice pieces that open in the [melopilot.com](https://melopilot.com/piano) web player.

The server is hosted by Melopilot, so there's nothing to install or run.

| | |
|---|---|
| **Server URL** | `https://api.melopilot.com/mcp` |
| **Transport** | Streamable HTTP |
| **Sign-in** | OAuth with your Melopilot account (dynamic client registration) |
| **Cost** | Free. Your own credits are spent only if you ask it to buy a song |

## Connect

You need a free Melopilot account. If you don't have one, the sign-in page lets you create one.

### Claude

1. Open **Settings → Connectors** and choose **Add custom connector**.
2. Name it `Piano Pilot` and paste `https://api.melopilot.com/mcp`.
3. Choose **Connect** and sign in with your Melopilot account.

On Claude Team and Enterprise plans, an organization owner adds the connector first.

### ChatGPT

1. Open **Settings → Apps & Connectors → Advanced settings** and turn on **Developer mode**.
2. Create a connector with the URL `https://api.melopilot.com/mcp` and OAuth authentication.
3. Sign in with your Melopilot account when asked.

### Other MCP clients

Any client that supports remote MCP servers with OAuth can connect to `https://api.melopilot.com/mcp`. For example, in Claude Code:

```sh
claude mcp add --transport http piano-pilot https://api.melopilot.com/mcp
```

The server publishes standard OAuth discovery metadata (`/.well-known/oauth-protected-resource`), so clients find the sign-in page on their own.

## Tools

| Tool | What it does | Changes anything? |
|---|---|---|
| `get_account` | Your profile, skill level, daily goal, streak, credit balance and family profiles | No |
| `search_songs` | Searches the song library by words, difficulty, length, focus and category | No |
| `list_categories` | The library's categories | No |
| `get_song` | A song's key, tempo, measures, range and price, plus links to play it, hear a preview and open its easier versions | No |
| `list_favorites` | Your favorite songs | No |
| `set_favorite` | Adds a song to your favorites or removes it | Yes, your favorites |
| `get_practice_plan` | Today's plan: a warm-up, a song to review and a new song, sized to your daily goal | No |
| `get_progress` | Practice time, accuracy trends, consistency and streak over 7 days to all time | No |
| `get_recent_practice` | Your latest sessions and scores, and the songs you played most recently | No |
| `get_score_history` | Your saved scores, for one song or all | No |
| `get_credits` | Your credit balance, what credits buy and recent transactions | No |
| `make_practice_piece` | Cuts chosen bars of a song into a short drill (looped, hands separate or rhythm) and gives you a link to play it | No (free) |
| `buy_song` | Buys a paid song with your credits | **Yes, spends credits** |

Most tools take an optional `profile_id`, so a parent can act for a child's family profile.

### Spending credits

`buy_song` is the only tool that spends anything. The assistant has to tell you the price and get your yes first. It then sends the agreed price as `confirm_price_credits`, and the server refuses the purchase if that doesn't match the song's current price, so a price change is never charged without you seeing it. Practice pieces, easier song versions and the daily plan are free.

## Example prompts

- "What should I practice today on Piano Pilot?"
- "Find me three beginner classical songs under three minutes."
- "How has my accuracy changed this month?"
- "Bars 9 to 12 of Für Elise keep tripping me up. Make me a left-hand drill at 75% speed."
- "Open the melody-only version of Ode to Joy."
- "Add Minuet in G to my favorites."

## Privacy and security

- You sign in on Melopilot's own sign-in page, so the assistant never sees your password. Its access token only works for the tools above.
- The server receives only what each tool needs, such as search words, a song or bars. It never asks for or stores your conversation, chat history or the assistant's memory.
- Tool calls are recorded in Melopilot's activity log for security and support, under the retention periods in the privacy policy.
- To disconnect, remove the connector in your assistant's settings. Deleting your Melopilot account ends every connection.

Read the full [privacy policy](https://melopilot.com/privacy#ai-assistant-connections) and [terms](https://melopilot.com/terms).

## Support

- Help center: https://melopilot.com/help
- Email: support@melopilot.com
- Privacy questions: privacy@melopilot.com
- Security issues: please email support@melopilot.com with "Security" in the subject

This repository holds the server's public documentation and registry metadata (`server.json`). The server itself runs as part of Melopilot's hosted service.

## License

The documentation in this repository is available under the [MIT License](LICENSE). Piano Pilot and Melopilot are trademarks of Melopilot.
