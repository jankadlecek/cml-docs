# CML Documentation Repository

This repo contains API documentation for CML (Cloud Middleware Layer) - a data integration and orchestration platform.

## Key Files

- `api-docs.json` - Original OpenAPI 3.0 spec (monolithic, 400KB)
- `api-docs/` - Structured Markdown documentation broken down by domain (50 files)
- `api-docs/PROJECT.md` - CML architecture overview
- `api-docs/_overview.md` - Quick navigation map

## CML Knowledge Base (Confluence)

The documentation is also published to Confluence for AI agent access:

- **Space:** CAD (CML AI Docs) at `mbi-io.atlassian.net`
- **Use `/cml <question>` to query the knowledge base**

### Key Confluence Page IDs

| Page | ID |
|---|---|
| Homepage | `899023193` |
| Architecture Overview | `899678209` |
| Quick Reference Map | `899710977` |
| Orchestrations Schema | `900202497` |
| Input Clients - HTTP | `900038657` |
| Actions - Common | `900071426` |

## Custom Commands

- `/cml <question>` - Query the CML knowledge base in Confluence. Examples:
  - `/cml How do I configure HTTP pagination?`
  - `/cml Write an orchestration that fetches orders from an API and loads into Snowflake`
  - `/cml What auth types are supported?`
  - `/cml Update the Mapper schema page with an example config`
