# GoHighLevel MCP Server

A Model Context Protocol (MCP) server providing access to the complete GoHighLevel API. This server enables AI assistants like Claude to interact with GoHighLevel's services including contacts, opportunities, calendars, workflows, and more.

<a href="https://glama.ai/mcp/servers/@drausal/gohighlevel-mcp">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/@drausal/gohighlevel-mcp/badge" alt="GoHighLevel Server MCP server" />
</a>

## 🚀 Quick Start

```bash
npx @drausal/gohighlevel-mcp
```

Or install globally:

```bash
npm install -g @drausal/gohighlevel-mcp
```

## 🚀 Features

- **Complete API Coverage**: Access to all GoHighLevel API endpoints
- **OAuth2 Support**: Automatic token management and refresh
- **Bearer Token Auth**: Simple API key authentication
- **Type-Safe**: Full TypeScript implementation with Zod validation
- **Stdio Transport**: Works with Claude Desktop and other MCP clients
- **Auto-Generated**: Built from the official OpenAPI specification

## 📋 Prerequisites

- Node.js >= 20.0.0
- A GoHighLevel account with API access
- API credentials (Bearer token or OAuth2 client credentials)

## 🔧 Installation

### Quick Start with npx (Recommended)

No installation needed! Use npx to run the server directly:

```bash
npx @drausal/gohighlevel-mcp
```

### Alternative: Install Globally

For permanent installation:

```bash
npm install -g @drausal/gohighlevel-mcp
```

Then run:
```bash
gohighlevel-mcp
```

### From Source (Development)

For development or customization:

```bash
git clone https://github.com/drausal/gohighlevel-mcp.git
cd gohighlevel-mcp
pnpm install
pnpm run build
pnpm start
```

## 🎯 Usage

### Claude Desktop Configuration

Add this to your Claude Desktop configuration (`~/Library/Application Support/Claude/claude_desktop_config.json` on macOS):

#### Using npx (Recommended):
```json
{
  "mcpServers": {
    "gohighlevel": {
      "command": "npx",
      "args": [
        "@drausal/gohighlevel-mcp"
      ],
      "env": {
        "BEARER_TOKEN_BEARERAUTH": "your_api_key_here",
        "BEARER_TOKEN_BEARER": "your_api_key_here"
      }
    }
  }
}
```

#### Using global installation:
```json
{
  "mcpServers": {
    "gohighlevel": {
      "command": "gohighlevel-mcp",
      "args": [],
      "env": {
        "BEARER_TOKEN_BEARERAUTH": "your_api_key_here",
        "BEARER_TOKEN_BEARER": "your_api_key_here"
      }
    }
  }
}
```

#### Using local installation:
```json
{
  "mcpServers": {
    "gohighlevel": {
      "command": "node",
      "args": [
        "/absolute/path/to/gohighlevel-mcp/build/index.js"
      ],
      "env": {
        "BEARER_TOKEN_BEARERAUTH": "your_api_key_here",
        "BEARER_TOKEN_BEARER": "your_api_key_here"
      }
    }
  }
}
```

### Warp Terminal Configuration

For Warp users, use the npx method:

```json
{
  "mcpServers": {
    "gohighlevel": {
      "command": "npx",
      "args": [
        "@drausal/gohighlevel-mcp"
      ],
      "env": {
        "BEARER_TOKEN_BEARERAUTH": "your_api_key_here",
        "BEARER_TOKEN_BEARER": "your_api_key_here"
      }
    }
  }
}
```

### Running Standalone

```bash
# With npx
npx @drausal/gohighlevel-mcp

# With global installation
gohighlevel-mcp

# From source
pnpm start
```

## 🔐 Authentication

### Bearer Token

The simplest authentication method. Get your API key from GoHighLevel:

1. Log into your GoHighLevel account
2. Go to Settings → API
3. Copy your API key
4. Set `BEARER_TOKEN_BEARERAUTH` in your `.env` file

### OAuth2

For more advanced use cases with automatic token refresh:

1. **Agency Access**: For agency-level operations
   - Set `OAUTH_CLIENT_ID_AGENCY_ACCESS` and `OAUTH_CLIENT_SECRET_AGENCY_ACCESS`

