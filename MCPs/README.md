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
	}
}
```