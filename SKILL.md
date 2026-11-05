---
name: featureos
description: Use the FeatureOS MCP server to read and manage product feedback, roadmaps, changelogs and help-center articles. Use when the user mentions FeatureOS, feature requests, feedback boards, upvotes, duplicate posts, changelogs or release notes, or knowledge base articles in their FeatureOS workspace.
---

# FeatureOS

The FeatureOS MCP server (`https://mcp.featureos.app/mcp`) acts on the user's live workspace with their own role and OAuth session. Writes are real: they change boards, notify subscribers and publish content.

## Rules

- Never invent IDs. Get them from a prior call:
  - board `bucket_id` from `list_buckets`
  - tag ids from `list_tags`, changelog `label_ids` from `list_changelog_labels`
  - `collection_id` from `list_collections`
  - post, changelog and article ids from the matching `list_*` or `search_*` tool
- Assign teammates by email (`list_team_members` returns it), not by id.
- Prefer `search_*` over listing whole boards. When listing, pass `bucket_id` and keep `per_page` small, then page.
- List tools return one page. If the result says more pages exist, call again with `page` incremented and every other argument unchanged.
- Array filters must be real JSON arrays (`[1, 2]`), not strings.
- `update_comment` replaces the whole comment body. Send the full new text.
- Confirm with the user before destructive or outward-facing actions: `merge_posts`, `delete_article`, `delete_changelog_label`, `invite_team_member`, `update_team_member`, and publishing a changelog.

## Common flows

- Triage: `list_buckets` → `list_feature_requests(bucket_id=...)` → `get_feature_request` → `create_comment` or `update_feature_request`
- Assign: `list_team_members` → `assign_feature_request(email=...)`
- Dedupe: `search_feature_requests` → `get_feature_request` on each → `merge_posts` into the most-voted parent
- Release notes: `search_feature_requests` for shipped posts → `list_changelog_labels` → `create_changelog(label_ids=...)`
- Help center: `search_articles` → `get_article` → `update_article`

## If it fails

- Empty results usually mean the wrong workspace was picked at sign-in. Reconnect and choose the right one.
- "Unauthorized" means the session expired or was revoked. Reconnect.
