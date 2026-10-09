---
name: claude-startup-email
description: Set up or repair a company email address on an existing domain using Cloudflare Email Routing, Resend SMTP, and Gmail with free email service plans. Use when a founder needs a company address, including before applying to Claude Startups.
---

# Claude Startup Email

Set up a working company email address, such as `hello@example.com`, with incoming mail forwarded to the user's existing Gmail and outgoing mail sent through Resend from Gmail.

This creates an email address and a Gmail sending identity, not a separate Google account, Google Workspace subscription, or hosted mailbox. Keep this skill focused on email. Website deployment and startup program applications are separate tasks unless the user requests them.

## Start with the existing setup

Get the domain, desired company address, destination Gmail, and sender display name from the conversation or existing configuration. Ask only for missing information. Confirm the user controls the domain and identify the correct Cloudflare account, Resend team, and Gmail account before changing settings.

Inspect existing DNS records, mail routing, verified destinations, Resend domains, and Gmail sending identities. Reuse working configuration. If an active mail provider already owns the domain's MX records, explain the conflict and resolve the intended migration or use of a subdomain before replacing them. Do not disrupt existing mail to make this setup work.

**Author's recommendation: I recommend using Ego Browser (`ego-browser`) with this skill.** It lets the agent work in your existing signed-in browser sessions while you follow the setup.

Prefer `ego-browser` when installed and read its skill instructions before browser operations. Reuse one TaskSpace for the setup and respect handoffs for browser permission prompts. If Ego Browser is unavailable, use the user's available authenticated browser integration or scoped provider APIs. If no browser or API access exists, provide the exact next manual steps and report setup as incomplete. A prompt alone does not grant browser access.

Respect the user's existing account creation, sign-in, verification, and sending preferences. Continue authorized configuration without repeated approval questions. Hand off steps that require the user's credentials, MFA, CAPTCHA, or browser permission interaction. Do not assume that creating an email alias authorizes creating other service accounts or submitting applications.

## Explain the cost accurately

Check the current official plans before describing the setup as free:

- [Cloudflare Email Service pricing](https://developers.cloudflare.com/email-service/platform/pricing/): use Email Routing for incoming mail. Do not enable paid outbound sending or Workers merely to implement this workflow.
- [Resend pricing](https://resend.com/pricing): use the free sending plan and confirm the account's actual daily, monthly, and domain limits. Do not enable upgrades or paid overages.
- Existing Gmail: use the user's current account without adding a subscription.
- Domain registration and renewal remain separate costs. If the user does not own a domain, explain this prerequisite; do not promise a free domain or buy one without authorization.

Describe the result as **no additional monthly email-service subscription within the free-plan limits**, not an entirely free company account. Paid AI tools or browser tooling, if applicable, are separate from the email-service cost.

## 1. Receive mail with Cloudflare

1. Record the relevant existing DNS values so this task's changes can be reversed without affecting other services.
2. Open Email Routing for the correct zone. Inspect the DNS changes proposed by Cloudflare before applying them. Use the current provider-generated MX and TXT values rather than copied examples.
3. Reuse or verify the destination Gmail address according to the user's verification preferences.
4. Create an exact routing rule from the requested company address to that Gmail. A catch-all or Worker is unnecessary for a single address.
5. Confirm the destination is verified and the rule is active. Distinguish a configured rule from observed email delivery.

If Cloudflare is not the authoritative DNS provider, identify the required DNS onboarding and its impact before changing nameservers. Do not silently move DNS hosting as part of an email alias setup.

## 2. Verify sending with Resend

1. Check the current team and free domain allowance. Reuse the intended domain if it already exists.
2. Add the sender domain and copy the DNS records generated for that specific domain. Preserve Cloudflare's inbound MX records. Do not put a sending or bounce subdomain's MX record at the root.
3. Use DNS-only records where applicable. At each hostname, maintain a single SPF record; reconcile required mechanisms instead of publishing conflicting SPF records. Preserve existing DKIM selectors and DMARC policy.
4. If no DMARC policy exists, a monitoring policy such as `v=DMARC1; p=none;` can be a starting point. Explain that it monitors rather than rejects spoofed mail. Do not invent reporting destinations or strengthen an existing policy without checking alignment.
5. Wait for the domain to become verified. Check authoritative and public DNS if verification stalls; do not repeatedly delete and recreate correct records.
6. Create a descriptively named API key with **Sending access**, restricted to the target domain. If the domain is missing from the key selector, resolve verification or team selection rather than choosing all domains.

Treat the key as an SMTP password. Transfer it only through a secret-safe mechanism to the intended credential field. Never print it, include it in screenshots or snapshots, put it in shell arguments, or save it in ordinary files. If the available tool cannot keep it out of logs and output, have the user paste it directly.

Official reference: [Resend domain management](https://resend.com/docs/dashboard/domains/manage-domains).

## 3. Send from Gmail

Check [Resend's SMTP documentation](https://resend.com/docs/send-with-smtp) and [Gmail's send-as instructions](https://support.google.com/mail/answer/22370) for current settings.

In Gmail, open **Settings → See all settings → Accounts and Import → Send mail as → Add another email address**. Enter the confirmed display name and company address. Treating the address as an alias is generally appropriate for the user's own company identity; follow the actual intended ownership. Preserve other aliases and the default sender unless the user requests a change.

Use the provider-confirmed SMTP settings. The documented configuration to check is:

| Setting | Value |
| --- | --- |
| SMTP host | `smtp.resend.com` |
| Username | `resend` |
| Password | Domain-restricted Resend sending API key |
| Connection | Port `465` with SSL, or `587` with STARTTLS |

Complete Gmail's address confirmation through the incoming route, respecting the user's verification preferences. Track these as separate milestones: SMTP credentials accepted, confirmation email received, and sending identity verified. One milestone does not prove the others.

## 4. Verify and report

Use actual messages to verify delivery only when test sending is authorized. Send tests solely between addresses controlled by the user. If authorization is absent, finish the configuration and provide a ready-to-run test for the user, marking delivery as untested.

- Incoming: a message addressed to the company address arrives in the destination Gmail. An already received service verification message can establish this.
- Outgoing: a message sent through Gmail with the company address selected in From arrives at another user-controlled inbox.
- Reply: a reply reaches the company address and is forwarded back to Gmail.
- Authentication: inspect received headers for SPF, DKIM, and DMARC results and alignment when available. Do not equate SMTP acceptance or provider verification with inbox delivery.

Report the company address, destination Gmail, routing status, domain verification, Gmail sending identity, observed delivery results, checked free-plan limits, and unresolved steps. State what evidence supports each completed stage. Keep secrets, login links, and verification codes out of the report and any saved handoff notes.

If a step fails, inspect the relevant layer and make an evidence-based correction. Stop repeating the same failed action when no new evidence supports a retry. Leave a precise next step rather than rebuilding the whole setup. For key rotation, replace only the task-specific key and confirm the replacement before revoking a working old key; handle a known compromised key promptly.

## Claude Startups handoff

If the user's purpose is a startup application, explain that a working company email is only one part of preparation. Check the [official Claude Startups page](https://claude.com/programs/startups) for current eligibility and benefits if discussing the program. Never promise acceptance, a free Claude subscription, or credits based on email setup. Do not hard-code promotional benefits into the workflow.

Do not create a Claude account or submit an application merely because the email is ready. Continue those tasks only within the user's explicit request and existing submission preferences.

## Example invocation

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
