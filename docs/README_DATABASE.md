# Database Schema Documentation

This directory contains comprehensive documentation of the Hyperswitch database schema.

## 📊 Quick Links

| Document | Description |
|----------|-------------|
| [**database_schema.dbml**](./database_schema.dbml) | Complete database schema in DBML format (1100+ lines) |
| [**DATABASE_SCHEMA.md**](./DATABASE_SCHEMA.md) | Detailed schema overview and design patterns |
| [**VISUALIZE_DATABASE_SCHEMA.md**](./VISUALIZE_DATABASE_SCHEMA.md) | Step-by-step guide to visualize the schema |

## 🚀 Quick Start

### View the Database Diagram (2 minutes)

1. Go to [https://dbdiagram.io/d](https://dbdiagram.io/d)
2. Click "Import"
3. Copy and paste the contents of [`database_schema.dbml`](./database_schema.dbml)
4. Done! You'll see an interactive ER diagram

### Generate Documentation Website

```bash
npm install -g dbdocs
dbdocs login
dbdocs build database_schema.dbml
```

## 📚 What's Inside

The database schema includes **50+ tables** covering:

### Core Components
- ✅ **Payment Processing** - payment_intent, payment_attempt, captures, refund
- ✅ **Merchant Management** - organization, merchant_account, business_profile
- ✅ **Customer Data** - customers, address, payment_methods
- ✅ **Security** - authentication (3DS), fraud_check, blocklist
- ✅ **User Management** - users, roles, api_keys (RBAC)
- ✅ **Payouts** - payout transactions and attempts
- ✅ **Disputes** - chargeback handling
- ✅ **Subscriptions** - recurring billing, invoices
- ✅ **Events** - webhook delivery and audit trail
- ✅ **Routing** - intelligent payment routing algorithms

### Key Features
- 🔒 **Encrypted PII** - Customer data encrypted at rest
- 🏢 **Multi-tenancy** - Organization → Merchant → Profile hierarchy
- 🔄 **Comprehensive Audit** - created_at, modified_at, created_by tracking
- 🌐 **Multi-gateway** - Support for multiple payment processors
- 📊 **Analytics Ready** - Event tracking and routing statistics

## 🎯 Use Cases

### For Developers
- Understand the data model before coding
- Plan database queries and joins
- Design new features with existing patterns
- Review migration impact

### For Architects
- Understand system design and relationships
- Plan scaling and partitioning strategies
- Review security and encryption patterns
- Design integration points

### For Product Managers
- Understand data flows and capabilities
- Plan feature requirements
- Review business logic implementation
- Understand multi-tenancy model

## 🛠️ Technical Details

- **Database**: PostgreSQL
- **ORM**: Diesel (Rust)
- **Schema Source**: `crates/diesel_models/src/schema.rs`
- **Migrations**: `migrations/` directory
- **Format**: DBML (Database Markup Language)

## 📖 Learn More

- [DBML Documentation](https://www.dbml.org/docs/)
- [dbdiagram.io](https://dbdiagram.io) - Interactive ER diagrams
- [dbdocs.io](https://dbdocs.io) - Database documentation
- [Hyperswitch Docs](https://docs.hyperswitch.io/)

## 🤝 Contributing

When updating the database schema:

1. Create and apply migrations in `migrations/`
2. Update `crates/diesel_models/src/schema.rs`
3. Regenerate `database_schema.dbml` to reflect changes
4. Update documentation as needed

---

**Need Help?**
- 📖 Read [DATABASE_SCHEMA.md](./DATABASE_SCHEMA.md) for detailed overview
- 🎨 Read [VISUALIZE_DATABASE_SCHEMA.md](./VISUALIZE_DATABASE_SCHEMA.md) for visualization guide
- 💬 Check the main Hyperswitch documentation
