# RubyGems (rubygems)
RubyGems.org is the Ruby community's primary gem hosting service, providing a centralized repository for publishing, discovering, and managing Ruby libraries and packages. Their developer platform offers a comprehensive set of APIs for interacting with the gem registry, including gem metadata retrieval, search, download statistics, webhooks, and programmatic gem publishing.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/rubygems/refs/heads/main/apis.yml)

## Scope

- **Type:** Contract
- **Position:** Consuming
- **Access:** 3rd-Party

## Tags:

 - Packages, Ruby, Gems, Package Management, Software Distribution, Open Source

## Timestamps

- **Created:** 2026-03-20
- **Modified:** 2026-03-20

## APIs

### RubyGems Gems API
The RubyGems Gems API provides endpoints for retrieving detailed information about Ruby gems hosted on RubyGems.org, including gem metadata, version history, dependencies, and download statistics. Developers can query individual gems by name, list gems owned by a specific user, and submit or yank gem versions programmatically. The API supports both JSON and YAML response formats and is the primary interface for tooling that interacts with the RubyGems.org package registry.

**Human URL:** [https://guides.rubygems.org/rubygems-org-api/](https://guides.rubygems.org/rubygems-org-api/)


#### Tags:

 - Packages, Ruby, Gems, Package Management, Software Distribution

#### Properties

- [Documentation](https://guides.rubygems.org/rubygems-org-api/)
- [OpenAPI](openapi/rubygems-gems-api-openapi.yml)

### RubyGems Search API
The RubyGems Search API allows developers to search for gems on RubyGems.org by matching a query string against gem names and descriptions. It returns an array of matching active gems with their metadata, making it useful for building gem discovery tools, IDE integrations, and dependency resolution utilities. The search endpoint supports pagination for handling large result sets.

**Human URL:** [https://guides.rubygems.org/rubygems-org-api/](https://guides.rubygems.org/rubygems-org-api/)


#### Tags:

 - Search, Ruby, Gems, Package Discovery

#### Properties

- [Documentation](https://guides.rubygems.org/rubygems-org-api/)
- [OpenAPI](openapi/rubygems-search-api-openapi.yml)

### RubyGems Webhooks API
The RubyGems Webhooks API enables developers to register, list, test, and remove webhook subscriptions that fire when gems are pushed to RubyGems.org. Webhooks can be configured for specific gems or applied globally using a wildcard to receive notifications for all gem pushes. Each webhook request includes an Authorization header for verification, allowing downstream systems to confirm the notification originated from RubyGems.org.

**Human URL:** [https://guides.rubygems.org/rubygems-org-api/](https://guides.rubygems.org/rubygems-org-api/)


#### Tags:

 - Webhooks, Notifications, Ruby, Events

#### Properties

- [Documentation](https://guides.rubygems.org/rubygems-org-api/)
- [OpenAPI](openapi/rubygems-webhooks-api-openapi.yml)
- [AsyncAPI](asyncapi/rubygems-webhooks-asyncapi.yml)

### RubyGems Activity API
The RubyGems Activity API provides endpoints that return the most recently added and most recently updated gems on RubyGems.org. The latest endpoint returns the 50 gems most recently published to the registry, while the just-updated endpoint returns the 50 most recently modified gems. These endpoints are useful for building activity feeds, monitoring new releases, and tracking changes across the Ruby ecosystem.

**Human URL:** [https://guides.rubygems.org/rubygems-org-api/](https://guides.rubygems.org/rubygems-org-api/)


#### Tags:

 - Activity, Ruby, Gems, Updates, Recent

#### Properties

- [Documentation](https://guides.rubygems.org/rubygems-org-api/)
- [OpenAPI](openapi/rubygems-activity-api-openapi.yml)

### RubyGems Downloads API
The RubyGems Downloads API provides access to download count statistics for gems hosted on RubyGems.org. Developers can retrieve total download counts for specific gems and individual gem versions. This data is commonly used to gauge gem popularity, generate usage reports, and build dashboards that track adoption trends across the Ruby package ecosystem.

**Human URL:** [https://guides.rubygems.org/rubygems-org-api/](https://guides.rubygems.org/rubygems-org-api/)


#### Tags:

 - Downloads, Statistics, Ruby, Gems, Analytics

#### Properties

- [Documentation](https://guides.rubygems.org/rubygems-org-api/)
- [OpenAPI](openapi/rubygems-downloads-api-openapi.yml)

### RubyGems API V2
The RubyGems API V2 is the next-generation API for interacting with RubyGems.org, providing improved endpoints for querying gem versions and dependency information. It offers more structured responses and enhanced filtering capabilities compared to the V1 API. The V2 API is designed to better support modern tooling and dependency resolution workflows used by bundler and other Ruby package management tools.

**Human URL:** [https://guides.rubygems.org/rubygems-org-api-v2/](https://guides.rubygems.org/rubygems-org-api-v2/)


#### Tags:

 - Packages, Ruby, Gems, Versions, Dependencies

#### Properties

- [Documentation](https://guides.rubygems.org/rubygems-org-api-v2/)
- [OpenAPI](openapi/rubygems-api-v2-openapi.yml)

## Common Properties

- [Portal](https://guides.rubygems.org/)
- [Documentation](https://guides.rubygems.org/rubygems-org-api/)
- [Website](https://rubygems.org/)
- [TermsOfService](https://rubygems.org/pages/about)
- [Blog](https://blog.rubygems.org/)
- [Login](https://rubygems.org/sign_in)
- [Support](https://help.rubygems.org/)

## Maintainers

**FN:** API Evangelist

**Email:** info@apievangelist.com
