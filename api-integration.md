# Valfred API Integration Guide

This guide covers authentication, core endpoints, webhook setup, and SDK usage for integrating Valfred into your sales stack.

---

## Base URL

```
https://api.valfred.io/v1
```

All requests must be made over HTTPS. HTTP requests will be rejected.

---

## Authentication

Valfred uses API keys for authentication. Include your API key in the `Authorization` header of every request.

```
Authorization: Bearer YOUR_API_KEY
```

### Getting your API key

1. Log into your Valfred dashboard at [app.valfred.io](https://app.valfred.io)
2. Navigate to **Settings → API Keys**
3. Click **Generate New Key**
4. Copy and store your key securely — it will only be shown once

### Key scopes

| Scope | Description |
|---|---|
| `sessions:write` | Start and end call sessions |
| `sessions:read` | Read session data and transcripts |
| `crm:write` | Push data to connected CRM |
| `crm:read` | Read CRM data into Valfred |
| `webhooks:write` | Register and manage webhooks |
| `analytics:read` | Access call analytics and reporting |

---

## Core Endpoints

### Sessions

#### Start a session

```http
POST /v1/sessions
```

Starts a new call session. Valfred begins listening for real-time coaching signals.

**Request body:**

```json
{
  "rep_id": "rep_abc123",
    "deal_id": "deal_xyz789",
      "call_type": "discovery",
        "crm": "salesforce",
          "metadata": {
              "prospect_name": "Acme Corp",
                  "prospect_industry": "SaaS",
                      "deal_value": 45000
                        }
                        }
                        ```

                        | Field | Type | Required | Description |
                        |---|---|---|---|
                        | `rep_id` | string | ✅ | Unique identifier for the sales rep |
                        | `deal_id` | string | ✅ | CRM deal/opportunity ID |
                        | `call_type` | string | ✅ | `discovery`, `demo`, `negotiation`, `followup` |
                        | `crm` | string | ❌ | CRM to sync with: `salesforce`, `hubspot`, `pipedrive` |
                        | `metadata` | object | ❌ | Additional context for better AI suggestions |

                        **Response:**

                        ```json
                        {
                          "session_id": "sess_1a2b3c4d",
                            "status": "active",
                              "started_at": "2025-04-21T14:30:00Z",
                                "websocket_url": "wss://rt.valfred.io/sessions/sess_1a2b3c4d"
                                }
                                ```

                                ---

                                #### End a session

                                ```http
                                POST /v1/sessions/{session_id}/end
                                ```

                                Ends the session, triggers CRM sync, and generates the call summary.

                                **Response:**

                                ```json
                                {
                                  "session_id": "sess_1a2b3c4d",
                                    "status": "completed",
                                      "duration_seconds": 2847,
                                        "ended_at": "2025-04-21T15:17:27Z",
                                          "summary": {
                                              "objections_handled": 4,
                                                  "key_topics": ["pricing", "integration complexity", "security"],
                                                      "next_steps": "Send security compliance doc and schedule technical call",
                                                          "crm_synced": true,
                                                              "crm_deal_stage_updated": "Proposal/Price Quote"
                                                                }
                                                                }
                                                                ```

                                                                ---

                                                                #### Get session details

                                                                ```http
                                                                GET /v1/sessions/{session_id}
                                                                ```

                                                                Retrieve full details of a completed session, including transcript, suggestions, and CRM sync status.

                                                                ---

                                                                #### List sessions

                                                                ```http
                                                                GET /v1/sessions
                                                                ```

                                                                **Query parameters:**

                                                                | Parameter | Type | Description |
                                                                |---|---|---|
                                                                | `rep_id` | string | Filter by sales rep |
                                                                | `from` | ISO 8601 | Start date filter |
                                                                | `to` | ISO 8601 | End date filter |
                                                                | `call_type` | string | Filter by call type |
                                                                | `limit` | integer | Max results (default: 50, max: 200) |
                                                                | `offset` | integer | Pagination offset |

                                                                ---

                                                                ### Real-Time Suggestions (WebSocket)

                                                                During an active session, connect to the WebSocket URL returned in the session creation response to receive live coaching suggestions.

                                                                ```javascript
                                                                const ws = new WebSocket('wss://rt.valfred.io/sessions/sess_1a2b3c4d', {
                                                                  headers: { Authorization: 'Bearer YOUR_API_KEY' }
                                                                  });

                                                                  ws.on('message', (data) => {
                                                                    const event = JSON.parse(data);

                                                                      switch (event.type) {
                                                                          case 'objection_detected':
                                                                                // An objection was detected in the conversation
                                                                                      console.log(event.objection_type); // e.g. "price_too_high"
                                                                                            console.log(event.suggested_response); // Suggested rep response
                                                                                                  break;

                                                                                                      case 'battlecard_triggered':
                                                                                                            // Competitor mentioned — show battlecard
                                                                                                                  console.log(event.competitor); // e.g. "Gong"
                                                                                                                        console.log(event.talking_points); // Array of talking points
                                                                                                                              break;
                                                                                                                              
                                                                                                                                  case 'coaching_tip':
                                                                                                                                        // General coaching suggestion
                                                                                                                                              console.log(event.tip);
                                                                                                                                                    break;
                                                                                                                                                    
                                                                                                                                                        case 'deal_signal':
                                                                                                                                                              // Positive/negative deal signal detected
                                                                                                                                                                    console.log(event.signal_type); // "buying_signal" | "risk_signal"
                                                                                                                                                                          console.log(event.context);
                                                                                                                                                                                break;
                                                                                                                                                                                  }
                                                                                                                                                                                  });
                                                                                                                                                                                  ```
                                                                                                                                                                                  
                                                                                                                                                                                  ---
                                                                                                                                                                                  
                                                                                                                                                                                  ### Reps
                                                                                                                                                                                  
                                                                                                                                                                                  #### List reps
                                                                                                                                                                                  
                                                                                                                                                                                  ```http
                                                                                                                                                                                  GET /v1/reps
                                                                                                                                                                                  ```
                                                                                                                                                                                  
                                                                                                                                                                                  #### Create a rep
                                                                                                                                                                                  
                                                                                                                                                                                  ```http
                                                                                                                                                                                  POST /v1/reps
                                                                                                                                                                                  ```
                                                                                                                                                                                  
                                                                                                                                                                                  ```json
                                                                                                                                                                                  {
                                                                                                                                                                                    "name": "Sarah Johnson",
                                                                                                                                                                                      "email": "sarah@yourcompany.com",
                                                                                                                                                                                        "crm_user_id": "sf_user_00500000001234",
                                                                                                                                                                                          "role": "ae"
                                                                                                                                                                                          }
                                                                                                                                                                                          ```
                                                                                                                                                                                          
                                                                                                                                                                                          ---
                                                                                                                                                                                          
                                                                                                                                                                                          ### Analytics
                                                                                                                                                                                          
                                                                                                                                                                                          #### Team performance
                                                                                                                                                                                          
                                                                                                                                                                                          ```http
                                                                                                                                                                                          GET /v1/analytics/team
                                                                                                                                                                                          ```
                                                                                                                                                                                          
                                                                                                                                                                                          Returns aggregated metrics: objection frequency, handle rates, coaching acceptance rate, deal win rate correlation.
                                                                                                                                                                                          
                                                                                                                                                                                          #### Rep performance
                                                                                                                                                                                          
                                                                                                                                                                                          ```http
                                                                                                                                                                                          GET /v1/analytics/reps/{rep_id}
                                                                                                                                                                                          ```
                                                                                                                                                                                          
                                                                                                                                                                                          ---
                                                                                                                                                                                          
                                                                                                                                                                                          ## Webhooks
                                                                                                                                                                                          
                                                                                                                                                                                          Register webhooks to receive real-time POST notifications when key events occur in Valfred.
                                                                                                                                                                                          
                                                                                                                                                                                          ### Register a webhook
                                                                                                                                                                                          
                                                                                                                                                                                          ```http
                                                                                                                                                                                          POST /v1/webhooks
                                                                                                                                                                                          ```
                                                                                                                                                                                          
                                                                                                                                                                                          ```json
                                                                                                                                                                                          {
                                                                                                                                                                                            "url": "https://yourserver.com/valfred-webhook",
                                                                                                                                                                                              "events": [
                                                                                                                                                                                                  "session.completed",
                                                                                                                                                                                                      "crm.synced",
                                                                                                                                                                                                          "objection.detected",
                                                                                                                                                                                                              "deal_signal.detected"
                                                                                                                                                                                                                ],
                                                                                                                                                                                                                  "secret": "your_webhook_secret"
                                                                                                                                                                                                                  }
                                                                                                                                                                                                                  ```
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  ### Webhook events
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  | Event | Description |
                                                                                                                                                                                                                  |---|---|
                                                                                                                                                                                                                  | `session.started` | A new call session has begun |
                                                                                                                                                                                                                  | `session.completed` | Session ended, summary available |
                                                                                                                                                                                                                  | `objection.detected` | Real-time objection flagged |
                                                                                                                                                                                                                  | `deal_signal.detected` | Buying or risk signal detected |
                                                                                                                                                                                                                  | `crm.synced` | CRM successfully updated post-call |
                                                                                                                                                                                                                  | `crm.sync_failed` | CRM sync error — retry or manual action needed |
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  ### Webhook payload example
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  ```json
                                                                                                                                                                                                                  {
                                                                                                                                                                                                                    "event": "session.completed",
                                                                                                                                                                                                                      "timestamp": "2025-04-21T15:17:27Z",
                                                                                                                                                                                                                        "data": {
                                                                                                                                                                                                                            "session_id": "sess_1a2b3c4d",
                                                                                                                                                                                                                                "rep_id": "rep_abc123",
                                                                                                                                                                                                                                    "deal_id": "deal_xyz789",
                                                                                                                                                                                                                                        "duration_seconds": 2847,
                                                                                                                                                                                                                                            "crm_synced": true,
                                                                                                                                                                                                                                                "objections_handled": 4
                                                                                                                                                                                                                                                  }
                                                                                                                                                                                                                                                  }
                                                                                                                                                                                                                                                  ```
                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                  ### Webhook signature verification
                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                  Every webhook request includes a `X-Valfred-Signature` header. Verify it using your webhook secret:
                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                  ```javascript
                                                                                                                                                                                                                                                  const crypto = require('crypto');
                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                  function verifySignature(payload, signature, secret) {
                                                                                                                                                                                                                                                    const expected = crypto
                                                                                                                                                                                                                                                        .createHmac('sha256', secret)
                                                                                                                                                                                                                                                            .update(payload)
                                                                                                                                                                                                                                                                .digest('hex');
                                                                                                                                                                                                                                                                  return `sha256=${expected}` === signature;
                                                                                                                                                                                                                                                                  }
                                                                                                                                                                                                                                                                  ```
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                  ---
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                  ## SDK
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                  ### JavaScript / Node.js
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                  ```bash
                                                                                                                                                                                                                                                                  npm install @valfred/sdk
                                                                                                                                                                                                                                                                  ```
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                  ```javascript
                                                                                                                                                                                                                                                                  import { Valfred } from '@valfred/sdk';
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                  const client = new Valfred({ apiKey: 'YOUR_API_KEY' });
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                  // Start a session
                                                                                                                                                                                                                                                                  const session = await client.sessions.create({
                                                                                                                                                                                                                                                                    repId: 'rep_abc123',
                                                                                                                                                                                                                                                                      dealId: 'deal_xyz789',
                                                                                                                                                                                                                                                                        callType: 'discovery',
                                                                                                                                                                                                                                                                          crm: 'hubspot'
                                                                                                                                                                                                                                                                          });
                                                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                                          // Listen for real-time events
                                                                                                                                                                                                                                                                          session.on('objection_detected', ({ objectionType, suggestedResponse }) => {
                                                                                                                                                                                                                                                                            displaySuggestion(suggestedResponse);
                                                                                                                                                                                                                                                                            });
                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                            session.on('deal_signal', ({ signalType, context }) => {
                                                                                                                                                                                                                                                                              updateDealPanel(signalType, context);
                                                                                                                                                                                                                                                                              });
                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                              // End the session
                                                                                                                                                                                                                                                                              await session.end();
                                                                                                                                                                                                                                                                              ```
                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                              ### Python
                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                              ```bash
                                                                                                                                                                                                                                                                              pip install valfred
                                                                                                                                                                                                                                                                              ```
                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                              ```python
                                                                                                                                                                                                                                                                              from valfred import Valfred
                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                              client = Valfred(api_key="YOUR_API_KEY")
                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                              session = client.sessions.create(
                                                                                                                                                                                                                                                                                  rep_id="rep_abc123",
                                                                                                                                                                                                                                                                                      deal_id="deal_xyz789",
                                                                                                                                                                                                                                                                                          call_type="demo",
                                                                                                                                                                                                                                                                                              crm="salesforce"
                                                                                                                                                                                                                                                                                              )
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              print(f"Session started: {session.session_id}")
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              # End session when call finishes
                                                                                                                                                                                                                                                                                              result = session.end()
                                                                                                                                                                                                                                                                                              print(f"CRM synced: {result.crm_synced}")
                                                                                                                                                                                                                                                                                              ```
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              ---
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              ## Error Codes
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              | HTTP Status | Code | Description |
                                                                                                                                                                                                                                                                                              |---|---|---|
                                                                                                                                                                                                                                                                                              | 400 | `invalid_request` | Missing or malformed parameters |
                                                                                                                                                                                                                                                                                              | 401 | `unauthorized` | Invalid or missing API key |
                                                                                                                                                                                                                                                                                              | 403 | `forbidden` | API key lacks required scope |
                                                                                                                                                                                                                                                                                              | 404 | `not_found` | Resource does not exist |
                                                                                                                                                                                                                                                                                              | 409 | `conflict` | Session already active for this deal |
                                                                                                                                                                                                                                                                                              | 422 | `crm_sync_error` | CRM authentication failed or sync error |
                                                                                                                                                                                                                                                                                              | 429 | `rate_limited` | Too many requests (see rate limits) |
                                                                                                                                                                                                                                                                                              | 500 | `internal_error` | Valfred server error |
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              ---
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              ## Rate Limits
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              | Plan | Requests/min | Concurrent Sessions |
                                                                                                                                                                                                                                                                                              |---|---|---|
                                                                                                                                                                                                                                                                                              | Starter | 60 | 5 |
                                                                                                                                                                                                                                                                                              | Growth | 300 | 25 |
                                                                                                                                                                                                                                                                                              | Enterprise | Unlimited | Unlimited |
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              Rate limit headers are included in every response:
                                                                                                                                                                                                                                                                                              ```
                                                                                                                                                                                                                                                                                              X-RateLimit-Limit: 300
                                                                                                                                                                                                                                                                                              X-RateLimit-Remaining: 287
                                                                                                                                                                                                                                                                                              X-RateLimit-Reset: 1713715200
                                                                                                                                                                                                                                                                                              ```
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              ---
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              ## Changelog
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              | Version | Date | Changes |
                                                                                                                                                                                                                                                                                              |---|---|---|
                                                                                                                                                                                                                                                                                              | v1.2 | 2025-04-01 | Added `deal_signal` WebSocket event; Python SDK GA |
                                                                                                                                                                                                                                                                                              | v1.1 | 2025-02-15 | HubSpot Workflows compatibility; webhook signature verification |
                                                                                                                                                                                                                                                                                              | v1.0 | 2025-01-10 | Initial public release |
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              ---
                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                              *Questions? Reach out to [developers@valfred.io](mailto:developers@valfred.io) or open an issue in this repo.*
