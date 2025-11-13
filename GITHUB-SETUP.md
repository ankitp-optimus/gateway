# AGF POC - GitHub Configuration Repository

This repository contains configuration files for the AGF POC Spring Boot application to demonstrate Azure Config Server integration.

## Repository Structure

```
agf-config-repo/
├── agf-poc-app.yml          # Default configuration
├── agf-poc-app-dev.yml      # Development environment
├── agf-poc-app-prod.yml     # Production environment
└── README.md                # This file
```

## Configuration Files

### Default Configuration (`agf-poc-app.yml`)
- Used when no specific profile is active
- Contains base configuration with emoji indicators for Config Server

### Development Configuration (`agf-poc-app-dev.yml`)
- Activated with profile: `dev`
- Enhanced logging enabled
- Contains DEV-specific message indicators

### Production Configuration (`agf-poc-app-prod.yml`)
- Activated with profile: `prod`  
- Reduced logging for performance
- Contains PROD-specific message indicators

## Azure Config Server Setup

### 1. Upload to GitHub
1. Create new GitHub repository: `agf-config-repo`
2. Upload these files to the repository
3. Note the repository URL: `https://github.com/[username]/agf-config-repo`

### 2. Configure Azure Config Server
1. Go to Azure Portal
2. Navigate to your Container App Environment
3. Go to "Services" tab
4. Click "Add Service" → "Config Server"
5. Configure:
   - **Repository URI**: `https://github.com/[username]/agf-config-repo`
   - **Default Branch**: `main`
   - **Folder Path**: `/` (root folder)

### 3. Verification

#### Current Local Values vs Config Server Values

| Endpoint | Local Value | Config Server Value |
|----------|-------------|-------------------|
| `GET /` | "app is running" | "🌟 App is running - POWERED BY CONFIG SERVER! 🌟" |
| `GET /greeting` | "hello from optimus to agf" | "🚀 Hello from Optimus to AGF - FETCHED FROM GITHUB CONFIG SERVER! 🚀" |
| `GET /health` | "AGF POC Application is running!" | "✅ AGF POC Application is running! - Configuration from GitHub Repository ✅" |

#### Test Commands

```bash
# Test current app (should show local values)
curl https://[your-app-url]/
curl https://[your-app-url]/greeting  
curl https://[your-app-url]/health
curl https://[your-app-url]/config

# After Config Server setup (should show emoji values)
curl https://[your-app-url]/
curl https://[your-app-url]/greeting
curl https://[your-app-url]/health
curl https://[your-app-url]/config

# Test with different profiles
curl https://[your-app-url]/config?spring.profiles.active=dev
curl https://[your-app-url]/config?spring.profiles.active=prod
```

## Expected Results

### Before Config Server (Local Config)
```
GET / → "app is running"
GET /config → environment.name = "local"
```

### After Config Server (GitHub Config)
```
GET / → "🌟 App is running - POWERED BY CONFIG SERVER! 🌟"
GET /config → environment.name = "config-server"
```

### With DEV Profile
```
GET / → "🔧 DEV: App running - Config Server Development Mode 🔧"
```

### With PROD Profile
```
GET / → "🏭 PROD: App running - Config Server Production Mode 🏭"
```

## Notes

- No code changes required in the Spring Boot application
- Configuration changes take effect immediately
- Use `/actuator/refresh` endpoint to reload config without restart
- Emoji indicators make it easy to identify config source