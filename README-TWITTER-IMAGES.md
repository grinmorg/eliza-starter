# 🖼️ Twitter Image Generation for Eliza AI Agent

This feature enables your AI agent to automatically generate and attach images to Twitter posts using AI image generation.

## 🚀 Quick Start

Your AI agent (Grigory) is now configured to:
1. **Automatically generate images** for ~30% of tweets
2. **Create contextual images** based on tweet content
3. **Use explicit image tags** like `[IMAGE: description]`

## 📋 Features

### Automatic Image Generation
- Analyzes tweet content for tech keywords (web3, blockchain, AI, etc.)
- Generates relevant images based on context
- Configurable frequency (default: 30% of tweets)

### Manual Control
- Add `[IMAGE: your description]` to any tweet
- The tag will be replaced with a generated image
- Full control over image content

### Smart Keyword Detection
Supports keywords in both English and Russian:
- `web3`, `blockchain`, `блокчейн`
- `solidity`, `смарт-контракт`
- `AI`, `ИИ`, `programming`, `программирование`
- `hackathon`, `хакатон`
- And many more...

## 🛠️ Configuration

Edit `characters/grigory.character.json`:

```json
"imageGeneration": {
  "enabled": true,              // Enable/disable feature
  "frequency": 0.3,             // 30% chance for auto-images
  "model": "black-forest-labs/FLUX.1-schnell-Free",
  "style": "professional, tech, coding, web3, modern, clean",
  "prompts": [                  // Random prompt pool
    "modern developer workspace",
    "blockchain visualization",
    // ... add more
  ]
}
```

## 📝 Usage Examples

### Example 1: Automatic Generation
```
Input: "Изучаю web3 технологии сегодня! 🚀"
Output: Tweet + blockchain visualization image
```

### Example 2: Explicit Image
```
Input: "Новый проект готов! [IMAGE: futuristic dapp interface]"
Output: Tweet + custom dapp interface image
```

### Example 3: No Image (70% of time)
```
Input: "Отличный день!"
Output: Just the tweet text
```

## 🏗️ Architecture

### Components
1. **Actions** (`src/actions/twitter-image/`)
   - `generateTwitterImageAction.ts` - Core image generation
   - `twitterPostEnhancer.ts` - Post enhancement logic

2. **Client Handler** (`src/clients/twitter-with-images.ts`)
   - Processes posts before sending to Twitter
   - Handles image fetching and attachment

3. **Plugin** (`src/plugins/twitter-image-plugin.ts`)
   - Integrates with Eliza plugin system
   - Provides service for other components

## 🧪 Testing

Run the test suite:
```bash
npm run test:twitter-images
```

Or manually:
```bash
node --loader ts-node/esm src/test/test-twitter-images.ts
```

## 🔧 Customization

### Add New Keywords
Edit `src/clients/twitter-with-images.ts`:
```typescript
const techKeywords = {
  "your_keyword": "image description",
  // Add more...
};
```

### Change Image Style
Update the `style` field in character config:
```json
"style": "cyberpunk, neon, futuristic"
```

### Adjust Frequency
Change `frequency` from 0.0 (never) to 1.0 (always)

## 📊 Performance Considerations

- Images are 1200x675 (Twitter optimal)
- Generation takes 2-5 seconds
- Consider costs of image generation API
- Failed generations fallback to text-only

## 🐛 Troubleshooting

### Images Not Generating
1. Check `enabled: true` in config
2. Verify API keys in `.env`
3. Check console logs for errors

### Wrong Images
1. Make prompts more specific
2. Add keywords to mapping
3. Use explicit `[IMAGE]` tags

### Rate Limits
1. Reduce `frequency` setting
2. Implement caching (future feature)
3. Check API limits

## 🚦 Status Indicators

Watch for these log messages:
- `"Generating image with prompt:"` - Image being created
- `"Successfully generated image"` - Image ready
- `"Failed to generate image"` - Fallback to text

## 🔮 Future Enhancements

Planned features:
- [ ] Image caching for common prompts
- [ ] Multi-image support for threads
- [ ] Image style variations by time/mood
- [ ] Analytics on image engagement
- [ ] Custom image templates

## 📚 Documentation

- Full docs: `docs/twitter-image-generation.md`
- API reference: See code comments
- Examples: `src/test/test-twitter-images.ts`

## 🤝 Contributing

To improve image generation:
1. Add keywords to `techKeywords` mapping
2. Enhance prompt templates
3. Optimize image dimensions
4. Add new style presets

---

**Note**: This feature requires proper API configuration for both Twitter and image generation services. Ensure all environment variables are set correctly.