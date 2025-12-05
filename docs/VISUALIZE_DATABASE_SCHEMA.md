# How to Visualize Hyperswitch Database Schema

The Hyperswitch database schema is documented in DBML (Database Markup Language) format. Here's how to view and work with it.

## Quick Start - View Online (2 minutes)

### Method 1: dbdiagram.io (Recommended - No Installation)

1. **Go to** [https://dbdiagram.io/d](https://dbdiagram.io/d)

2. **Click** "Import" (or press `Ctrl+I` / `Cmd+I`)

3. **Copy** the entire contents of [`database_schema.dbml`](./database_schema.dbml)

4. **Paste** into the import dialog

5. **Done!** You'll see an interactive ER diagram with:
   - All tables and their columns
   - Relationships between tables
   - Primary and foreign keys
   - Data types and constraints

6. **Features you can use:**
   - Zoom in/out
   - Pan around the diagram
   - Click on tables to highlight relationships
   - Export as PNG, PDF, or SQL
   - Share your diagram with a link

### Method 2: dbdocs.io (Generate Documentation Website)

1. **Install dbdocs CLI:**
   ```bash
   npm install -g dbdocs
   ```

2. **Login (create free account if needed):**
   ```bash
   dbdocs login
   ```

3. **Build and publish:**
   ```bash
   cd docs
   dbdocs build database_schema.dbml
   ```

4. **Access** the generated documentation website at the provided URL

## Working with DBML Locally

### Convert to SQL

Generate PostgreSQL schema SQL:

```bash
# Install dbml-cli
npm install -g @dbml/cli

# Generate SQL
dbml2sql database_schema.dbml --postgres -o schema.sql
```

### Convert to other formats

```bash
# Generate JSON
dbml2sql database_schema.dbml --json -o schema.json

# Generate MySQL
dbml2sql database_schema.dbml --mysql -o schema_mysql.sql
```

## Understanding the Diagram

### Color Coding (on dbdiagram.io)
- **Tables** are represented as boxes
- **Lines** represent relationships:
  - `>` : one-to-many relationship
  - `-` : one-to-one relationship
  - `<>` : many-to-many (via junction table)

### Key Tables to Explore

1. **Start with Core Payment Flow:**
   - `payment_intent` → `payment_attempt` → `captures` → `refund`

2. **Then Explore:**
   - **Merchant Setup**: `organization` → `merchant_account` → `business_profile`
   - **Customer Data**: `customers` → `address` → `payment_methods`
   - **Security**: `authentication` (3DS) → `fraud_check`
   - **User Access**: `users` → `user_roles` → `roles`

3. **Advanced Features:**
   - **Routing**: `routing_algorithm` → `dynamic_routing_stats`
   - **Subscriptions**: `subscription` → `invoice`
   - **Disputes**: `dispute` → `evidence`

## Tips for Large Schemas

The Hyperswitch schema has 50+ tables. Here are tips for navigating:

1. **Use Search** (in dbdiagram.io): 
   - Press `/` to search for table names
   - Search for "payment", "customer", "merchant" etc.

2. **Focus on Specific Areas**:
   - Create multiple diagrams for different domains
   - Copy relevant tables into a new diagram

3. **Follow Relationships**:
   - Click on a table to highlight its relationships
   - Use the minimap to navigate large diagrams

## Keeping Schema Up to Date

The DBML file is generated from:
- **Source**: `crates/diesel_models/src/schema.rs`
- **Migrations**: `migrations/` directory

To update after schema changes:
1. Apply database migrations
2. Regenerate DBML from the updated Diesel schema
3. Re-import into dbdiagram.io or rebuild dbdocs

## Troubleshooting

### DBML Import Errors

If you get errors when importing:
1. Check the DBML syntax (look for missing brackets, quotes)
2. Try importing in sections (split the file)
3. Verify DBML version compatibility

### Diagram Too Large

If the diagram is overwhelming:
1. Use the search feature to find specific tables
2. Export specific sections as separate diagrams
3. Use the zoom controls to navigate
4. Focus on key tables first (payment_intent, merchant_account, customers)

## Additional Resources

- **DBML Documentation**: https://www.dbml.org/docs/
- **dbdiagram.io Guides**: https://dbdiagram.io/docs
- **dbdocs.io Guides**: https://dbdocs.io/docs
- **Hyperswitch Docs**: https://docs.hyperswitch.io/

## Examples

### Example 1: Payment Flow Diagram

Focus on these tables for payment processing:
- payment_intent
- payment_attempt
- captures
- refund
- dispute

### Example 2: User Management

Focus on these tables for RBAC:
- users
- user_roles
- roles
- api_keys
- organization
- merchant_account

### Example 3: Fraud Prevention

Focus on these tables for security:
- authentication
- fraud_check
- blocklist
- blocklist_fingerprint
- gateway_status_map

---

**Questions or Issues?**

For questions about the database schema or this documentation:
1. Check the main [DATABASE_SCHEMA.md](./DATABASE_SCHEMA.md)
2. Review the inline comments in [database_schema.dbml](./database_schema.dbml)
3. Consult the Hyperswitch documentation
