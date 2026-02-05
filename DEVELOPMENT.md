# Chatwoot Development Guide

This guide provides instructions for developers working on Chatwoot in a development environment.

> **🇧🇷 Para desenvolvedores brasileiros**: Veja a seção [Habilitando Recursos Premium](#habilitando-recursos-premium) para aprender como acessar a função Captain no ambiente de desenvolvimento.

## Table of Contents

- [Setup](#setup)
- [Running the Application](#running-the-application)
- [Testing](#testing)
- [Code Quality](#code-quality)
- [Enabling Premium Features](#enabling-premium-features)

## Setup

### Prerequisites

- Ruby (version specified in `.ruby-version`)
- Node.js and pnpm
- PostgreSQL
- Redis

### Installation

```bash
# Install Ruby dependencies
bundle install

# Install JavaScript dependencies
pnpm install
```

## Running the Application

### Development Server

```bash
# Start all services (Rails, Webpack, etc.)
pnpm dev

# Or using overmind
overmind start -f ./Procfile.dev
```

## Testing

### Ruby Tests

```bash
# Run all specs
bundle exec rspec

# Run a specific spec file
bundle exec rspec spec/path/to/file_spec.rb

# Run a specific test
bundle exec rspec spec/path/to/file_spec.rb:LINE_NUMBER
```

### JavaScript Tests

```bash
# Run all tests
pnpm test

# Watch mode
pnpm test:watch
```

## Code Quality

### Linting

#### JavaScript/Vue

```bash
# Check for issues
pnpm eslint

# Fix issues automatically
pnpm eslint:fix
```

#### Ruby

```bash
# Check for issues
bundle exec rubocop

# Fix issues automatically
bundle exec rubocop -a
```

## Enabling Premium Features

Chatwoot offers premium features like Captain (AI Agent), SLA, Audit Logs, and Custom Roles. By default, these features are disabled in development mode. To enable them, you need to switch your development environment to the **Enterprise** or **Cloud** variant.

### Available Variants

1. **Community** (default) - Free version with basic features
2. **Enterprise** - Self-hosted with premium features enabled
3. **Cloud** - Cloud deployment with premium features enabled

### Switching Variants

#### Interactive Method

Use the interactive variant toggle task:

```bash
bundle exec rails chatwoot:dev:toggle_variant
```

This will display a menu where you can select:
- `1` - Community (free features only)
- `2` - Enterprise (enables premium features)
- `3` - Cloud (enables premium features)

#### Manual Method

You can also manually update the configuration in your Rails console:

```ruby
# Open Rails console
bundle exec rails console

# Switch to Enterprise variant (enables premium features)
InstallationConfig.find_or_create_by(name: 'DEPLOYMENT_ENV').update(value: 'self-hosted')
InstallationConfig.find_or_create_by(name: 'INSTALLATION_PRICING_PLAN').update(value: 'enterprise')

# Or switch to Cloud variant
InstallationConfig.find_or_create_by(name: 'DEPLOYMENT_ENV').update(value: 'cloud')
InstallationConfig.find_or_create_by(name: 'INSTALLATION_PRICING_PLAN').update(value: 'enterprise')

# Clear cache for changes to take effect
GlobalConfig.clear_cache
```

### Enabling Captain (AI Agent)

After switching to Enterprise or Cloud variant, Captain features will be available. However, you also need to configure the OpenAI API credentials:

1. Switch to Enterprise or Cloud variant (as described above)

2. Configure OpenAI credentials in your `.env` file or through the SuperAdmin dashboard:

```bash
# Add to .env file
CAPTAIN_OPEN_AI_API_KEY=your_openai_api_key_here
CAPTAIN_OPEN_AI_MODEL=gpt-4-turbo-preview  # Optional, defaults to gpt-4.1-mini
CAPTAIN_OPEN_AI_ENDPOINT=https://api.openai.com/  # Optional
CAPTAIN_EMBEDDING_MODEL=text-embedding-3-small  # Optional
```

3. Restart your development server

4. Captain features will now be available in the UI

### Premium Features List

The following features require Enterprise or Cloud variant:

- **Captain** (`captain_integration`) - AI Agent for automated support
- **Captain V2** (`captain_integration_v2`) - Enhanced AI features
- **Disable Branding** (`disable_branding`) - Remove Chatwoot branding
- **Audit Logs** (`audit_logs`) - Track user actions and changes
- **SLA** (`sla`) - Service Level Agreement management
- **Custom Roles** (`custom_roles`) - Define custom user roles and permissions
- **SAML** (`saml`) - SAML SSO authentication
- **Conversation Required Attributes** (`conversation_required_attributes`) - Enforce required fields

### Verifying Feature Status

To check which variant you're currently running:

```bash
bundle exec rails chatwoot:dev:show_variant
```

This will display:
- Current variant (Community, Enterprise, or Cloud)
- Deployment environment
- Pricing plan

## Additional Resources

- [Official Documentation](https://www.chatwoot.com/help-center)
- [Contributing Guide](https://www.chatwoot.com/docs/contributing-guide)
- [Captain Documentation](https://chwt.app/captain-docs)
