# Vapor for Jekyll

My blog, running on GitHub pages using Vapor theme for Jekyll.

## Local Development

This site is built with Jekyll and hosted on GitHub Pages. To run it locally:

1. Make sure you have Ruby installed (preferably via rbenv)
2. Install dependencies:
   ```bash
   bundle install
   ```
3. Start the local server:
   ```bash
   bundle exec jekyll serve
   ```
4. Visit http://localhost:4000 in your browser

### Requirements
- Ruby 3.3.4 or later
- Bundler 2.6.5 or later

The site uses the following key dependencies:
- GitHub Pages
- Jekyll 3.10.0 (via github-pages gem)
- Redcarpet for Markdown processing
- Pygments for syntax highlighting
