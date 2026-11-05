# FeatureOS MCP Server

The official [Model Context Protocol](https://modelcontextprotocol.io) server for [FeatureOS](https://featureos.com), the product feedback, roadmap and changelog platform.

Connect it to Claude, Cursor, Gemini CLI or any MCP client, then triage feature requests, merge duplicates, assign owners, tag posts, publish changelogs and update help-center articles in plain language, against your live FeatureOS workspace.

This is a **hosted remote server**. There is nothing to install or run.

| | |
|---|---|
| Endpoint | `https://mcp.featureos.app/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth 2.0 (browser sign-in on first connect) |
| Permissions | Your FeatureOS role. If you cannot do it in the dashboard, the assistant cannot either. |

## Connect

### Claude Code

```bash
claude mcp add --transport http featureos https://mcp.featureos.app/mcp
```

Or add it to your MCP config file:

```json
{
  "mcpServers": {
    "featureos": {
      "type": "http",
      "url": "https://mcp.featureos.app/mcp"
    }
  }
}
```

### Claude Desktop

1. Open **Settings → Connectors** and click **Add custom connector**.
2. Name it `FeatureOS` and paste `https://mcp.featureos.app/mcp`.
3. Click **Add**, then **Connect**, and approve access in the browser.

### Cursor

In Cursor's MCP settings, add a new server with the URL `https://mcp.featureos.app/mcp`, or add the same JSON block shown above to your MCP config file.

### Gemini CLI

```bash
gemini extensions install https://github.com/featureos/featureos-mcp
```

This repo ships a `gemini-extension.json` that points Gemini CLI at the hosted server.

### Other clients

Any client with remote MCP support: add a new remote server, paste `https://mcp.featureos.app/mcp`, and complete the sign-in when the browser opens.

### Verify the connection

Ask: *"What are the top feature requests in my FeatureOS workspace?"* Real posts from your boards mean you are connected.

## Authentication

On first connect your browser opens the FeatureOS sign-in page. Log in to the workspace you want and approve access. No API key or token goes in your config file.

Scopes requested: `posts`, `comments`, `votes`, `tags`, `changelog`, `articles`, `collections`, `customers` (read and write), `roadmap:read`, `webhooks:manage`.

Revoke access any time under **Dashboard → Organization Settings → Advanced**.

**Several workspaces?** MCP clients store one login per URL. Use an org-scoped URL to give each workspace its own connection: `https://mcp.featureos.app/<your-org-slug>/mcp`.

## Tools

| Area | Tool | What it does |
|---|---|---|
| Feature requests | `list_feature_requests` | List feature requests with optional filtering and pagination |
| | `search_feature_requests` | Search feature requests by keyword or meaning |
| | `get_feature_request` | Get full details of a single feature request |
| | `create_feature_request` | Create a new feature request post |
| | `update_feature_request` | Update an existing feature request |
| | `assign_feature_request` | Assign a team member by email |
| | `unassign_feature_request` | Remove the assignee |
| | `merge_posts` | Merge posts into a parent, consolidating votes, comments and subscribers |
| | `unmerge_post` | Restore a merged post as an independent post |
| | `list_merged_posts` | List posts merged into a parent |
| | `list_post_tags` | List tags on a feature request |
| | `add_tag_to_feature_request` | Add a tag to a feature request |
| | `remove_tag_from_feature_request` | Remove a tag from a feature request |
| | `create_attachment_upload_url` | Upload a file to FeatureOS storage, or get a presigned upload URL |
| Comments | `list_comments` | List comments on a feature request |
| | `get_comment` | Get a single comment |
| | `create_comment` | Comment on a feature request |
| | `update_comment` | Replace a comment's text or pin state |
| Votes | `list_upvoters` | List upvoters |
| | `list_downvoters` | List downvoters |
| | `vote` | Upvote or downvote a post |
| Changelogs | `list_changelogs` | List changelogs with status, locale and label filters |
| | `search_changelogs` | Search changelogs by keyword |
| | `get_changelog` | Get a changelog with labels, linked requests and translations |
| | `create_changelog` | Create a changelog |
| | `update_changelog` | Update a changelog |
| | `list_changelog_tags` | List labels on a changelog |
| | `add_tag_to_changelog` | Add a label to a changelog |
| | `remove_tag_from_changelog` | Remove a label from a changelog |
| | `set_changelog_labels` | Set a changelog's labels to an exact list |
| Changelog labels | `list_changelog_labels` | List all changelog labels |
| | `create_changelog_label` | Create a changelog label |
| | `update_changelog_label` | Rename, recolor or re-describe a label |
| | `delete_changelog_label` | Delete a label from every changelog |
| Help center | `list_articles` | List knowledge base articles |
| | `search_articles` | Search articles by keyword |
| | `get_article` | Get an article with authors, tags and collection |
| | `create_article` | Create an article in a collection |
| | `update_article` | Update an article |
| | `delete_article` | Permanently delete an article |
| | `list_collections` | List knowledge base collections |
| | `get_collection` | Get a collection with article counts and privacy settings |
| Workspace | `list_buckets` | List boards |
| | `list_tags` | List all tags |
| | `get_statuses` | List configured post statuses |
| | `get_organization` | Get organization info |
| | `list_customers` | List customers |
| Team | `list_team_members` | List team members |
| | `get_team_member` | Get a team member by id or email |
| | `invite_team_member` | Invite a user by email |
| | `update_team_member` | Change a team member's role |

IDs are opaque: the assistant gets them from a prior tool call (for example `list_buckets` before `list_feature_requests`). List tools return one page at a time.

## Example prompts

- "List all feature requests with status Under Review on the Product board, sorted by votes."
- "Show me posts submitted in the last 7 days that are still unapproved."
- "Get the full details of feature request 1234, then show the latest 10 comments."
- "Find duplicates of 'dark mode' and merge them into the most-voted post."
- "Assign post 1234 to priya@example.com and tag it ux."
- "Draft a changelog for everything we shipped this week and label it Improvement."
- "Search the help center for SSO and update the setup article with the new steps."

## Learn more

- [How to manage feedback via Claude Code](https://featureos.com/blog/how-to-manage-feedback-via-claude-code)
- [AI feedback triage workflows](https://featureos.com/blog/ai-feedback-triage-workflows)
- [FeatureOS MCP integration guide](https://help.featureos.app/en/articles/featureos-mcp-integration)
- [FeatureOS Developer API](https://featureos.com/developer-api)
- Pricing: see [featureos.com/pricing](https://featureos.com/pricing)

## License

Apache-2.0. See [LICENSE](LICENSE).
