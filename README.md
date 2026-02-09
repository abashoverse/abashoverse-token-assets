# Abashoverse Token Assets

Token and NFT metadata and icons for the Abashoverse ecosystem on Abstract chain.

## Usage

### Fetching Token List

```typescript
const ASSETS_URL = 'https://cdn.jsdelivr.net/gh/abashoverse/abashoverse-token-assets@main';

// Fetch token list
const response = await fetch(`${ASSETS_URL}/tokens.json`);
const tokenList = await response.json();
```

### Fetching NFT List

```typescript
const ASSETS_URL = 'https://cdn.jsdelivr.net/gh/abashoverse/abashoverse-token-assets@main';

// Fetch NFT collection list
const response = await fetch(`${ASSETS_URL}/nft.json`);
const nftList = await response.json();
```

### Fetching Icons

Each token/NFT has a `logoURI` field specifying the path to its icon:

```typescript
const ASSETS_URL = 'https://cdn.jsdelivr.net/gh/abashoverse/abashoverse-token-assets@main';

// Using logoURI from metadata (recommended)
function getIconUrl(entry: { logoURI: string }): string {
  return `${ASSETS_URL}/${entry.logoURI}`;
}

// Fallback for unknown tokens (assumes .svg)
function getTokenIconUrlFallback(chainId: number, address: string): string {
  return `${ASSETS_URL}/icons/tokens/${chainId}/${address.toLowerCase()}.svg`;
}
```

Supported formats: `.svg` (preferred), `.png`, `.webp`

## Directory Structure

```
abashoverse-token-assets/
├── tokens.json          # ERC-20 token metadata and collections
├── schema.json          # JSON schema for token validation
├── nft.json             # NFT collection metadata
├── nft-schema.json      # JSON schema for NFT validation
├── icons/
│   ├── tokens/
│   │   ├── 2741/        # Abstract mainnet
│   │   │   ├── 0x000...000.svg
│   │   │   └── 0xabc...def.png
│   │   └── 11124/       # Abstract testnet
│   │       └── 0xb14...abe.svg
│   └── nfts/
│       ├── 2741/        # Abstract mainnet
│       └── 11124/       # Abstract testnet
```

## Token List Format

```json
{
  "version": "1.0.0",
  "name": "Abashoverse Token List",
  "tokens": [
    {
      "chainId": 2741,
      "address": "0x0000000000000000000000000000000000000000",
      "symbol": "ETH",
      "name": "Ether",
      "decimals": 18,
      "verified": true,
      "tags": ["native"],
      "logoURI": "icons/tokens/2741/0x0000000000000000000000000000000000000000.svg"
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

## NFT List Format

```json
{
  "version": "1.0.0",
  "name": "Abashoverse NFT List",
  "nfts": [
    {
      "chainId": 2741,
      "address": "0xabcdef1234567890abcdef1234567890abcdef12",
      "name": "Example Collection",
      "symbol": "EXMPL",
      "type": "ERC721",
      "verified": true,
      "tags": ["pfp"],
      "logoURI": "icons/nfts/2741/0xabcdef1234567890abcdef1234567890abcdef12.png"
    }
  ]
}
```

## Adding a New Token

1. **Fork this repository**

2. **Add token metadata** to `tokens.json`:
   ```json
   {
     "chainId": 2741,
     "address": "0xyourtokenaddress",
     "symbol": "TKN",
     "name": "Your Token",
     "decimals": 18,
     "verified": false,
     "tags": ["community"],
     "logoURI": "icons/tokens/2741/0xyourtokenaddress.svg"
   }
   ```

3. **Add token icon** to `icons/tokens/{chainId}/`:
   - Filename: `{lowercase_address}.svg` or `.png` or `.webp`
   - Dimensions: 64x64 px for PNG/WebP, `viewBox="0 0 64 64"` for SVG
   - Shape: Square (no rounded corners)
   - Background: Solid brand color (no transparency)
   - Must match the `logoURI` path in tokens.json

4. **Submit a Pull Request**

## Adding a New NFT Collection

1. **Fork this repository**

2. **Add collection metadata** to `nft.json`:
   ```json
   {
     "chainId": 2741,
     "address": "0xyourcollectionaddress",
     "name": "Your Collection",
     "symbol": "COL",
     "type": "ERC721",
     "verified": false,
     "tags": ["pfp"],
     "logoURI": "icons/nfts/2741/0xyourcollectionaddress.png"
   }
   ```

3. **Add collection icon** to `icons/nfts/{chainId}/`:
   - Same guidelines as token icons (see below)

4. **Submit a Pull Request**

## Icon Guidelines

- **Dimensions**: 64x64 pixels (required for PNG/WebP, viewBox for SVG)
- **Format**: SVG preferred (scales better), PNG and WebP also accepted
- **Shape**: Square (no rounded corners)
- **Background**: Solid brand color (no transparency)
- **Naming**: `{lowercase_address}.{ext}` (e.g., `0xb1488f753521c58f2ddfaf99b503c48aa14dfabe.svg`)
- **Location**: `icons/tokens/{chainId}/` for tokens, `icons/nfts/{chainId}/` for NFT collections

| Format | Dimensions | Notes |
|--------|------------|-------|
| SVG | `viewBox="0 0 64 64"` | Preferred - scales perfectly |
| PNG | 64x64 px | Use for complex logos, keep under 10KB |
| WebP | 64x64 px | Good compression, modern browsers only |

### SVG Template

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64">
  <rect width="64" height="64" fill="#YOUR_BRAND_COLOR"/>
  <!-- Your logo content here -->
</svg>
```

## Verification

To get your token or NFT collection verified (checkmark):

1. Contract must be deployed on Abstract mainnet or testnet
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
https://purge.jsdelivr.net/gh/abashoverse/abashoverse-token-assets@main/nft.json
```

## License

MIT
