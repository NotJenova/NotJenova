# cimd/ — OAuth Client ID Metadata Document

`pi-coding-agent.json` is **not** profile content. It is an OAuth
**Client ID Metadata Document** (SEP-991) hosted here for one reason: it needs a
public HTTPS URL that serves `Content-Type: application/json`.

## Why it exists

The `pdf-to-md` MCP server (`https://pdf2md.dudko.dev/mcp`) is protected by the
authorization server `https://auth.dudko.dev`, which advertises:

```
registration_endpoint                  -> (absent)   # no RFC 7591 dynamic client registration
client_id_metadata_document_supported  -> true       # SEP-991 CIMD is the supported path
```

So an MCP client cannot register itself. It must present a **hosted metadata
document as its `client_id`**, and the authorization server fetches that URL to
read the client's metadata.

## Hosting

`raw.githubusercontent.com` serves `.json` as `text/plain`, which the
authorization server rejects:

```
{"error":"invalid_client",
 "message":"the client_id document could not be read: content-type is text/plain, not application/json"}
```

The file is therefore served through the jsDelivr GitHub CDN, which returns
`application/json`:

```
https://cdn.jsdelivr.net/gh/NotJenova/NotJenova@main/cimd/pi-coding-agent.json
```

That exact URL is the `client_id` inside the document, and it must stay in sync
with `client_id` in the JSON. jsDelivr caches `@main` for up to 12 hours; after
editing this file, purge it with:

```bash
curl -s "https://purge.jsdelivr.net/gh/NotJenova/NotJenova@main/cimd/pi-coding-agent.json"
```

## Constraints

The document values must match what the MCP client presents, or the
authorization server rejects the request:

| Field | Must match |
|---|---|
| `client_id` | this file's own public URL |
| `redirect_uris` | `oauth.redirectUri` in `~/.config/mcp/mcp.json` |
| `scope` | the scope the client requests (`usage offline_access`) |
| `token_endpoint_auth_method` | `none` (public client + PKCE S256) |

Upgrading to GitHub Pages would remove the third-party CDN, but Pages is not
enabled on this repository.
