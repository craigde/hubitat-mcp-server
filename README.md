# Hubitat Maker API - OpenAPI Specification

An OpenAPI 3.0 specification for the [Hubitat Maker API](https://docs2.hubitat.com/en/apps/maker-api), designed for easy integration with Workato MCP servers, API gateways, and other tools that support OpenAPI/Swagger specifications.

## Overview

The Hubitat Maker API provides a simple HTTP interface to control and monitor smart home devices connected to your Hubitat Elevation hub. This OpenAPI specification documents all available endpoints, making it easy to:

- Import into **Workato** to create MCP servers for AI agents (Claude, ChatGPT, Cursor)
- Generate API clients in any language
- Create interactive API documentation
- Build integrations with other platforms

## Features

This specification covers all Hubitat Maker API functionality:

| Category | Endpoints | Description |
|----------|-----------|-------------|
| **Devices** | `/devices`, `/devices/all`, `/devices/{id}` | List and query device information |
| **Device Commands** | `/devices/{id}/{command}` | Send commands like on, off, setLevel, setColor |
| **Device Events** | `/devices/{id}/events` | Retrieve device event history |
| **Hub Variables** | `/hubvariables` | Read and write hub variables |
| **Modes** | `/modes` | View and change hub modes (Home, Away, Night) |
| **HSM** | `/hsm` | Control Hubitat Safety Monitor arm states |
| **Events** | `/postURL` | Configure webhook for real-time event streaming |

## Quick Start

### Prerequisites

1. A Hubitat Elevation hub
2. Maker API app installed and configured on your hub
3. Access token from your Maker API instance
4. A Workato account (free Developer Sandbox available)

### Getting a Free Workato Developer Sandbox Account

Workato offers a free Developer Sandbox that provides full access to their platform, including MCP server capabilities. Here's how to get started:

1. **Sign up at [workato.com/sandbox](https://www.workato.com/sandbox)**
   - No credit card required
   - Instant access after signup

2. **What's included:**
   - Full access to Workato ONE platform
   - 100,000 free usage events (approximately $1,000 value)
   - 1,200+ pre-built connectors
   - MCP server creation and hosting
   - API Platform access
   - AI-native capabilities (AIRO copilots, intelligent document processing)
   - Enterprise-grade security (SAML 2.0, encryption, audit logs)

3. **Usage limits:**
   - No time limit — access continues until you reach your usage limit
   - When limit is reached, recipes pause but you retain access
   - Option to upgrade plans if needed

4. **Supported regions:**
   - US, EU, AU, and SG data centers
   - MCP servers hosted in US, EU, and APAC regions

> 💡 **Tip:** The Developer Sandbox is a permanent developer environment, not a time-limited trial. It's designed for experimentation and building real integrations.

### Getting Your Maker API Credentials

1. Log into your Hubitat hub's admin interface
2. Go to **Apps** → **Add Built-In App** → **Maker API**
3. Select the devices you want to expose
4. Note your **App ID** and **Access Token** from the generated URLs

Your API URL format will be:
```
http://[HUB_IP]/apps/api/[APP_ID]/[endpoint]?access_token=[ACCESS_TOKEN]
```

## Usage with Workato MCP

### Creating an MCP Server

1. Log into your Workato account
2. Navigate to **Platform** → **API Platform** → **API Collections**
3. Click **Create new API collection**
4. Select **Import OpenAPI Specification**
5. Upload `hubitat-maker-api.yaml`
6. Select the endpoints you want to expose
7. Configure authentication (use Query Parameter auth with `access_token`)
8. Save and activate your collection

### Creating the MCP Server

1. Go to **AI Hub** → **MCP Servers**
2. Click **Create MCP Server**
3. Select your Hubitat API collection
4. Configure access settings
5. Copy your MCP URL for use with Claude, ChatGPT, or Cursor

### Example: Using with Claude

Once your MCP server is configured, you can use natural language commands like:

- *"Turn on the living room lights"*
- *"Set the bedroom thermostat to 72 degrees"*
- *"What's the status of all my devices?"*
- *"Lock all the doors"*
- *"Set the house to Away mode"*

## API Endpoints Reference

### Devices

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/devices` | List all authorized devices (basic info) |
| GET | `/devices/all` | List all devices with full details |
| GET | `/devices/{deviceId}` | Get specific device details |
| GET | `/devices/{deviceId}/events` | Get device event history |
| GET | `/devices/{deviceId}/commands` | List available commands for device |
| GET | `/devices/{deviceId}/attribute/{name}` | Get specific attribute value |

### Device Commands

| Method | Endpoint | Example |
|--------|----------|---------|
| GET | `/devices/{id}/{command}` | `/devices/1/on` |
| GET | `/devices/{id}/{command}/{value}` | `/devices/1/setLevel/50` |
| GET | `/devices/{id}/{command}/{v1}/{v2}` | `/devices/1/setLevel/50/2` |

**Common Commands:**
- `on` / `off` - Switch control
- `lock` / `unlock` - Lock control
- `open` / `close` - Door/valve control
- `setLevel/{0-100}` - Dimmer level
- `setColorTemperature/{2700-6500}` - Color temperature in Kelvin
- `setColor/{JSON}` - Set color using HSB or hex values
- `setThermostatSetpoint/{temp}` - Thermostat control
- `refresh` - Refresh device state

### Hub Variables

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/hubvariables` | List all hub variables |
| GET | `/hubvariables/{name}` | Get variable value |
| GET | `/hubvariables/{name}/{value}` | Set variable value |

### Modes & HSM

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/modes` | List all modes and current mode |
| GET | `/modes/{modeId}` | Change to specified mode |
| GET | `/hsm` | Get HSM status |
| GET | `/hsm/{armState}` | Set HSM arm state |

**HSM Arm States:** `armAway`, `armHome`, `armNight`, `disarm`, `disarmAll`, `cancelAlerts`

## Configuration

### Server URLs

The specification includes two server configurations:

**Local Access:**
```yaml
url: http://{hub_ip}/apps/api/{app_id}
```

**Cloud Access:**
```yaml
url: https://cloud.hubitat.com/api/{cloud_id}/apps/{app_id}
```

Update the server variables with your specific values before importing.

### Authentication

All endpoints require the `access_token` query parameter. In the OpenAPI spec, this is defined as:

```yaml
securitySchemes:
  accessToken:
    type: apiKey
    in: query
    name: access_token
```

> ⚠️ **Security Note:** Your access token is like a password. Anyone with this token can control your devices. Never share it publicly or commit it to version control.

## File Structure

```
├── README.md
├── hubitat-maker-api.yaml    # OpenAPI 3.0 specification
└── LICENSE
```

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests for:

- Bug fixes in the specification
- Additional endpoint documentation
- Example integrations
- Improvements to descriptions

## Resources

### Hubitat
- [Hubitat Maker API Documentation](https://docs2.hubitat.com/en/apps/maker-api)
- [Hubitat Community Forums](https://community.hubitat.com/)

### Workato
- [Workato Developer Sandbox Signup](https://www.workato.com/sandbox)
- [Workato MCP Documentation](https://docs.workato.com/mcp.html)
- [Workato API Collections Guide](https://docs.workato.com/api-mgmt/api-collections.html)
- [Getting Started with MCP Servers](https://docs.workato.com/en/mcp/getting-started.html)
- [Remote MCP Server Configuration](https://docs.workato.com/en/mcp/remote-mcp-servers.html)

### OpenAPI
- [OpenAPI 3.0 Specification](https://spec.openapis.org/oas/v3.0.3)

## License

MIT License - See [LICENSE](LICENSE) for details.

## Disclaimer

This project is not affiliated with or endorsed by Hubitat, Inc. Hubitat and Hubitat Elevation are trademarks of Hubitat, Inc.
