# AGENTS.md

Guidelines for AI agents working on the Source Ghost theme.

## Build Commands

```bash
# Development mode with livereload (watches CSS, JS, and HBS files)
yarn dev

# Build assets only (compiles CSS and JS to assets/built/)
gulp build

# Create distribution zip for theme upload
yarn zip

# Run theme validation with gscan
yarn test

# Run validation in CI mode (fails on errors, verbose output)
yarn test:ci
```

**Note**: There are no unit tests in this project. Testing is done via `gscan` which validates the theme structure and Ghost API usage. Always run `yarn test` before committing changes.

## Technology Stack

- **Templating**: Handlebars (Ghost theme API)
- **CSS**: PostCSS with autoprefixer, cssnano, postcss-easy-import
- **JavaScript**: Vanilla ES6+ (no frameworks)
- **Build Tool**: Gulp 4
- **Package Manager**: Yarn
- **Node Version**: 22.21.1 (see `.nvmrc`)

## Code Style Guidelines

### JavaScript
- Use vanilla JavaScript (ES6+), no frameworks
- Use `const` and `let`, avoid `var`
- Use arrow functions where appropriate
- Wrap functionality in IIFEs or named functions
- Error handling: Use try/catch for async operations
- Event listeners: Use `addEventListener`, not inline handlers
- DOM queries: Use `querySelector`/`querySelectorAll`
- Check for element existence before attaching listeners

### CSS
- One main file: `assets/css/screen.css` (imports handled via postcss-easy-import)
- Use CSS custom properties (variables) for theming
- BEM-like naming: `.gh-component-name` (gh = Ghost)
- Utility classes: `.is-*` for states, `.has-*` for features
- Responsive breakpoints: mobile-first approach
- Autoprefixer handles vendor prefixes automatically

### Handlebars Templates
- Use double quotes for attributes
- Indentation: 4 spaces
- Partials: Use `{{> "partial-name"}}` syntax
- Comments: Use `{{!-- comment --}}` for template comments
- Conditionals: Prefer `{{#if}}` over `{{#unless}}` where readable
- Custom settings: Access via `{{@custom.setting_name}}`

### File Organization

```
assets/
  css/
    screen.css          # Main CSS entry point
  js/
    lib/                # Third-party libraries (loaded first)
      *.min.js
    main.js             # Entry point
    *.js                # Feature modules
  built/                # Generated files (DO NOT EDIT)
    screen.css
    source.js

partials/
  components/           # UI components (header, footer, etc.)
  icons/                # SVG icon partials
  typography/           # Font loading partials
  *.hbs                 # Shared partials

*.hbs                   # Root-level templates
```

### Naming Conventions
- **Templates**: lowercase with hyphens (e.g., `post-card.hbs`)
- **CSS Classes**: `.gh-*` prefix for Ghost theme components
- **JavaScript**: camelCase for variables/functions
- **Partials**: Use descriptive names (e.g., `email-subscription.hbs`)

### Ghost Theme API
- Use `{{asset "path"}}` for theme assets (handles cache-busting)
- Use `{{img_url image size="m"}}` for responsive images
- Required in templates: `{{ghost_head}}`, `{{ghost_foot}}`, `{{body_class}}`
- Image sizes: xs (160px), s (320px), m (600px), l (960px), xl (1200px), xxl (2000px)

### Error Handling
- Check DOM elements exist before manipulation
- Graceful degradation for missing features
- Use `beeper` in Gulp tasks for build errors

## Important Notes

- **DO NOT edit `assets/built/` files directly** - they are auto-generated
- Always run `yarn test` before committing to validate theme
- Use `yarn dev` for development - it watches all file types
- Theme must pass `gscan` validation before upload to Ghost
- Test with Ghost version >= 5.0.0
- Ghost admin: http://localhost:2368/ghost/ (when Ghost is running)
