# Twitter Image Generation for Eliza AI Agent

This documentation explains how the Twitter image generation feature works for your AI agent.

## Overview

The AI agent can now automatically generate images for Twitter posts based on:
1. Explicit `[IMAGE: description]` tags in posts
2. Automatic contextual analysis of post content
3. Random selection from predefined image prompts
4. Configurable frequency settings

## Configuration

The image generation is configured in your character file (`characters/grigory.character.json`):

```json
{
  "imageGeneration": {
    "enabled": true,
    "frequency": 0.3,  // 30% chance to add image to posts without [IMAGE] tag
    "model": "black-forest-labs/FLUX.1-schnell-Free",
    "style": "professional, tech, coding, web3, modern, clean",
    "prompts": [
      // Predefined prompts that can be randomly selected
    ]
  }
}
```

## How It Works

### 1. Manual Image Generation
Add `[IMAGE: description]` to any post:
```
Только что задеплоил новый смарт-контракт! 🚀 [IMAGE: smart contract deployment visualization]
```

### 2. Automatic Image Generation
The system analyzes post content for keywords and automatically adds relevant images:
- Web3/Blockchain keywords → blockchain visualizations
- Programming/coding keywords → developer workspace images
- AI/ML keywords → neural network visualizations
- Hackathon mentions → collaborative coding scenes

### 3. Random Image Selection
Based on the `frequency` setting, posts may randomly get images from the predefined prompts list.

## Implementation Details

### Actions
1. **generateTwitterImageAction**: Generates images on demand
2. **twitterPostEnhancer**: Enhances posts with contextual image tags

### Services
- **TwitterImageService**: Processes posts and generates images before sending to Twitter

### Integration
The system integrates with the Twitter client to:
1. Intercept outgoing posts
2. Process content for image generation
3. Generate images using the configured AI model
4. Attach images to tweets automatically

## Usage Examples

### Example 1: Explicit Image Tag
```
Input: "Изучаю Solidity сегодня! 📚 [IMAGE: Solidity code on IDE screen]"
Output: Tweet with text + generated image of Solidity code
```

### Example 2: Automatic Contextual Image
```
Input: "Начинаю новый web3 проект с использованием смарт-контрактов"
Output: Tweet with text + auto-generated blockchain visualization
```

### Example 3: Random Image from Presets
```
Input: "Отличный день для программирования! 💻"
Output: Tweet with text + random image from predefined prompts (30% chance)
```

## API Usage

### Using the Twitter Image Handler
```typescript
import { createTwitterImageHandler } from './clients/twitter-with-images';

// In your Twitter client
const imageHandler = createTwitterImageHandler(runtime);
const { text, mediaData, mediaType } = await imageHandler.processPostWithImage(content);
```

### Using the Actions Directly
```typescript
// Generate image for a specific prompt
await runtime.processAction("GENERATE_TWITTER_IMAGE", message, state);

// Enhance a post with contextual image
await runtime.processAction("ENHANCE_TWITTER_POST", message, state);
```

## Customization

### Adding New Keywords
Edit `src/clients/twitter-with-images.ts` to add new keyword mappings:
```typescript
const techKeywords = {
  "your_keyword": "your image description",
  // ... more keywords
};
```

### Changing Image Dimensions
Twitter optimal dimensions are 1200x675 (16:9 ratio). To change:
```typescript
const imageResult = await generateImage({
  prompt: prompt,
  width: 1200,  // Change these values
  height: 675,
}, runtime);
```

### Custom Styles
Modify the `style` field in your character configuration to change the visual style of generated images.

## Troubleshooting

### Images Not Generating
1. Check `imageGeneration.enabled` is `true` in character config
2. Verify the image provider is properly configured
3. Check logs for error messages

### Wrong Image Context
1. Review keyword mappings in `twitter-with-images.ts`
2. Add more specific keywords for your use case
3. Use explicit `[IMAGE]` tags for precise control

### Performance Issues
1. Consider reducing image dimensions
2. Adjust `frequency` to generate fewer images
3. Use cached images for common prompts

## Best Practices

1. **Use Descriptive Prompts**: More detailed prompts generate better images
2. **Match Your Style**: Keep prompts consistent with your character's style
3. **Test Keywords**: Test which keywords trigger which images
4. **Monitor Usage**: Track image generation to optimize costs
5. **Fallback Content**: Always ensure posts work without images

## Future Enhancements

Potential improvements:
- Image caching for common prompts
- Multi-image support for threads
- Dynamic style adjustment based on content
- Integration with other social platforms
- A/B testing for image effectiveness