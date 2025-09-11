# Lucas Brown's Cybersecurity Blog

A Hugo-based cybersecurity blog hosted on GitHub Pages, featuring content about malware analysis, APT campaigns, and OT Security.

## 🚀 Quick Start

### Prerequisites
- [Hugo Extended](https://gohugo.io/installation/) (version 0.100.0 or later)
- Git
- GitHub account

### Local Development
1. Clone the repository:
   ```bash
   git clone https://github.com/lucasdbrown/cyber_blog.git
   cd cyber_blog
   ```

2. Navigate to the blog directory:
   ```bash
   cd blog
   ```

3. Install Hugo theme dependencies:
   ```bash
   git submodule update --init --recursive
   ```

4. Start the development server:
   ```bash
   hugo server
   ```

5. Open your browser to `http://localhost:1313` to preview the site

## 📝 Adding New Blog Posts

### Method 1: Using Hugo's Built-in Command
1. Navigate to the blog directory:
   ```bash
   cd blog
   ```

2. Create a new post:
   ```bash
   hugo new posts/your-post-title.md
   ```

3. Edit the generated file in `content/posts/your-post-title.md`

### Method 2: Manual Creation
1. Create a new markdown file in `blog/content/posts/`
2. Use the following front matter template:

```yaml
---
title: "Your Post Title"
date: 2024-01-15T00:00:00Z
draft: false
tags: ["cybersecurity", "malware-analysis", "apt"]
categories: ["Analysis"]
description: "Brief description of your post"
---

Your post content goes here...
```

### Post Front Matter Options
- `title`: The post title (required)
- `date`: Publication date in RFC3339 format
- `draft`: Set to `false` to publish, `true` to keep as draft
- `tags`: Array of relevant tags
- `categories`: Array of categories
- `description`: SEO description for search engines
- `featured`: Set to `true` to feature on homepage
- `hero`: Path to hero image (optional)

### Writing Tips
- Use Markdown syntax for formatting
- Add code blocks with syntax highlighting:
  ```bash
  # Example bash code
  echo "Hello World"
  ```
- Include images by placing them in `blog/static/images/` and referencing with `/images/filename.jpg`
- Use Hugo shortcodes for advanced features (see [Hugo documentation](https://gohugo.io/content-management/shortcodes/))

## 🖼️ Managing Images

### Adding Images
1. Place images in `blog/static/images/`
2. Reference them in posts with `/images/filename.jpg`
3. For profile pictures, place in `blog/static/` and reference with `/filename.jpg`

### Image Optimization
- Use WebP format when possible for better performance
- Compress images before adding to reduce file size
- Recommended max width: 1200px for blog images

## 🎨 Customizing the Site

### Theme Configuration
The blog uses the Blowfish theme. Key configuration files:
- `blog/hugo.toml` - Main site configuration
- `blog/themes/blowfish/config/_default/params.toml` - Theme parameters
- `blog/themes/blowfish/config/_default/menus.en.toml` - Navigation menus

### Updating Profile Information
Edit `blog/content/_index.md` to update:
- Bio information
- Areas of interest
- Social media links

### Changing Social Links
Update the social links in `blog/hugo.toml`:
```toml
[[params.social]]
  name = "GitHub"
  url = "https://github.com/lucasdbrown"
  icon = "github"
```

## 🚀 Deploying to GitHub Pages

### Automatic Deployment (Recommended)
The site is configured for automatic deployment via GitHub Actions:

1. Push changes to the `main` branch:
   ```bash
   git add .
   git commit -m "Add new post: Your Post Title"
   git push origin main
   ```

2. GitHub Actions will automatically build and deploy the site
3. Your site will be available at `https://lucasdbrown.github.io/cyber_blog/`

### Manual Deployment
If you need to deploy manually:

1. Build the site:
   ```bash
   cd blog
   hugo --minify
   ```

2. Copy the `public/` directory contents to your GitHub Pages repository
3. Commit and push the changes

## 📁 Project Structure

```
cyber_blog/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions deployment
├── blog/                       # Hugo site directory
│   ├── content/
│   │   ├── _index.md          # Homepage content
│   │   └── posts/             # Blog posts
│   ├── static/
│   │   ├── profile.jpg        # Profile picture
│   │   └── images/            # Blog images
│   ├── themes/
│   │   └── blowfish/          # Hugo theme
│   └── hugo.toml              # Site configuration
└── README.md                  # This file
```

## 🔧 Troubleshooting

### Common Issues

**Hugo server not starting:**
- Ensure you have Hugo Extended installed
- Check that you're in the `blog/` directory
- Run `hugo version` to verify installation

**Theme not loading:**
- Run `git submodule update --init --recursive`
- Check that the theme is properly referenced in `hugo.toml`

**Images not displaying:**
- Ensure images are in `blog/static/images/`
- Use absolute paths starting with `/images/`
- Check file permissions and case sensitivity

**Build errors:**
- Check for syntax errors in markdown files
- Validate front matter YAML syntax
- Run `hugo --verbose` for detailed error messages

### Getting Help
- [Hugo Documentation](https://gohugo.io/documentation/)
- [Blowfish Theme Documentation](https://blowfish.page/docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)

## 📚 Content Guidelines

### Writing Style
- Write in a professional yet accessible tone
- Use clear headings and structure
- Include code examples where relevant
- Add diagrams or screenshots for complex concepts

### Post Categories
- **Analysis**: Malware analysis, threat research
- **APT**: Advanced Persistent Threat campaigns
- **OT Security**: Industrial Control Systems security
- **Tools**: Security tools and techniques
- **Tutorials**: Step-by-step guides

### Tagging System
Use consistent tags for better organization:
- `malware-analysis`
- `apt-campaigns`
- `ot-security`
- `threat-intelligence`
- `reverse-engineering`
- `incident-response`

## 🤝 Contributing

If you find issues or have suggestions:
1. Create an issue on GitHub
2. Fork the repository
3. Make your changes
4. Submit a pull request

## 📄 License

This blog content is licensed under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/).

---

**Happy blogging!** 🚀

For questions or support, reach out on [GitHub](https://github.com/lucasdbrown) or [LinkedIn](https://linkedin.com/in/lucas-d-brown/).
