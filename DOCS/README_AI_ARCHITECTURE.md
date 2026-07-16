This repository contains the master engine blueprint for our multi-tenant, auto parts e-commerce platform. It enables multiple independent scrap yards (tenants) to manage inventory, utilize an AI chatbot, and process split payments via a single codebase.

## 🤖 AI Co-Working & Task Delegation Rules

To optimize development speed, reduce token consumption, and avoid rate limits, any AI Coding Agent reading this repository must follow these strict operational guidelines:

### 1. Claude 3.5 Sonnet Roles (The System Architect & Complex Logic Lead)
*   **Use Case:** High-level database schema design, multi-tenant state isolation, security routing, and third-party API integration pipelines (Stripe Connect, eBay Inventory API, Royal Mail Shipping API).
*   **Action Required:** Before writing code for any major milestone, Claude must sketch out the system design architecture first and wait for approval.

### 2. Qwen-Coder / Plus Roles (The Velocity Builder & Boilerplate Developer)
*   **Use Case:** Rapid implementation of frontend component views (the 3-step mobile wizard HTML/CSS), repetitive database CRUD routers, utility helpers, and formatting layers.
*   **Action Required:** Qwen should take the structural logic templates drafted by Claude and build out the complete file payloads quickly and efficiently.

---

## 🏗️ Production Runtime Routing Architecture

When processing live application data, the code must implement a multi-model routing structure using environment keys:

Incoming Customer Enquiries]
│
Is it a standard chat interaction?
┌──────────────────┴──────────────────┐
▼ ▼
[ YES ] [ NO ]
(General text / queries) (Complex image extraction)
│ │
Route to Qwen API Route to Claude API
(Lightning fast / Ultra cheap) (Multimodal Vision Engine)

1.  **Conversational Text Tier (Qwen API):** Standard client interactions with the frontend chatbot (e.g., location queries, business hours, simple parts lookups) must be processed through Qwen to maximize response velocity and lower operational overhead.
2.  **Multimodal & Reasoning Tier (Claude 3.5 Sonnet API):** Image-to-text extractions, complex data parsers, and low-confidence inventory matches must fallback to Claude to ensure high accuracy.

---

## 🗄️ Master System Micro-Blueprint

### 1. Database Table Configurations (`/database/schema.sql`)
The database engine must handle absolute multi-tenancy. No client configuration data should be hardcoded into views.

*   `tenants` table: `id` (PK), `domain_name`, `business_name`, `phone`, `city`, `stripe_connect_account_id`, `primary_color`, `secondary_color`, `logo_url`.
*   `inventory` table: `id` (PK), `tenant_id` (FK), `vrm_plate`, `make`, `model`, `year`, `engine_code`, `part_category`, `price`, `status` ('Active' | 'Sold').
*   `wishlists` table: `id` (PK), `tenant_id` (FK), `customer_phone`, `customer_name`, `vehicle_make`, `vehicle_model`, `part_category`, `is_fulfilled` (Boolean).

### 2. The 3-Step Touch-Optimized Worker Screen
The inventory worker interface requires a mobile-first step-by-step wizard dashboard to remain entirely "dummy-proof":
*   **Step 1:** Big tap target button to activate device camera and upload 2-3 images.
*   **Step 2:** One text input field for the UK VRM (Number Plate) and a grid layout of large categorical select panels (Engine, Gearbox, Lights, Body panels).
*   **Step 3:** One pricing numeric field and a prominent 'PUBLISH LIVE' button.

### 3. Automated Revenue Modules
*   **Stripe Connect Hook:** Multi-tenant split gateway processing. Calculate order sums, send the main amount directly to the corresponding `stripe_connect_account_id`, and route a 5% system technology fee to our master platform account.
*   **Twilio SMS Closer Trigger:** A post-insert database hook on the `inventory` table. Whenever a part shifts to 'Active', scan the `wishlists` table for unfulfilled matches matching `tenant_id`, `vehicle_make`, `vehicle_model`, and `part_category`. If found, instantly trigger an automated checkout SMS alert via Twilio and toggle `is_fulfilled = true`.

---

## 🛠️ Getting Started with Claude or Qwen

When you start working on a task:
1. Locate the file you need to modify.
2. Ensure you check for the global `tenant_id` context using standard request/session parameters.
3. Keep the file completely dynamic, matching the `.env` design framework.

**Acknowledge this blueprint layout before modifying any existing controllers or models.**
