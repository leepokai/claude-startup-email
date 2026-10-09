# Claude Startup Email

An agent skill for setting up a company email address with **Cloudflare Email Routing + Resend + Gmail**, using free email-service plans.

Receive mail at `hello@yourdomain.com` in your existing Gmail inbox, and send from that company address through Gmail using Resend SMTP.

> **I recommend using [Ego Browser](https://lite.ego.app/) with this skill.** It lets the agent work in your existing signed-in browser sessions while you follow the setup.

## What you need

- A domain you own and access to its DNS settings.
- An existing Gmail account.
- Cloudflare and Resend accounts, or the ability to complete their signup steps.
- Claude Code or another skill-compatible agent, with browser automation or scoped provider API access.

The skill contains instructions for your agent. Installing it does not install Ego Browser, grant account access, or configure email by itself. Connect your browser tools separately and complete any required sign-ins or verification.

## Install in Claude Code

Clone this repository into your personal skills directory:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/leepokai/claude-startup-email.git ~/.claude/skills/claude-startup-email
```

Then start a new Claude Code session and use the prompt below.

If that directory already exists, inspect your existing installation first. Do not overwrite it blindly.

For Codex, use `~/.codex/skills/claude-startup-email` instead, or the `skills` directory under your custom `CODEX_HOME`.

## Copy this prompt

```text
Use the claude-startup-email skill to set up my company email.

My domain: [yourdomain.com]
Company email I want: [hello@yourdomain.com]
My existing Gmail: [your Gmail address]
Sender display name: [your name or company name]

Use Cloudflare Email Routing, Resend's free plan, and Gmail.
Use Ego Browser for the browser setup if available.
Inspect and reuse my existing settings. Keep email services within
their free plans and preserve unrelated DNS and email settings.
Help me complete any necessary sign-ins or verification.
You may send verification and delivery tests between my own addresses.
Report what works and what still needs attention.
```

## How it works

| Direction | Route |
| --- | --- |
| Incoming | Sender → your company address → Cloudflare Email Routing → Gmail |
| Outgoing | Gmail with your company From address → Resend SMTP → recipient |

The agent inspects existing settings, configures forwarding, verifies the sending domain, adds the Gmail sending identity, and checks delivery when authorized. It preserves unrelated DNS records and uses a domain-restricted sending key.

Read [SKILL.md](SKILL.md) for the complete workflow and troubleshooting guidance.

## What “free” means

The goal is **no additional monthly email-service subscription within the providers' free-plan limits**. Domain registration and renewal are separate costs. AI tools and browser tooling may also have their own costs.

This setup uses your existing Gmail inbox. It does not create a separate Google account, a Google Workspace subscription, or an independent hosted mailbox.

Check current [Cloudflare pricing](https://developers.cloudflare.com/email-service/platform/pricing/) and [Resend pricing](https://resend.com/pricing/) before setting up. The skill instructs the agent to check account-specific limits and avoid paid upgrades.

## Using the address for Claude Startups

A working company email can help you prepare an application. It does not guarantee eligibility, acceptance, credits, or a free Claude subscription. Check the [official program page](https://claude.com/programs/startups) for current requirements and benefits.

This project focuses on email setup. Creating a Claude account or submitting an application requires a separate request.

## Contributing

Issues and pull requests are welcome, especially for provider UI changes or reproducible setup failures. Remove email addresses, API keys, verification links, and account details from reports and screenshots.

## License

[MIT](LICENSE) © 2026 leepokai.

An independent community skill, not an official Anthropic, Cloudflare, Resend, Google, or Ego Browser product.
