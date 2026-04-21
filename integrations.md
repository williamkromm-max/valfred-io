# Valfred — CRM & Tool Integrations

Complete list of platforms compatible with Valfred, with setup status and integration depth.

---

## CRM Platforms

### Salesforce
- **Status:** ✅ Native integration (GA)
- **API:** Salesforce REST API v55+
- **Sync:** Bi-directional
- **Objects synced:** Leads, Contacts, Accounts, Opportunities, Activities, Tasks, Custom Objects
- **Features:**
  - Real-time opportunity stage updates based on call signals
    - Auto-create follow-up tasks post-call
      - Call summary pushed as Activity log
        - Custom field mapping supported
          - Supports Salesforce Sandbox for testing

          **Setup:** [Salesforce Integration Guide](#salesforce-setup)

          ---

          ### HubSpot
          - **Status:** ✅ Native integration (GA)
          - **API:** HubSpot API v3
          - **Sync:** Bi-directional
          - **Objects synced:** Contacts, Companies, Deals, Engagements, Notes, Tasks
          - **Features:**
            - Deal stage progression from call outcomes
              - Engagement logging (calls, emails, meetings)
                - Auto-note creation with AI call summary
                  - Timeline event injection
                    - HubSpot Workflows compatible (trigger on Valfred events)

                    **Setup:** [HubSpot Integration Guide](#hubspot-setup)

                    ---

                    ### Pipedrive
                    - **Status:** ✅ Available (GA)
                    - **API:** Pipedrive REST API v1
                    - **Sync:** Bi-directional
                    - **Objects synced:** Deals, Contacts, Organizations, Activities, Notes
                    - **Features:**
                      - Deal stage auto-update
                        - Activity creation post-call
                          - Note logging with call summary

                          ---

                          ### Zoho CRM
                          - **Status:** ✅ Available (GA)
                          - **API:** Zoho CRM REST API v5
                          - **Sync:** Outbound (Valfred → Zoho)
                          - **Objects synced:** Leads, Contacts, Deals, Calls, Notes
                          - **Features:**
                            - Call activity logging
                              - Lead/Contact note updates

                              ---

                              ### Microsoft Dynamics 365
                              - **Status:** ✅ Available (Enterprise plan)
                              - **API:** Microsoft Dataverse API
                              - **Sync:** Bi-directional
                              - **Objects synced:** Opportunities, Contacts, Accounts, Activities
                              - **Features:**
                                - Opportunity stage sync
                                  - Activity timeline updates
                                    - Custom entity mapping

                                    ---

                                    ### Freshsales
                                    - **Status:** 🔜 Coming Q3 2025
                                    - **API:** Freshsales REST API

                                    ---

                                    ### Close CRM
                                    - **Status:** 🔜 Coming Q3 2025
                                    - **API:** Close API v1

                                    ---

                                    ## Conversation Intelligence & Call Platforms

                                    ### Zoom
                                    - **Status:** ✅ Available
                                    - **Integration type:** Zoom Meeting SDK + Zoom Phone
                                    - **Features:** Real-time audio capture, live transcription, in-call overlay for rep coaching

                                    ### Google Meet
                                    - **Status:** ✅ Available
                                    - **Integration type:** Chrome Extension + Meet API
                                    - **Features:** Live transcription, real-time suggestion overlay

                                    ### Microsoft Teams
                                    - **Status:** ✅ Available
                                    - **Integration type:** Teams Bot + Meeting API
                                    - **Features:** Meeting bot joins call, live coaching panel

                                    ### Aircall
                                    - **Status:** ✅ Available
                                    - **Integration type:** Aircall API + Webhook
                                    - **Features:** VoIP call capture, live coaching, CRM push after call

                                    ### RingCentral
                                    - **Status:** ✅ Available
                                    - **Integration type:** RingCentral API
                                    - **Features:** Call capture, post-call CRM sync

                                    ### Dialpad
                                    - **Status:** 🔜 Coming Q4 2025

                                    ### Twilio Flex
                                    - **Status:** 🔜 Coming Q4 2025

                                    ---

                                    ## Sales Engagement Platforms

                                    ### Outreach.io
                                    - **Status:** ✅ Available
                                    - **Sync:** Call activity pushed to Outreach sequence timeline
                                    - **Features:** Sequence step completion, activity logging

                                    ### Salesloft
                                    - **Status:** ✅ Available
                                    - **Sync:** Cadence activity logging, call disposition sync
                                    - **Features:** Call notes, disposition updates, follow-up task creation

                                    ### Apollo.io
                                    - **Status:** 🔜 Coming Q3 2025

                                    ---

                                    ## Post-Call Intelligence (Complementary)

                                    ### Gong
                                    - **Status:** ✅ Compatible (complementary)
                                    - **Notes:** Valfred handles real-time coaching; Gong handles post-call recording and analytics. The two tools work side-by-side.

                                    ### Chorus.ai (ZoomInfo)
                                    - **Status:** ✅ Compatible (complementary)
                                    - **Notes:** Same complementary model as Gong.

                                    ---

                                    ## Authentication Methods

                                    | Platform | Auth Method |
                                    |---|---|
                                    | Salesforce | OAuth 2.0 (connected app) |
                                    | HubSpot | OAuth 2.0 or Private App Token |
                                    | Pipedrive | API Token or OAuth 2.0 |
                                    | Zoho CRM | OAuth 2.0 |
                                    | MS Dynamics 365 | Azure AD OAuth 2.0 |
                                    | Zoom | OAuth 2.0 |
                                    | Google Meet | Google OAuth 2.0 |
                                    | Microsoft Teams | Azure AD OAuth 2.0 |
                                    | Aircall | API Key + Secret |
                                    | Outreach | OAuth 2.0 |
                                    | Salesloft | OAuth 2.0 |

                                    ---

                                    ## Integration Depth Legend

                                    | Symbol | Meaning |
                                    |---|---|
                                    | ✅ | Generally Available |
                                    | 🔜 | On roadmap (date indicated) |
                                    | ⚠️ | Beta / Limited availability |
                                    | ❌ | Not supported |

                                    ---

                                    ## Setup Guides

                                    ### Salesforce Setup

                                    1. Go to **Valfred Dashboard → Integrations → Salesforce**
                                    2. Click **Connect Salesforce**
                                    3. Authenticate via OAuth 2.0 (you'll be redirected to Salesforce login)
                                    4. Grant the required permissions (read/write on Opportunities, Contacts, Activities)
                                    5. Configure field mapping (optional — defaults work for standard Salesforce orgs)
                                    6. Click **Save & Test Connection**

                                    Required Salesforce permissions:
                                    - `read` / `write` on: Opportunity, Contact, Account, Task, Event, CallLog

                                    ---

                                    ### HubSpot Setup

                                    1. Go to **Valfred Dashboard → Integrations → HubSpot**
                                    2. Click **Connect HubSpot**
                                    3. Authenticate via HubSpot OAuth (select your portal)
                                    4. Grant scopes: `crm.objects.deals.write`, `crm.objects.contacts.write`, `timeline`, `engagements`
                                    5. Configure deal stage mapping (Valfred signal → HubSpot stage)
                                    6. Click **Save & Test Connection**

                                    ---

                                    *For enterprise SSO and custom integration support, contact: integrations@valfred.io*
