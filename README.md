# Abashoverse Token Assets

Token metadata and icons for the Abashoverse ecosystem on Abstract chain.

## Usage

### Fetching Token List

```typescript
const ASSETS_URL = 'https://cdn.jsdelivr.net/gh/abashoverse/abashoverse-token-assets@main';

// Fetch token list
const response = await fetch(`${ASSETS_URL}/tokens.json`);
const tokenList = await response.json();
```

### Fetching Token Icons

Each token has a `logoURI` field specifying the path to its icon:

```typescript
const ASSETS_URL = 'https://cdn.jsdelivr.net/gh/abashoverse/abashoverse-token-assets@main';

// (recommended) Using logoURI from token metadata 
function getTokenIconUrl(token: Token): string {
  return `${ASSETS_URL}/${token.logoURI}`;
}

// Fallback for unknown tokens (assumes .svg)
function getTokenIconUrlFallback(address: string): string {
  return `${ASSETS_URL}/icons/${address.toLowerCase()}.svg`;
}
```

Supported formats: `.svg` (preferred), `.png`, `.webp`

## Directory Structure

```
abashoverse-token-assets/
├── tokens.json          # Token metadata and collections
├── schema.json          # JSON schema for validation
├── icons/               # Token icons (SVG preferred, PNG accepted)
│   ├── 0x0000...0000.svg
│   └── 0xb148...abe.svg
```

## Token List Format

```json
{
  "version": "1.0.0",
  "name": "Abashoverse Token List",
  "tokens": [
    {
      "chainId": 11124,
      "address": "0x0000000000000000000000000000000000000000",
      "symbol": "ETH",
      "name": "Ether",
      "decimals": 18,
      "verified": true,
      "tags": ["native"],
      "logoURI": "icons/0x0000000000000000000000000000000000000000.svg"
    }
  ],
  "collections": [
    {
      "id": "abashoverse-verified",
      "name": "Abashoverse Verified",
      "tokens": ["0x0000..."]
    }
  ]
}
```

## Adding a New Token

1. **Fork this repository**

2. **Add token metadata** to `tokens.json`:
   ```json
   {
     "chainId": 11124,
     "address": "0xyourtokenaddress",
     "symbol": "TKN",
     "name": "Your Token",
     "decimals": 18,
     "verified": false,
     "tags": ["community"],
     "logoURI": "icons/0xyourtokenaddress.svg"
   }
   ```

3. **Add token icon** to `icons/`:
   - Filename: `{lowercase_address}.svg` or `.png` or `.webp`
   - Dimensions: 64x64 px for PNG/WebP, `viewBox="0 0 64 64"` for SVG
   - Shape: Square with rounded corners (12px radius recommended)
   - Background: Solid brand color (no transparency)
   - Must match the `logoURI` path in tokens.json

4. **Submit a Pull Request**

## Icon Guidelines

- **Dimensions**: 64x64 pixels (required for PNG/WebP, viewBox for SVG)
- **Format**: SVG preferred (scales better), PNG and WebP also accepted
- **Shape**: Square with rounded corners (12px radius)
- **Background**: Solid brand color (no transparency)
- **Naming**: `{lowercase_address}.{ext}` (e.g., `0xb1488f753521c58f2ddfaf99b503c48aa14dfabe.svg`)

| Format | Dimensions | Shape | Notes |
|--------|------------|-------|-------|
| SVG | `viewBox="0 0 64 64"` | `<rect rx="12"/>` | Preferred - scales perfectly |
| PNG | 64x64 px | 12px corner radius | Use for complex logos, keep under 10KB |
| WebP | 64x64 px | 12px corner radius | Good compression, modern browsers only |

### SVG Template

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64">
  <rect width="64" height="64" rx="12" fill="#YOUR_BRAND_COLOR"/>
  <!-- Your logo content here -->
</svg>
```

## Verification

To get your token verified (checkmark):

1. Token must be deployed on Abstract mainnet or testnet
2. Contract must be verified on block explorer
3. Project must have:
   - Active website
   - Public team or established reputation
   - No history of malicious activity

Submit a verification request by opening an issue.

## Collections

Collections group tokens by category:

| Collection ID | Description |
|--------------|-------------|
| `abashoverse-verified` | Tokens verified by the Abashoverse team |
| `stablecoins` | USD-pegged stablecoins |
| `defi` | DeFi protocol tokens |

## CDN Caching

We use jsDelivr CDN which caches files. After pushing changes:

- **Instant**: Use `@latest` (not recommended for production)
- **~24 hours**: Use `@main` (recommended)
- **Permanent**: Use specific commit hash `@abc1234`

To purge cache immediately:
```
https://purge.jsdelivr.net/gh/abashoverse/abashoverse-token-assets@main/tokens.json
```

## License

MIT
