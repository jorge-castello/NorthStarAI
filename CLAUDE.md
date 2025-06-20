# Sage AI Assistant - Development Notes

This document captures the key decisions and technical strategy for transforming LibreChat into Sage, a concise and motivating AI assistant focused on helping users achieve their goals.

## Project Vision: Sage

Sage is an AI assistant that:

- **Prompts users routinely** to collect information about their goals and inspirations
- **Generates intelligent action items** (todo lists) to help users achieve their goals
- **Maintains a concise, kind, and motivating tone** - says in one sentence what could be said in many
- **Shows emotional intelligence** and genuine interest in helping users become their best selves

## Implementation Strategy: Agent-First Architecture

### Why Agent

**Agent Benefits:**

1. **Built-in Tool Integration** - Native MCP server support, actions, file handling
2. **System Instructions** - Perfect vehicle for Sage's personality and behavioral rules
3. **User Experience** - Appears as a specialized assistant, not just another model
4. **Extensibility** - Easy to add goal-tracking tools without core modifications
5. **Branding** - Custom avatar, name, description for distinct Sage identity
6. **Fork-Friendly** - All functionality contained in Agent config + MCP server

## Fork Maintenance Strategy

### 1. Modular Architecture

```
/packages/sage-mcp/          # Sage MCP Server
├── server.js               # MCP server implementation
├── tools/                  # Goal tracking tools
│   ├── goalCreate.js
│   ├── taskGenerate.js
│   └── progressCheck.js
├── models/                 # Database models
│   ├── Goal.js
│   └── Task.js
└── utils/                  # Helper functions

/assets/sage/               # Sage assets
└── sage-avatar.png         # Custom avatar
```

### 3. Git Strategy

- **Main branch**: Clean LibreChat fork with no modifications
- **Sage assets**: Only add configuration files and MCP server
- **Upstream sync**: Simply merge - no conflicts possible
- **All Sage logic**: Contained in MCP server and configuration

## Technical Architecture

### MCP Server Architecture

- **Standalone Node.js service** implementing MCP protocol
- **MongoDB integration** for persistent goal/task storage
- **Tool implementations** for goal management operations
- **RESTful API** for direct database operations if needed

### Agent Integration

- **System instructions** define Sage personality and behavior
- **Tool attachment** connects MCP server capabilities to Agent
- **Model selection** optimized for concise, motivating responses
- **No frontend changes** required - uses existing Agent UI

## Development Workflow

### Local Development Setup

1. **LibreChat Backend**: `npm run backend:dev` - Express server with nodemon hot-reload on port 3080
2. **LibreChat Frontend**: `npm run frontend:dev` - React/Vite with HMR on port 3090
3. **Sage MCP Server**: `npm run sage:dev` - MCP server with hot-reload on port 3001
4. **MongoDB**: Docker container with port forwarding on 27017

### Development Environment

- **LibreChat**: Uses existing MongoDB container for user data
- **Sage MCP**: Connects to same MongoDB for goal/task persistence
- **Agent Configuration**: Live in `librechat.yaml` for instant updates
- **Hot Reload**: Both LibreChat and MCP server support live code updates

### Testing Strategy

- **Unit Tests**: Jest for MCP tool functions
- **Integration Tests**: Test Agent + MCP server interactions
- **E2E Tests**: User goal creation flow through Agent interface
- **Manual Testing**: Real conversations with Sage Agent

## Key Design Decisions

1. **Agent vs Custom Provider**: Agent provides better UX and tool integration
2. **MCP for Tools**: Clean separation of concerns, easy to maintain and extend
3. **MongoDB Integration**: Leverage existing LibreChat database infrastructure
4. **Zero Core Modifications**: Maintain upstream compatibility through configuration
5. **Personality in Instructions**: Define Sage's concise, motivating behavior in Agent config
