# ElModdy Blog

This is the personal blog of ElModdy, built with [Jekyll](https://jekyllrb.com/) and hosted on GitHub Pages. It features a minimalist design using the [no-style-please](https://github.com/riggraz/no-style-please) theme.

## Features

- **Minimalist Design**: Focus on content with a clean, text-centric layout.
- **MathJax Support**: Render mathematical equations beautifully using MathJax.
- **GitHub Pages Integration**: Easy deployment via GitHub Actions or Pages.

## Content Highlights

The blog currently features posts on technical challenges and optimization problems, such as:

- **Berghain Challenge**: An exploration of the mathematical framework and optimization strategy for the Berghain bouncer simulation challenge.

## Local Development

To run this site locally, follow these steps:

1.  **Prerequisites**: Ensure you have Ruby and Bundler installed.
2.  **Install Dependencies**:
    ```bash
    bundle install
    ```
3.  **Run the Site**:
    ```bash
    bundle exec jekyll serve
    ```
4.  **View**: Open your browser and navigate to `http://localhost:4000`.

## Project Structure

- `_posts/`: Contains blog posts written in Markdown.
- `_includes/`: Custom HTML partials.
- `assets/`: Static assets like images, PDFs, and scripts.
- `_config.yml`: Jekyll configuration file.
- `Gemfile`: Ruby gem dependencies.
