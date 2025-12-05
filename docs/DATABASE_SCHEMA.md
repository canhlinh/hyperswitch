# Hyperswitch Database Schema

This directory contains the database schema documentation for the Hyperswitch payment orchestration platform.

## Files

- **database_schema.dbml** - Complete database schema in DBML (Database Markup Language) format

## Schema Overview

The Hyperswitch database is designed as a multi-tenant payment orchestration platform with the following key components:

### Core Entities

1. **Organizations & Merchants**
   - `organization` - Top-level entity
   - `merchant_account` - Merchant configurations
   - `merchant_connector_account` - Payment gateway integrations
   - `business_profile` - Multiple payment configurations per merchant

2. **Payments Flow**
   - `payment_intent` - Payment transactions
   - `payment_attempt` - Retry attempts for each payment
   - `refund` - Refund transactions
   - `captures` - Capture records for two-step auth/capture
   - `incremental_authorization` - Incremental auth increases

3. **Customers**
   - `customers` - Customer records with encrypted PII
   - `address` - Billing and shipping addresses
   - `payment_methods` - Stored payment methods (cards, wallets, etc.)
   - `mandate` - Recurring payment agreements

4. **Security & Fraud**
   - `authentication` - 3DS authentication records
   - `fraud_check` - Fraud/risk management checks
   - `blocklist` / `blocklist_fingerprint` / `blocklist_lookup` - Fraud prevention

5. **Payouts**
   - `payouts` - Payout transactions
   - `payout_attempt` - Payout attempt records

6. **Disputes**
   - `dispute` - Chargeback/dispute records

7. **Events & Webhooks**
   - `events` - Event tracking and webhook delivery

8. **User Management & Access Control**
   - `users` - User accounts
   - `user_roles` - User-role assignments (RBAC)
   - `roles` - Role definitions
   - `api_keys` - API key authentication

9. **Subscriptions**
   - `subscription` - Recurring billing
   - `invoice` - Subscription invoices

10. **Routing & Configuration**
    - `routing_algorithm` - Payment routing rules
    - `dynamic_routing_stats` - Success-based routing statistics
    - `gateway_status_map` - Error code mapping
    - `configs` - System configuration

## Visualizing the Schema

### Option 1: dbdiagram.io (Recommended)

1. Visit [https://dbdiagram.io](https://dbdiagram.io)
2. Click "Import" or create a new diagram
3. Copy the entire contents of `database_schema.dbml`
4. Paste it into the editor
5. The diagram will be generated automatically

### Option 2: dbdocs.io (Documentation)

1. Install the dbdocs CLI:
   ```bash
   npm install -g dbdocs
   ```

2. Build and publish documentation:
   ```bash
   dbdocs build docs/database_schema.dbml
   ```

3. View the generated documentation at the provided URL

### Option 3: CLI Tools

You can also use various DBML CLI tools to generate diagrams locally:

```bash
# Install dbml-cli
npm install -g @dbml/cli

# Generate SQL from DBML
dbml2sql docs/database_schema.dbml --postgres -o schema.sql

# Generate diagram (requires additional tools)
```

## Key Design Patterns

### Multi-Tenancy
The schema supports multi-tenancy through:
- Organizations contain multiple merchants
- Merchants can have multiple business profiles
- All entities are scoped to merchant_id and/or organization_id

### Data Encryption
Sensitive data is encrypted at rest:
- Customer PII (names, emails, phones, addresses)
- Payment method data
- Merchant credentials and configurations
- Keys stored in `merchant_key_store` and `user_key_store`

### Audit Trail
Comprehensive tracking through:
- `created_at` / `modified_at` timestamps on all tables
- `created_by` / `updated_by` / `last_modified_by` fields
- `events` table for webhook and event tracking

### Payment Flow
1. **Payment Intent** created with amount and currency
2. **Payment Attempt** made through a connector (gateway)
3. Optional **Authentication** (3DS) if required
4. Optional **Fraud Check** for risk management
5. **Capture** of authorized funds (or direct charge)
6. Optional **Refund** if needed
7. Possible **Dispute** handling

### Routing Intelligence
- `routing_algorithm` defines how payments are routed
- `dynamic_routing_stats` tracks success rates
- `gateway_status_map` provides unified error handling

## Schema Statistics

- **Total Tables**: ~50+ tables
- **Database**: PostgreSQL
- **Key Features**:
  - Composite primary keys for tenant isolation
  - JSONB columns for flexible metadata
  - Encrypted bytea columns for PII
  - Comprehensive foreign key relationships
  - Support for multiple payment flows

## Maintenance

The DBML schema is generated from the Diesel schema definition file:
- Source: `crates/diesel_models/src/schema.rs`
- Migration files: `migrations/`

When the database schema changes:
1. Update the Diesel schema via migrations
2. Regenerate this DBML file to reflect the changes
3. Update this documentation as needed

## Additional Resources

- [DBML Documentation](https://www.dbml.org/docs/)
- [dbdiagram.io](https://dbdiagram.io) - Online ER diagram tool
- [dbdocs.io](https://dbdocs.io) - Database documentation tool
- [Hyperswitch Documentation](https://docs.hyperswitch.io/)

## License

This schema documentation follows the same license as the Hyperswitch project.
