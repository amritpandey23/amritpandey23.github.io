# System Patterns: Amrit's Blog

## 🏗️ Architecture
Static Site Generator (Nikola) -> Static HTML -> GitHub Pages.

## 🛠️ Key Technologies
- **Python**: Powering the Nikola build process.
- **Nikola**: The core static site generator.
- **Markdown**: Used for writing posts and pages.
- **Bootstrap 4**: Providing the CSS framework for layout and styling.
- **Jinja2/Mako**: Templating engines used by Nikola.

## 📋 Best Practices
- **Content over Code**: Focus on high-quality Markdown content.
- **Static Assets**: Large images should be optimized and placed in `files/images/`.
- **Consistent Metadata**: Each post should have relevant tags, categories, and slugs.
- **Version Control**: Keep source code and output in a git repository.

## 🤖 AI Interaction Pattern
- Agents should refer to the Memory Bank (`memory-bank/`) for context.
- Agents should use `nikola build` and `nikola serve` for development.
- Agents should update the Memory Bank files (especially `activeContext.md` and `progress.md`) after significant changes.
