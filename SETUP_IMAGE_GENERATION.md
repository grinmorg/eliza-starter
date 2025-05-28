# Setting Up Image Generation for Twitter

## Required Environment Variables

Add these to your `.env` file to enable actual image generation:

### For Together AI (Recommended)
```env
# Together AI API Key for image generation
TOGETHER_API_KEY=your_together_api_key_here

# Image model (already configured in character)
# Model: black-forest-labs/FLUX.1-schnell-Free
```

### Alternative: OpenAI
```env
# If using OpenAI for images instead
OPENAI_API_KEY=your_openai_api_key_here
```

### Alternative: Replicate
```env
# If using Replicate for images
REPLICATE_API_TOKEN=your_replicate_token_here
```

## Getting API Keys

1. **Together AI** (Recommended - Free tier available):
   - Sign up at: https://api.together.xyz/
   - Get API key from dashboard
   - Free tier includes FLUX.1-schnell-Free model

2. **OpenAI**:
   - Sign up at: https://platform.openai.com/
   - Create API key in settings
   - Costs apply per image

3. **Replicate**:
   - Sign up at: https://replicate.com/
   - Get API token from account settings
   - Pay per generation

## Verify Setup

After adding your API key, test with:
```bash
pnpm test:twitter-images
```

You should see "Successfully generated image for tweet" instead of "Failed to generate image".

## Twitter API Setup

Don't forget to also set up Twitter credentials:
```env
TWITTER_USERNAME=your_twitter_username
TWITTER_PASSWORD=your_twitter_password
TWITTER_EMAIL=your_twitter_email
```

## Character Configuration

Your character is already configured in `characters/grigory.character.json`:
```json
"imageProvider": "together",
"imageGeneration": {
  "enabled": true,
  "frequency": 0.3,
  "model": "black-forest-labs/FLUX.1-schnell-Free"
}
```

## Troubleshooting

1. **"No model settings found"**: Add the appropriate API key
2. **"Failed to generate image"**: Check API key is valid
3. **Images not appearing**: Verify `enabled: true` in config
4. **Wrong provider**: Check `imageProvider` matches your API key

## Testing in Production

Once configured, your agent will:
- Automatically generate images for ~30% of tweets
- Use [IMAGE] tags when specified
- Analyze content for relevant imagery
- Post with or without images based on success

Start your agent normally:
```bash
pnpm start
```

Images will be generated and attached to tweets automatically!