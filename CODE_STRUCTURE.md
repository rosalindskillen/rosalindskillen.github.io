# Code Structure and Editing Guide

This document provides an overview of the repository's code structure and instructions on how to edit the content and appearance of the Jekyll-based static website.

## Repository Summary

The repository contains a Jekyll-based static website built with a modular and customizable theme. The project is well-structured, following Jekyll conventions, making it straightforward to manage and extend. The website is likely a blog, portfolio, or personal website.

## Code Structure

The repository is organized into several key directories:

| Path | Description |
| :--- | :--- |
| `_config.yml` | The main configuration file for the Jekyll site. It controls global settings, collections, plugins, and theme-specific variables such as colors, fonts, and logos. |
| `pages/` | Contains the site's static pages, like "About" and "Contact." To edit the content of these pages, modify the Markdown files in this folder. |
| `collections/_posts/` | This directory holds all the blog posts. To add a new post or edit an existing one, create or modify a Markdown file in this folder, following the `YYYY-MM-DD-title.md` naming convention. |
| `_layouts/` | Contains the HTML layouts for different types of pages (e.g., `post.html`, `home.html`). These files define the overall structure of a page by including various components from the `_includes` directory. |
| `_includes/framework/` | This directory contains reusable HTML components (e.g., `header.html`, `footer.html`, `card-post.html`) that are included in the layouts. To change a specific part of the site's UI, you would likely edit a file here. |
| `_data/` | Contains structured data in YAML or JSON format, such as menu links (`menu.yml`) and author information (`authors.yml`). This data is used to populate corresponding sections of the site. |
| `_sass/theme/` | Contains the theme's custom styles. To make significant changes to the site's appearance beyond what is offered in `_config.yml`, you would edit the SCSS files here. |
| `assets/` | Stores all static assets like images, fonts, and JavaScript files. To add a new image or logo, place it in the `assets/images/` subdirectory. |

## How to Edit

Here are the basic instructions for editing the website's content and appearance:

### Modifying Content

*   **Static Pages**: To edit static pages like "About" or "Contact," navigate to the `pages/` directory and modify the corresponding Markdown file.
*   **Blog Posts**: To add a new blog post, create a new Markdown file in the `collections/_posts/` directory. The filename must follow the `YYYY-MM-DD-title.md` format.

### Customizing Appearance

*   **Simple Customizations**: For simple visual changes, such as modifying colors, fonts, or logos, edit the variables in the `_config.yml` file.
*   **Advanced Customizations**: For more complex changes to the site's appearance, you will need to edit the SCSS files in the `_sass/theme/` directory.

### Updating Data

*   **Menu Links**: To change the menu links, edit the `_data/menu.yml` file.
*   **Author Information**: To update author details, modify the `_data/authors.yml` file.

### Changing Layouts

*   To alter the structure of a page, modify the HTML files in the `_layouts/` directory or the components in the `_includes/framework/` directory.

### Blog Rendering

The blog page, accessible at `/blog/`, is rendered through a combination of files that work together to display a paginated list of posts.

| Path                  | Description                                                                                                                                                             |
| :-------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `blog/index.html`     | The main entry point for the `/blog/` URL. This file uses the `blog` layout to structure its content.                                                                     |
| `_layouts/blog.html`  | Defines the overall structure of the blog page. It iterates through the posts available in `site.posts` and includes `theme/cards/card-post.html` for each post.         |
| `theme/cards/card-post.html` | A reusable component that formats and displays the individual blog post previews on the blog page, including the title, description, and thumbnail.                  |
| `collections/_posts/` | Contains the individual blog posts as Markdown files. The content from these files is used to populate the blog page.                                                       |
