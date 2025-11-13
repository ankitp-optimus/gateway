# Config Repository for AGF POC

This directory contains configuration files that will be stored in a GitHub repository and consumed by Azure Config Server.

## File Structure

- `agf-poc-app.yml` - Default configuration for the application
- `agf-poc-app-dev.yml` - Development environment specific configuration  
- `agf-poc-app-prod.yml` - Production environment specific configuration

## Usage

1. Push these files to a GitHub repository
2. Configure Azure Config Server to point to this repository
3. The Spring Boot application will fetch configuration based on:
   - Application name: `agf-poc-app`
   - Active profile: `dev`, `prod`, or default

## Configuration Precedence

Azure Config Server will merge configurations in this order (highest priority first):
1. `agf-poc-app-{profile}.yml` (profile-specific)
2. `agf-poc-app.yml` (default)
3. Local `application.yml` (fallback)

## Testing Configuration Changes

After updating configuration in GitHub:
1. POST to `/actuator/refresh` to reload configuration
2. Or restart the application
3. Check `/config` endpoint to verify new values