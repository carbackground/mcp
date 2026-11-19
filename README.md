# carbackground.ai MCP server

Turn ordinary car photos into dealership-quality listing photos from any AI assistant.

[carbackground.ai](https://carbackground.ai) replaces the background of a car photo with a studio, showroom or outdoor scene. The car is kept as photographed (same car, wheels, badges and viewing angle) and gets shadows, lighting and reflections that match the new scene. It is built for car dealers, marketplaces and anyone who publishes vehicle listings.

This repository documents the hosted MCP server. There is nothing to install or run.

## Connect

| | |
| --- | --- |
| Server URL | `https://api.carbackground.ai/mcp` |
| Transport | Streamable HTTP |
| Sign-in | OAuth, with your carbackground.ai account |
| Registry name | `ai.carbackground/carbackground` |

Add the server URL as a custom connector or remote MCP server in your client. The first time it is used, a browser window opens to sign in to carbackground.ai and approve the connection. New accounts can be created on that page and start with free trial credits.

**Claude:** Settings → Connectors → Add custom connector, then paste the server URL.

**Claude Code:**

```bash
claude mcp add --transport http carbackground https://api.carbackground.ai/mcp
```

**VS Code** (`.vscode/mcp.json`):

```json
{
  "servers": {
    "carbackground": {
      "type": "http",
      "url": "https://api.carbackground.ai/mcp"
    }
  }
}
```

**Cursor and other clients** (`mcp.json`):

```json
{
  "mcpServers": {
    "carbackground": {
      "url": "https://api.carbackground.ai/mcp"
    }
  }
}
```

Connected apps are listed in the carbackground.ai app under Settings → MCP, where each one can be disconnected.

## What you can ask for

- "Put this car in a white studio and blur the plate: https://example.com/car.jpg"
- "Use the same showroom background for all twelve photos of this listing."
- "Make a custom background that looks like our showroom: dark marble floor, warm light."
- "Reframe these photos to 4:3 for the marketplace and add our logo top-right."
- "How many credits do I have left?"

## Tools

| Tool | What it does |
| --- | --- |
| `list_backgrounds` | List the available scenes, built-in and custom, with preview images. |
| `generate_background` | Replace the background of one car photo. Can also hide the plate, add a dealer logo, set an aspect ratio, centre the car or show it washed. |
| `hide_license_plates` | Hide the plates in a photo without changing anything else. |
| `create_background` | Create a custom scene from a text description and/or reference photos. Returns a preview. |
| `refine_background` | Change a custom scene ("darker floor", "warmer light") and get a new preview. |
| `delete_background` | Delete a custom scene. |
| `list_logos` | List the workspace's dealer logos and plate logos. |
| `upload_logo` | Upload a logo. |
| `delete_logo` | Delete a logo. |
| `get_credits` | Show the credit balance. |
| `list_plans` | List the subscription plans. |
| `create_checkout` | Get a payment link for a first subscription. |
| `change_plan` | Preview or confirm a change of plan. |

Photos are passed as public image URLs (JPEG, PNG or WebP). Results come back as a signed image URL.

## Pricing

One credit per generated photo and per custom background preview. Failed generations are not charged. Hiding license plates is included with a subscription. Plans are listed at [carbackground.ai](https://carbackground.ai) and through `list_plans`.

## REST API

The same features are available as a REST API with API keys:

- OpenAPI: https://api.carbackground.ai/openapi.yaml
- Summary for agents: https://api.carbackground.ai/llms.txt

## Support

- Email: hello@carbackground.ai
- Website: https://carbackground.ai
- Problems with this server: open an issue in this repository.
