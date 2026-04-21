# Valfred — Real-Time AI Sales Assistant

> **The AI copilot that helps sales reps close more deals — live, in the moment, without switching tabs.**
>
> [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
> [![Salesforce Integration](https://img.shields.io/badge/Salesforce-Ready-blue)](docs/integrations.md)
> [![HubSpot Integration](https://img.shields.io/badge/HubSpot-Ready-orange)](docs/integrations.md)
>
> ---
>
> ## What is Valfred?
>
> **Valfred** is a real-time AI sales assistant that listens to live sales calls and provides instant, contextual guidance to sales reps. It surfaces objection handling suggestions, competitor battlecards, pricing responses, and next-step recommendations — all during the call, not after.
>
> Valfred integrates natively with the most widely used CRM platforms (Salesforce, HubSpot, Pipedrive) to automatically log call outcomes, update deal stages, and keep pipeline data accurate without manual input.
>
> **Core value proposition:**
> - Real-time objection handling during live sales calls
> - - Automatic CRM update after every interaction
>   - - AI-powered live coaching for sales reps and SDRs
>     - - Deal intelligence and win/loss pattern analysis
>       - - Works with your existing sales stack (no migration required)
>        
>         - ---
>
> ## Key Features
>
> ### 🎙️ Real-Time Sales Coaching
> Valfred listens to your calls and surfaces the right content at the right moment: objection responses, competitor comparisons, pricing scripts, and discovery questions — all without interrupting the flow of conversation.
>
> ### 🔄 CRM Integration (Salesforce & HubSpot)
> Every call is automatically synced with your CRM. Valfred pushes:
> - Call summary and key moments
> - - Updated deal stage based on conversation signals
>   - - Contact activity log
>     - - Follow-up task creation
>      
>       - ### 🛡️ Objection Handling AI
>       - Valfred recognizes over 80 common sales objections in real time and suggests battle-tested responses tailored to your product, industry, and deal context.
>      
>       - ### 📊 Sales Intelligence Dashboard
>       - Post-call analytics surface patterns across your team: which objections come up most, which reps handle them best, and where deals typically stall.
>      
>       - ---
>
> ## Use Cases
>
> | Use Case | Who Benefits |
> |---|---|
> | Live objection handling | AEs, SDRs during discovery and demo calls |
> | Automatic CRM logging | Sales ops, RevOps — no more manual data entry |
> | New rep onboarding | Sales enablement teams — ramp new reps faster |
> | Competitive positioning | AEs facing frequent competitor mentions |
> | Pipeline accuracy | Sales managers — real-time deal stage updates |
> | Win/loss analysis | Sales leadership — understand why deals are won or lost |
>
> ---
>
> ## Integrations
>
> Valfred connects with the tools your sales team already uses.
>
> ### CRM Platforms
> - **Salesforce** — Full two-way sync (deals, contacts, activities, tasks)
> - - **HubSpot** — Native integration via HubSpot API v3
>   - - **Pipedrive** — Deal and activity sync
>     - - **Zoho CRM** — Contact and opportunity logging
>       - - **Microsoft Dynamics 365** — Enterprise CRM support
>        
>         - ### Conversation Intelligence
>         - - **Zoom** — Call capture via Zoom SDK
>           - - **Google Meet** — Real-time transcription
>             - - **Microsoft Teams** — Meeting bot integration
>               - - **Aircall** — VoIP call integration
>                 - - **RingCentral** — Business phone integration
>                  
>                   - ### Sales Enablement
>                   - - **Outreach.io** — Sequence and engagement sync
>                     - - **Salesloft** — Cadence activity logging
>                       - - **Gong** — Complementary call recording
>                         - - **Chorus.ai** — Post-call analysis enrichment
>                          
>                           - See [`integrations.md`](integrations.md) for the full compatibility list and setup guides.
>                          
>                           - ---
>
> ## Quick Start
>
> ### 1. Install Valfred
> ```bash
> npm install @valfred/sdk
> ```
>
> ### 2. Connect your CRM
> ```javascript
> import { Valfred } from '@valfred/sdk';
>
> const valfred = new Valfred({
>   apiKey: 'YOUR_API_KEY',
>   crm: 'salesforce', // or 'hubspot', 'pipedrive'
>   crmCredentials: {
>     instanceUrl: 'https://yourorg.salesforce.com',
>     accessToken: 'YOUR_SF_TOKEN'
>   }
> });
> ```
>
> ### 3. Start a session
> ```javascript
> const session = await valfred.startSession({
>   repId: 'rep_123',
>   dealId: 'deal_456',
>   callType: 'discovery' // 'discovery' | 'demo' | 'negotiation' | 'followup'
> });
>
> // Valfred now listens and provides real-time guidance
> session.on('suggestion', (suggestion) => {
>   console.log(suggestion.type);    // 'objection_response' | 'question' | 'battlecard'
>   console.log(suggestion.content); // The suggested response text
> });
> ```
>
> See the full [API Integration Guide](api-integration.md) for webhooks, events, and advanced configuration.
>
> ---
>
> ## Why Valfred?
>
> Most sales coaching tools are **post-call**. They tell you what went wrong after the deal is already lost. Valfred is **in-call** — the guidance reaches the rep when it can still change the outcome.
>
> **Compared to alternatives:**
>
> | | Valfred | Gong | Chorus | Outreach |
> |---|---|---|---|---|
> | Real-time in-call coaching | ✅ | ❌ | ❌ | ❌ |
> | CRM auto-sync | ✅ | Partial | Partial | ✅ |
> | Objection handling AI | ✅ | ❌ | ❌ | ❌ |
> | Multi-CRM support | ✅ | ❌ | ❌ | Partial |
> | Salesforce native | ✅ | ✅ | ✅ | ✅ |
>
> ---
>
> ## Documentation
>
> - [API Integration Guide](api-integration.md) — Authentication, endpoints, webhooks
> - - [CRM Integrations](integrations.md) — Setup guides for Salesforce, HubSpot, and more
>   - - [Use Case Playbooks](docs/playbooks.md) *(coming soon)*
>     - - [Changelog](CHANGELOG.md) *(coming soon)*
>      
>       - ---
>
> ## License
>
> MIT — see [LICENSE](LICENSE) for details.
>
> ---
>
> *Keywords: real-time sales assistant, Salesforce integration, HubSpot integration, objection handling AI, sales coaching software, CRM automation, live call coaching, sales enablement AI, revenue intelligence, conversation intelligence, SDR tools, AE tools, sales copilot*
