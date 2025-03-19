# Social Media Preview Optimization

This Jekyll site has been optimized for social media previews when content is shared on platforms like Twitter, Facebook, LinkedIn, etc.

## How It Works

Social media previews are implemented using Open Graph and Twitter Card meta tags in the `<head>` section of every page. These tags are defined in `_includes/social_meta.html` and included in the main layout.

## Adding Social Media Images to Posts

For optimal sharing, add social media preview images to your posts:

1. Add a properly sized image (recommended: 1200x630px) to your `/images/` directory
2. Include the following front matter in your post:

```markdown
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD HH:MM:SS
image: "/images/your-image-filename.jpg"
description: "A concise, compelling description for social sharing (150-160 characters)"
---
```

## Image Guidelines

- **Dimensions**: 1200x630px is ideal for most platforms
- **Aspect Ratio**: 1.91:1
- **Quality**: Keep file sizes reasonable (< 1MB) for fast loading
- **Content**: Include relevant visuals and text that entices clicks

## Testing Social Previews

Before publishing, test your social media previews using:
- [LinkedIn Post Inspector](https://www.linkedin.com/post-inspector/)
- [Twitter Card Validator](https://cards-dev.twitter.com/validator)
- [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/)

## Troubleshooting

If your previews aren't showing correctly:
1. Ensure your `_config.yml` has the correct `url` value
2. Verify image paths are correct and images are accessible
3. Use the validator tools to force a refresh of the cache 