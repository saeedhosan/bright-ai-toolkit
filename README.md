# BrightAI — Laravel MSP Platform


## 📝 Project Description

BrightAI is a **developer-oriented toolkit** and **Managed Service Provider (MSP) platform** that integrates modern large-language-model (LLM) capabilities into scalable, multi-tenant Laravel applications. The core is a production-ready foundation for adding AI-driven features (chat, content generation, prompt orchestration) to web apps, while the overall platform provides a strict, logically-isolated hierarchy to serve **System Administrators**, **MSPs**, **Companies**, and **End Users (Clients)**.

The project's purpose is to offer a reusable, auditable, and extensible integration with LLM providers alongside robust subscription, billing, and white-labeling features for the MSP business model.

## 🎯 The Challenge / Problem Solved

The project addresses two main challenges:

1.  **MSP Scalability and Isolation:** Building a platform that strictly isolates data and business logic across the hierarchy: **System → MSP → Company → Client**. This is crucial for security, white-labeling, and monetization, requiring complex **multi-tenancy** with deny-by-default access control.
2.  **LLM Integration Complexity:** The difficulty of integrating LLMs reliably, which involves managing multiple providers, handling streaming, ensuring fault tolerance (retries/fallbacks), and addressing production concerns like **rate limiting**, **cost tracking**, and **PII redaction**.

## 💡 The Solution

The solution is a cohesive platform built on the **Laravel 12** framework and modern architectural patterns:

* **Strict Multi-Tenancy:** Implemented using a **Tenant Context Resolver** that determines the MSP and Company scope from the request (domain/session/token) before executing any business logic. All tenant-owned tables are scoped with a mandatory `company_id` column, and Eloquent models use a `TenantScoped` trait to enforce this isolation by default.
* **Modular Architecture:** The platform is segmented into an **Experience Layer** (portals/APIs), an **Application Layer** (business modules like Billing, Entitlements, Ticketing), and an **Infrastructure Layer** (DB, Queue, Caching).
* **LLM Toolkit:** A provider-agnostic LLM integration layer with centralized **prompt templates**, conversation session management, and robust observability (prompt logging, usage metrics) is provided to safely embed AI features.
* **Monetization Engine:** Uses **Laravel Cashier** for subscriptions and **Stripe** for payment processing and tax/invoicing, supporting complex tiered, seat-based, and metered billing models.

## 🚀 Features

### MSP Platform (Core)
* **Hierarchical Isolation:** System → MSP → Company → Client with deny-by-default cross-MSP access and granular **RBAC** (using `spatie/laravel-permission`).
* **Product & Service Catalog:** Centralized management of products, plans, features, and bundles.
* **Subscription & Billing Engine:** Supports tiered, **seat-based**, and **metered usage** billing, proration, multi-currency, taxes/VAT (via Stripe), and dunning.
* **White-Labeling:** Custom logo, colors, and domain (CNAME) per MSP and optionally per Company.
* **Ticketing & SLA:** Support system with policies, assignment, and MSP-scoped views.
* **Audit Logging:** Immutable store for every mutation with actor and scope (`msp_id`, `company_id`).

### AI Capabilities (BrightAI Toolkit)
* **Provider-Agnostic LLM Integration:** Pluggable adapters (e.g., using `openai-php/laravel`).
* **Centralized Prompt Templates:** Managed via the Admin UI/Studio for consistency and versioning.
* **Conversation Management:** Session history, roles, and context truncation.
* **Observability & Quotas:** Prompt logging, usage metrics, and per-user/org quota management.

## 🛠️ Tech Stack

The platform is built on the **Laravel React Starter Kit** base.

| Category | Technology | Composer Package |
| :--- | :--- | :--- |
| **Backend** | PHP 8.2+, Laravel 12.x | `laravel/framework: ^12.0` |
| **Frontend** | Livwire | `livewire/livewire: ^3.0` |
| **Database** | MySQL & Sqlite |  |
| **Billing/Payments** | Stripe, Laravel Cashier | `laravel/cashier: ^16.0` |
| **Authorization** | RBAC, Permissions | `spatie/laravel-permission` |
| **AI/LLM** | OpenAI adapter | `openai-php/laravel: ^0.17.0` |
| **Realtime** | Pusher | `pusher/pusher-php-server` |
| **Dev Tools** | Pail, Telescope, Sanctum, Tinker | `laravel/telescope: ^5.13`, `laravel/sanctum` |
| **Messaging** | Twilio, Mailgun | `twilio/sdk: ^8.0`, `symfony/mailgun-mailer: ^7.3` |
| **Utilities** | PDF (Dompdf), Excel (Maatwebsite), QR Codes | `barryvdh/laravel-dompdf`, `maatwebsite/excel`, `chillerlan/php-qrcode` |

## 📈 Results and Impact

* **Secure & Scalable Tenancy:** Established a secure logical isolation model, enabling the onboarding of multiple MSPs and their hundreds of Companies without cross-tenant data leakage (deny-by-default logic).
* **Accelerated LLM Feature Development:** The BrightAI toolkit provides a reusable, pre-audited integration layer, reducing the time-to-market for new AI-driven features like conversational assistants and content generation.
* **Robust Monetization:** Full support for complex MSP-specific billing (tiered, seat, metered usage) using the proven Laravel Cashier/Stripe integration, ensuring accurate revenue tracking.

## ⚙️ Installation and Usage

Note: The Installation and usage process are only available for Bright team. <br>
These instructions assume Docker (for Sail) and Node/NPM are installed.

1.  Clone the repository: `git clone [repo]`
2.  Install PHP/Composer dependencies: `composer install`
3.  Install JavaScript dependencies: `npm install`
4.  Set up environment variables: `cp .env.example .env` and fill in secrets (DB, Stripe, OpenAI, etc.).
5.  Run setup commands:
    ```bash
    php artisan key:generate
    php artisan migrate
    php artisan db:seed # To populate roles, initial data
    ```
6.  Run the application in development mode (starts server, queue listener, logs, and Vite):
    ```bash
    composer run dev
    ```
7.  View the application at: `http://localhost:[port]` (typically `http://localhost:8000`)
8.  Code format and style with strict types
    ```bash
    composer lint # with laravel pint
    composer types # with phpstan
    ```

## 🤝 Contributing

This project is currently for internal development only. Please direct all bug reports and feature requests to the internal issue tracker or contact the project lead.

## 👤 Author/Contact

* **Saeed Hosan** 
* appsaeed7@gmail.com
* https://www.linkedin.com/in/saeedhosan

## 📄 License

This project is proprietary software developed for a private company. The source code is not publicly licensed for reuse or distribution.
