# Project Overview: Jekyll Static Site

This project is a static website built using Jekyll, a popular static site generator written in Ruby. The structure is organized to separate content, layout, and configuration, which Jekyll then processes to generate a complete, static HTML/CSS/JS website in the `_site` directory.

### Core Jekyll Directories

*   **`_config.yml`**: The main configuration file for the Jekyll site. It contains global settings like the site title, URL, author information, and plugin configurations.
*   **`_data`**: Contains structured data files (in `.yml`, `.json`, etc.) that can be used throughout the site. For example, `authors.yml` and `social.json` store data that is referenced by the templates.
*   **`_includes`**: Holds reusable snippets of HTML that can be included in layouts or pages. This helps avoid repeating code for common elements like headers, footers, or navigation bars.
*   **`_layouts`**: Defines the main templates for different types of content. For instance, `post.html` dictates the structure of a single blog post, while `default.html` provides the base HTML structure for all pages.
*   **`collections/_posts`**: This is where the blog posts are located. Jekyll requires posts to be in the format `YYYY-MM-DD-title.md`.
*   **`_sass`**: Contains Sass (`.scss`) files for styling. These files are processed into a single CSS file that styles the final website.
*   **`_site`**: This directory contains the fully generated static website. Jekyll creates this folder and its contents when you build the site. **You should not edit files in this directory directly**, as your changes will be overwritten on the next build.

### Content Directories

*   **`pages/`**: Contains the site's main static pages, written in Markdown (e.g., `about.md`, `contact.md`). Each file is converted into an HTML page.
*   **`categories/`**: Holds Markdown files that define the different blog post categories.

### Assets & Environment

*   **`assets/`**: Stores all static assets like CSS, JavaScript, images, and fonts that are used by the site.
*   **`Gemfile` & `Gemfile.lock`**: Manages the Ruby dependencies (gems) for the project, including Jekyll itself and any plugins.
*   **`netlify.toml`**: A configuration file for deploying the site on Netlify, a popular hosting platform for static websites. It specifies build commands and other deployment settings.

### Updating Social Media Links

The social media links and their corresponding icons are configured in the `_data/social.json` file.

*   **How it Works**: The site uses data from `_data/social.json` to dynamically generate the social media links. The file `_includes/framework/social.html` reads this data and creates the HTML for the links. The icons are rendered using [Font Awesome](https://fontawesome.com/), which is a font and icon toolkit. Instead of image files, it uses CSS classes (e.g., `fab fa-twitter`) to display the correct icon.

*   **How to Update**:
    1.  **Edit `_data/social.json`**: Open this file to see a list of social media platforms.
    2.  **Update a Link**: To change the URL for a service, modify the `url` value for that entry.
    3.  **Update an Icon**: To change an icon, you need to find the correct class name from the Font Awesome library and update the `fa_icon` value. For example, to change the Twitter icon, you would update the `fa_icon` value from `fab fa-twitter` to a different Font Awesome class.
    4.  **Add or Remove Links**: You can add a new JSON object to the list to create a new social link. To remove a social link, simply delete its corresponding JSON object from `_data/social.json`. The site dynamically generates these links, so no other changes are needed elsewhere.
