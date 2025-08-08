# MCP servers

For Gemini

```json
"mcpServers": {
	"github": {
		"command": "docker",
		"args": [
			"run",
			"-i",
			"--rm",
			"-e",
			"GITHUB_PERSONAL_ACCESS_TOKEN",
			"docker-ghcr.artifactory.xero-support.com/github/github-mcp-server"
		],
		"env": {
			"GITHUB_PERSONAL_ACCESS_TOKEN": ""
		}
	},
	"atlassian-remote": {
		"timeout": 60,
		"type": "sse",
		"url": "https://mcp.atlassian.com/v1/sse",
		"disabled": false
	},
	"sequential-thinking": {
		"command": "npx",
		"args": [
			"-y",
			"@modelcontextprotocol/server-sequential-thinking"
		]
	},
	"filesystem": {
		"command": "npx",
		"args": [
			"-y",
			"@modelcontextprotocol/server-filesystem",
			"/Users/cheng.zou/Desktop/Projects/Working"
		]
	},
	"github": {
		"type": "http",
		"url": "https://api.githubcopilot.com/mcp/",
		"gallery": true
	},
	"xui-mcp-server": {
    "command": "docker",
    "args": [
      "run",
      "-i",
      "--rm",
      "xui-mcp-server"
    ],
    "alwaysAllow": [
      "getComponentDoc",
      "getMultipleComponentDocs",
      "getComponentProperties",
      "listComponents"
    ]
  },
	"memory": {
		"command": "docker",
		"args": ["run", "-i", "-v", "claude-memory:/app/dist", "--rm", "mcp/memory"]
	}
}
```