2. **Location Access**: For location-specific operations
   - Set `OAUTH_CLIENT_ID_LOCATION_ACCESS` and `OAUTH_CLIENT_SECRET_LOCATION_ACCESS`

The server automatically:
- Obtains access tokens using client credentials flow
- Caches tokens for their lifetime
- Refreshes tokens when expired

See `docs/oauth2-configuration.md` for detailed OAuth2 configuration.

## 📚 Available Tools

The MCP server exposes all GoHighLevel API endpoints as MCP tools. Major categories include:

### Core Resources
- **Contacts**: Create, update, search contacts
- **Opportunities**: Manage sales pipeline
- **Calendars**: Schedule appointments
- **Workflows**: Trigger and manage automation

### Communication
- **Conversations**: Messaging and chat
- **Emails**: Email campaigns and templates
- **SMS**: Text message campaigns

### Marketing
- **Social Media Posting**: Manage social campaigns
- **Blogs**: Content management
- **Forms**: Lead capture

### Business Management
- **Payments**: Process transactions
- **Invoices**: Billing management
- **Users**: Team and permission management
- **Locations**: Sub-account management

To see all available tools, the MCP client will list them when connecting to the server.

## 🛠️ Development

### Project Structure

```
gohighlevel-mcp/
├── src/
│   └── index.ts           # Main MCP server implementation
├── build/                 # Compiled JavaScript output
├── docs/                  # Documentation
├── .env.example           # Example environment configuration
├── package.json           # Project dependencies
├── tsconfig.json          # TypeScript configuration
└── README.md             # This file
```

### Scripts

- `pnpm run build` - Compile TypeScript to JavaScript
- `pnpm start` - Run the compiled server
- `pnpm run typecheck` - Check TypeScript types without building

### Type Safety

This server uses:
- **Zod** for runtime validation of inputs and outputs
- **TypeScript** for compile-time type checking
- **json-schema-to-zod** for generating validators from OpenAPI schemas

## 📖 Documentation

- [OAuth2 Configuration](docs/oauth2-configuration.md)
- [GoHighLevel API Documentation](https://highlevel.stoplight.io/)
- [Model Context Protocol Specification](https://modelcontextprotocol.io/)

## 🔍 Troubleshooting

### Server Won't Start

1. Check Node.js version:
   ```bash
   node --version  # Should be >= 20.0.0
   ```

2. Verify build succeeded:
   ```bash
   ls -la build/index.js
   ```

3. Check environment variables:
   ```bash
   cat .env
   ```

### Authentication Errors

1. **Bearer Token**: Verify your API key is correct
2. **OAuth2**: Ensure client ID and secret are valid
3. Check token expiration (OAuth2 tokens expire after 1 hour)

### Claude Desktop Integration Issues

1. Verify the absolute path in `claude_desktop_config.json`
2. Restart Claude Desktop after configuration changes
3. Check Claude Desktop logs for error messages

### Common Issues

**"ENOENT" errors**: Usually means the path in config is incorrect
**"Permission denied"**: Run `chmod +x build/index.js`
**"Invalid token"**: Check your API credentials in `.env`

## 🤝 Contributing

This project is auto-generated from the GoHighLevel OpenAPI specification. To update:

1. Download the latest OpenAPI spec from GoHighLevel
2. Run the generator again:
   ```bash
   npx openapi-mcp-generator -i openapi.json -o . -n gohighlevel --force
   ```
3. Reinstall dependencies and rebuild

## 📝 License

This MCP server is generated from GoHighLevel's public API specification. Please refer to GoHighLevel's terms of service for API usage guidelines.

## 🆘 Support

- **GoHighLevel API Issues**: Contact GoHighLevel support
- **MCP Protocol Issues**: See [MCP Documentation](https://modelcontextprotocol.io/)
- **Server Issues**: Check this README and troubleshooting section

## 🎉 Acknowledgments

- Built with [openapi-mcp-generator](https://www.npmjs.com/package/openapi-mcp-generator)
- Uses [Model Context Protocol SDK](https://github.com/modelcontextprotocol/sdk)
- Powered by GoHighLevel's comprehensive API