# RubyGems

RubyGems.org is the Ruby community's primary gem hosting service, providing the infrastructure for publishing, discovering, and installing Ruby gems. The RubyGems API enables programmatic access to gem metadata, version information, download statistics, search, owner management, webhooks, and the compact index used by Bundler.

**Human URL:** https://guides.rubygems.org/

## APIs

| API | Description |
|-----|-------------|
| [RubyGems Gems API](openapi/rubygems-gems-api-openapi.yml) | Gem metadata, ownership, publishing, and OIDC trusted publishing |
| [RubyGems API V2](openapi/rubygems-api-v2-openapi.yml) | Next-generation gem version details API |
| [RubyGems Downloads API](openapi/rubygems-downloads-api-openapi.yml) | Download count statistics for gems and versions |
| [RubyGems Search API](openapi/rubygems-search-api-openapi.yml) | Full-text gem search |
| [RubyGems Activity API](openapi/rubygems-activity-api-openapi.yml) | Recently added and updated gem feeds |
| [RubyGems Webhooks API](openapi/rubygems-webhooks-api-openapi.yml) | Webhook subscriptions for gem push events |

## OpenAPI Specifications

| Spec | Description |
|------|-------------|
| [rubygems-gems-api-openapi.yml](openapi/rubygems-gems-api-openapi.yml) | Gems, Versions, Owners, Profiles, Dependencies, API Keys |
| [rubygems-api-v2-openapi.yml](openapi/rubygems-api-v2-openapi.yml) | API V2 gem version details endpoint |
| [rubygems-downloads-api-openapi.yml](openapi/rubygems-downloads-api-openapi.yml) | Download statistics endpoints |
| [rubygems-search-api-openapi.yml](openapi/rubygems-search-api-openapi.yml) | Search endpoint |
| [rubygems-activity-api-openapi.yml](openapi/rubygems-activity-api-openapi.yml) | Activity feed endpoints |
| [rubygems-webhooks-api-openapi.yml](openapi/rubygems-webhooks-api-openapi.yml) | Webhook management endpoints |

## AsyncAPI Specifications

| Spec | Description |
|------|-------------|
| [rubygems-webhooks-asyncapi.yml](asyncapi/rubygems-webhooks-asyncapi.yml) | Webhook event schemas for gem push notifications |

## Rules

| Ruleset | Description |
|---------|-------------|
| [rubygems-spectral-rules.yml](rules/rubygems-spectral-rules.yml) | Spectral ruleset enforcing RubyGems API design conventions |

## Capabilities

### Workflow Capabilities

| Capability | Description |
|------------|-------------|
| [gem-discovery.yaml](capabilities/gem-discovery.yaml) | Gem search, metadata, version history, and dependency analysis |
| [gem-publishing.yaml](capabilities/gem-publishing.yaml) | Gem publishing, ownership management, and webhook setup |

### Shared Definitions

| Shared | Description |
|--------|-------------|
| [gems-api.yaml](capabilities/shared/gems-api.yaml) | Per-API definition for RubyGems Gems API v1 |
| [search-api.yaml](capabilities/shared/search-api.yaml) | Per-API definition for RubyGems Search API |
| [webhooks-api.yaml](capabilities/shared/webhooks-api.yaml) | Per-API definition for RubyGems Webhooks API |

## Schemas

| Schema | Description |
|--------|-------------|
| [rubygems-gem-schema.json](json-schema/rubygems-gem-schema.json) | JSON Schema for Ruby gem records |

## Structures

| Structure | Description |
|-----------|-------------|
| [rubygems-structure.json](json-structure/rubygems-structure.json) | JSON structure documentation for RubyGems data entities |

## JSON-LD

| Context | Description |
|---------|-------------|
| [rubygems-context.jsonld](json-ld/rubygems-context.jsonld) | JSON-LD context mapping RubyGems vocabulary to schema.org |

## Examples

| Example | Description |
|---------|-------------|
| [rubygems-get-gem-info-example.json](examples/rubygems-get-gem-info-example.json) | Get detailed information about the rails gem |
| [rubygems-search-gems-example.json](examples/rubygems-search-gems-example.json) | Search for JSON parser gems |

## Vocabulary

| Vocabulary | Description |
|------------|-------------|
| [rubygems-vocabulary.yml](vocabulary/rubygems-vocabulary.yml) | Domain vocabulary for Ruby gems, versioning, and publishing |

## Maintainers

**FN:** Kin Lane  
**Email:** kin@apievangelist.com
