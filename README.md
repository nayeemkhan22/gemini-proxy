# Gemini API Proxy (US-based)

This Vercel serverless function proxies requests to Google Gemini API from a US-based server, bypassing EU geo-restrictions.

## Deploy to Vercel

### Option 1: Deploy via GitHub (Recommended)

1. Create a new GitHub repository
2. Upload these files to the repository
3. Go to https://vercel.com
4. Sign up/Login with GitHub
5. Click "Add New Project"
6. Import your GitHub repository
7. Click "Deploy"
8. Wait ~1 minute for deployment
9. Copy your URL: `https://your-project.vercel.app`

### Option 2: Deploy via Vercel CLI

```bash
npm i -g vercel
vercel login
vercel --prod
```

## Usage in n8n

**URL:**
```
https://your-project.vercel.app/api/gemini?model=gemini-2.0-flash-exp&key=YOUR_GEMINI_API_KEY
```

**Method:** POST

**Headers:**
```
Content-Type: application/json
```

**Body:**
```json
{
  "contents": [
    {
      "role": "user",
      "parts": [
        {
          "text": "Your prompt here"
        },
        {
          "inline_data": {
            "mime_type": "image/jpeg",
            "data": "BASE64_IMAGE_DATA"
          }
        }
      ]
    }
  ],
  "generationConfig": {
    "responseModalities": ["image", "text"]
  }
}
```

## Parameters

| Parameter | Location | Required | Description |
|-----------|----------|----------|-------------|
| `key` | Query string | Yes | Your Gemini API key |
| `model` | Query string | No | Model name (default: gemini-2.0-flash-exp) |

## Why This Works

- Vercel's `regions: ["iad1"]` forces deployment to US-East (Washington DC)
- All requests to Gemini originate from US IP addresses
- Google sees US location → No geo-block

## Troubleshooting

| Error | Solution |
|-------|----------|
| 401 Unauthorized | Check your Gemini API key |
| 400 Bad Request | Check your request body format |
| 500 Internal Error | Check Vercel function logs |
| Still geo-blocked | Verify vercel.json has `"regions": ["iad1"]` |
