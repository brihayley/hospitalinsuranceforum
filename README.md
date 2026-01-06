# Hospital Insurance Forum - WordPress Site

A modern WordPress site for the Hospital Insurance Forum, featuring a custom theme built with SCSS, CSS Custom Properties, and custom Gutenberg blocks. The site includes community features powered by BuddyPress and bbPress for forums and member interactions.

See the Hospital Insurance Forum theme's ReadMe file for more info: [`themes/hiforum/README.md`](wordpress/wp-content/themes/hiforum/README.md)

## Table of Contents

- [Overview](#overview)
- [Technology Stack](#technology-stack)
- [Development Setup](#development-setup)
- [Build Process](#build-process)
- [Project Structure](#project-structure)
- [Theme Development](#theme-development)
- [Custom Blocks](#custom-blocks)
- [Plugins](#plugins)
- [Deployment](#deployment)
- [Content Editing Guide](#content-editing-guide)
- [Code Standards](#code-standards)
- [Troubleshooting](#troubleshooting)

## Overview

This WordPress site serves as the Hospital Insurance Forum's online platform, providing:
- Community forums and discussions (bbPress)
- Member profiles and social features (BuddyPress)
- Custom content blocks for flexible page building
- SEO optimization (Yoast SEO)
- Custom field management (Advanced Custom Fields)

**WordPress Version:** 6.9+  
**PHP Version:** 8.3+  
**Node.js Version:** 20+

## Technology Stack

### Core Technologies
- **WordPress:** 6.9+ (latest minor version installed via build process)
- **PHP:** 8.3+ (with strict typing enabled)
- **Node.js:** 20+ (for development tools)

### Build Tools
- **Gulp:** Build automation and task runner
- **Sass/SCSS:** CSS preprocessing
- **PostCSS:** CSS processing with Autoprefixer
- **@wordpress/scripts:** Block compilation (webpack-based)

### Frontend
- **SCSS:** Styling with CSS Custom Properties
- **Vanilla JavaScript (ES6+):** No jQuery dependency
- **CSS Custom Properties:** For theming and design tokens

## Development Setup

### Prerequisites

Before you begin, ensure you have the following installed:
- **Node.js** (v20 or higher) and npm
- **PHP** (v8.3 or higher)
- **WordPress** (6.0 or higher)
- **Git**

### Initial Setup

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd hospitalinsuranceforum
   ```

2. **Install theme dependencies:**
   ```bash
   cd wordpress/wp-content/themes/hiforum
   npm install
   ```

3. **Install block dependencies (for each custom block):**
   ```bash
   cd blocks/brand-button
   npm install
   cd ../hero
   npm install
   cd ../icon-block
   npm install
   cd ../section-title
   npm install
   ```

4. **Build theme assets:**
   ```bash
   # From the theme directory
   npm run build
   ```

5. **Configure WordPress:**
   - Copy `wp-config-sample.php` to `wp-config.php` if needed
   - Set up your database connection
   - Configure site URL and other WordPress settings

### Development Workflow

1. **Start watching for changes:**
   ```bash
   cd wordpress/wp-content/themes/hiforum
   npm run watch
   ```
   This will watch for changes in SCSS, JavaScript, and block files, automatically rebuilding when files are modified.

2. **Build for production:**
   ```bash
   npm run build
   ```

3. **Individual build tasks:**
   ```bash
   npm run css      # Build CSS only
   npm run js       # Build JavaScript only
   npm run blocks   # Build custom blocks only
   ```

## Build Process

### Theme Assets

The theme uses Gulp to compile and minify assets:

1. **SCSS Compilation:**
   - Source files: `assets/src/css/**/*.scss`
   - Output: `assets/dist/css/style.css` and `style.min.css`
   - Process: SCSS → PostCSS (Autoprefixer) → Minification

2. **JavaScript Compilation:**
   - Source files: `assets/src/js/**/*.js`
   - Output: `assets/dist/js/main.js` and `main.min.js`
   - Process: Terser minification

3. **Block Compilation:**
   - Each block uses `@wordpress/scripts` independently
   - Source: `blocks/{block-name}/src/`
   - Output: `blocks/{block-name}/build/`
   - Process: Webpack compilation (JSX, ES6, SCSS)

### Build Commands

| Command | Description |
|---------|-------------|
| `npm run build` | Build all assets (CSS, JS, blocks) |
| `npm run start` | Build and watch for changes |
| `npm run watch` | Watch for changes and rebuild |
| `npm run css` | Build CSS only |
| `npm run js` | Build JavaScript only |
| `npm run blocks` | Build custom blocks only |

## Project Structure

```
hospitalinsuranceforum/
├── deployment/              # AWS CodeDeploy configuration
│   ├── develop/
│   ├── staging/
│   ├── production/
│   └── maintenance/
├── scripts/                 # Deployment scripts
│   └── post_deploy.sh      # Post-deployment tasks
├── buildspec.yml           # AWS CodeBuild configuration
├── wordpress/              # WordPress core and content
│   ├── wp-content/
│   │   ├── themes/
│   │   │   └── hiforum/    # Custom theme
│   │   │       ├── assets/
│   │   │       │   ├── src/    # Source files (SCSS, JS)
│   │   │       │   └── dist/   # Compiled files (CSS, JS)
│   │   │       ├── blocks/     # Custom Gutenberg blocks
│   │   │       ├── inc/        # PHP includes
│   │   │       ├── template-parts/
│   │   │       ├── functions.php
│   │   │       ├── theme.json  # Block editor configuration
│   │   │       └── gulpfile.js
│   │   └── plugins/        # WordPress plugins
│   └── wp-config.php
└── README.md
```

### Theme Structure

```
hiforum/
├── assets/
│   ├── src/
│   │   ├── css/
│   │   │   ├── components/     # Component styles
│   │   │   ├── globals/         # Global styles
│   │   │   ├── _variables.scss # SCSS variables
│   │   │   ├── _utility.scss   # Utility classes
│   │   │   └── style.scss      # Main stylesheet
│   │   └── js/
│   │       └── main.js         # Main JavaScript
│   └── dist/                   # Compiled assets (git-ignored)
├── blocks/                      # Custom Gutenberg blocks
│   ├── brand-button/
│   ├── hero/
│   ├── icon-block/
│   └── section-title/
├── inc/                         # PHP includes
│   ├── enqueue-assets.php      # Asset enqueuing
│   ├── theme-support.php       # Theme features
│   ├── nav-menus.php           # Navigation menus
│   └── register-blocks.php     # Block registration
├── template-parts/              # Reusable template parts
├── acf-json/                    # ACF field group exports
├── functions.php                # Main theme file
├── theme.json                   # Block editor settings
└── gulpfile.js                  # Build configuration
```

## Theme Development

### Theme Configuration

The theme uses `theme.json` for block editor configuration instead of traditional `add_theme_support()` hooks where possible. Key settings include:

- **Color Palette:** Custom colors defined in `theme.json` (also generates CSS variables)
- **Typography:** Custom font sizes with fluid typography support
- **Spacing:** Custom spacing scale (0.25rem increments)
- **Layout:** Content width (1280px) and wide width (1440px)
- **Appearance Tools:** Enabled (auto-enables new block styling features)

**Note:** The `appearanceTools` setting is set to `true`, which means new block styling features from WordPress updates will automatically be available. This can be both helpful and potentially overwhelming - monitor WordPress changelogs for new features.

### SCSS Architecture

- **Variables:** Shared variables (breakpoints, colors) in `_variables.scss`
- **Components:** Reusable component styles in `components/`
- **Globals:** Base styles, header, footer in `globals/`
- **Utilities:** Utility mixins, functions, and classes in `_utility.scss`

**Important:** Use SCSS variables for breakpoints in media queries (CSS custom properties cannot be used in media queries).

### JavaScript Architecture

- **Vanilla JavaScript:** ES6+ features, no jQuery
- **Modular:** Organized by feature/component
- **WordPress Integration:** Properly enqueued with dependencies

## Custom Blocks

The theme includes several custom Gutenberg blocks:

1. **Brand Button** (`blocks/brand-button/`)
2. **Hero** (`blocks/hero/`)
3. **Icon Block** (`blocks/icon-block/`)
4. **Section Title** (`blocks/section-title/`)

### Block Development

Each block is self-contained with its own `package.json` and build process:

1. **Block Structure:**
   ```
   blocks/{block-name}/
   ├── src/
   │   ├── index.js      # Block registration
   │   ├── edit.js       # Editor component
   │   ├── save.js       # Save function
   │   ├── style.scss    # Frontend styles
   │   └── editor.scss   # Editor styles
   ├── build/            # Compiled output (auto-generated)
   ├── block.json        # Block metadata
   └── package.json       # Build configuration
   ```

2. **Building Blocks:**
   - Blocks are automatically discovered and built via Gulp
   - Each block runs `npm run build` using `@wordpress/scripts`
   - Blocks are automatically registered via `inc/register-blocks.php`

3. **Creating a New Block:**
   ```bash
   # Use WordPress create-block tool
   npx @wordpress/create-block {block-name} --no-plugin
   
   # Move to theme blocks directory
   mv {block-name} wordpress/wp-content/themes/hiforum/blocks/
   
   # Install dependencies
   cd blocks/{block-name}
   npm install
   
   # Build
   npm run build
   ```

## Plugins

### Active Plugins

1. **Advanced Custom Fields (ACF)**
   - Purpose: Custom field management
   - Usage: Create custom fields for posts, pages, and custom post types
   - Field groups: Stored in `acf-json/` directory (version controlled)

2. **BuddyPress**
   - Purpose: Social networking and member profiles
   - Features: User profiles, activity streams, groups, messaging
   - Customization: Styles in `assets/src/css/components/_buddypress.scss`

3. **bbPress**
   - Purpose: Forum functionality
   - Features: Forums, topics, replies
   - Note: Private forums use "Private" status (prefix removed in theme)

4. **Yoast SEO**
   - Purpose: SEO optimization
   - Features: Meta tags, XML sitemaps, content analysis, schema markup

5. **WP Migrate DB Pro**
   - Purpose: Database migration tool
   - Usage: Development and staging environment synchronization

### Plugin Configuration

- **ACF Field Groups:** Exported to `acf-json/` for version control
- **BuddyPress/bbPress:** Custom styling integrated with theme
- **Yoast SEO:** Configured for optimal SEO performance

## Deployment

### Deployment Environments

The project uses AWS CodeBuild and CodeDeploy for automated deployments:

- **Develop:** Development environment
- **Staging:** Staging/pre-production environment
- **Production:** Live production environment
- **Maintenance:** Maintenance mode environment

### Build Process

The `buildspec.yml` file defines the build process:

1. **Pre-build:**
   - PHP syntax validation
   - Environment-specific `appspec.yml` selection
   - YAML validation

2. **Build:**
   - WordPress core download (latest minor version of 6.9)
   - Theme asset compilation (`npm install` → `gulp build`)
   - Cleanup of development files

3. **Post-deploy:**
   - Database migrations (`wp core update-db`)
   - Environment-specific configuration

### Deployment Checklist

Before deploying:

- [ ] All assets are built (`npm run build`)
- [ ] PHP syntax is valid (checked automatically)
- [ ] No uncommitted changes in `assets/dist/` (should be git-ignored)
- [ ] Database migrations are tested
- [ ] Environment-specific configuration is correct

## Content Editing Guide

### For Content Editors

This guide is for non-technical users who will be editing content on the site.

### Getting Started

1. **Log in to WordPress:**
   - Navigate to `/wp-admin`
   - Use your assigned username and password

2. **Understanding the Dashboard:**
   - **Posts:** Blog posts and articles
   - **Pages:** Static pages (About, Contact, etc.)
   - **Forums:** Community forums (bbPress)
   - **Members:** User profiles (BuddyPress)

### Using the Block Editor

The site uses WordPress's Block Editor (Gutenberg) for content creation.

#### Available Blocks

**Standard WordPress Blocks:**
- Paragraph, Heading, List, Quote, Image, Gallery, etc.
- All standard WordPress blocks are available

**Custom Theme Blocks:**
- **Brand Button:** Styled button with brand colors
- **Hero:** Large hero section with background image
- **Icon Block:** Content block with icon
- **Section Title:** Styled section heading

#### Block Editor Tips

1. **Adding Blocks:**
   - Click the `+` button or type `/` followed by the block name
   - Example: `/hero` to add a Hero block

2. **Block Settings:**
   - Select a block to see its settings in the right sidebar
   - Use the toolbar above the block for formatting options

3. **Color Palette:**
   - The theme provides a custom color palette
   - Available colors: Primary, Secondary, Text Dark, Text Light, etc.
   - Access via block color settings

4. **Spacing:**
   - Use the spacing controls in block settings
   - Custom spacing scale: 0.25rem increments

#### Utility Classes

The theme provides utility classes that can be added to blocks via the "Additional CSS class(es)" field in block settings. These classes help with layout and responsive behavior.

**Container Padding:**
- **`.container_padding`** - Adds responsive horizontal padding to a block
  - Mobile: 18px (1.125rem) padding
  - Desktop: 32px (2rem) padding
  - Automatically adjusts between breakpoints
  - Useful for maintaining consistent spacing from screen edges

**Responsive Visibility Classes:**

These classes control when content is visible on different screen sizes:

- **`.mobile_only`** - Content visible only on mobile devices (up to 500px)
  - Hidden on tablet and desktop
  - Use when you want mobile-specific content

- **`.tablet_only`** - Content visible only on tablet devices (501px - 1023px)
  - Hidden on mobile and desktop
  - Use when you want tablet-specific content

- **`.desktop_only`** or **`.hide_on_mobile`** - Content visible only on desktop (1024px and above)
  - Hidden on mobile and tablet
  - Use for desktop-only content or to hide elements on smaller screens

- **`.hide_on_desktop`** or **`.mobile_only.tablet_only`** - Content visible on mobile and tablet, hidden on desktop
  - Visible on screens up to 1023px
  - Hidden on desktop (1024px+)
  - Use when you want to hide content on large screens

**How to Use Utility Classes:**

1. Select the block you want to modify
2. In the block settings sidebar, find the **"Advanced"** section
3. Locate the **"Additional CSS class(es)"** field
4. Enter one or more utility classes (separated by spaces)
5. Example: `container_padding mobile_only` (adds padding and shows only on mobile)

**Breakpoints:**
- Mobile: up to 500px
- Tablet: 501px - 1023px
- Desktop: 1024px and above

**Tips:**
- You can combine multiple classes: `container_padding desktop_only`
- Preview your changes on different screen sizes to verify responsive behavior
- Use these classes to create different content experiences for mobile vs. desktop users

### Managing Content

#### Pages

1. **Creating a Page:**
   - Go to **Pages → Add New**
   - Enter a title
   - Add content using blocks
   - Set featured image if needed
   - Publish or save as draft

2. **Page Templates:**
   - Some pages may have custom templates
   - Check the **Page Attributes** panel for template options

#### Posts

1. **Creating a Post:**
   - Go to **Posts → Add New**
   - Similar to pages, but posts include categories and tags
   - Posts appear in blog feeds and archives

#### Forums (bbPress)

1. **Creating a Forum:**
   - Go to **Forums → Add New**
   - Set forum status (Public, Private, Hidden)
   - Private forums are members-only

2. **Forum Topics:**
   - Users can create topics within forums
   - Topics can have replies and discussions

#### Member Profiles (BuddyPress)

1. **Viewing Profiles:**
   - Navigate to member profiles from the community area
   - Profiles show activity, groups, and connections

### Advanced Custom Fields (ACF)

Some content types use custom fields for additional data. These fields appear below the main content editor and provide additional settings and options.

#### Finding Custom Fields

1. **Location:**
   - Custom fields may appear below the main content editor OR in the right sidebar panel, depending on the field group
   - Look for field group sections with collapsible accordions
   - Fields vary by content type and template
   - Check both the main editor area and the sidebar when looking for custom fields

2. **Using Custom Fields:**
   - Follow the field labels and instructions
   - Required fields are marked with an asterisk (*)
   - Toggle switches show "On" or "Off" states
   - Some fields may be organized in accordion sections

#### Field Groups

**Additional Page Settings**

This field group appears on pages and posts using the default template and provides page-level settings:

- **Location:** Appears in the right sidebar panel when editing pages or posts with the default template
- **Fields:**
  - **Restrict Content to Members-Only** (Toggle)
    - **Purpose:** Controls whether a page is accessible to all visitors or only logged-in members
    - **How to Use:**
      - Toggle **ON** ("Yes - Members Only") to restrict the page to logged-in members only
      - Toggle **OFF** ("No - Public") to make the page publicly accessible
    - **When to Use:** Use this for member-exclusive content, resources, or pages that should only be visible to registered users
    - **Note:** When enabled, non-logged-in visitors will be redirected or see a login prompt

**Menu Item**

This field group appears when editing menu items in the primary navigation menu:

- **Location:** Appears when editing menu items in **Appearance → Menus**
- **Fields:**
  - **Is Button?** (Checkbox)
    - **Purpose:** Displays the menu item as a styled button instead of a regular link
    - **How to Use:**
      - Check the "Show as button" checkbox to display the menu item as a button
      - Leave unchecked to display as a regular navigation link
    - **When to Use:** Use this for call-to-action items like "Sign Up", "Contact", or "Get Started" that should stand out in the navigation
    - **Styling:** Button-styled menu items use the theme's button styles and brand colors

### SEO with Yoast SEO

1. **SEO Analysis:**
   - Scroll down to see the Yoast SEO meta box
   - Green, yellow, or red indicators show SEO status

2. **Focus Keyphrase:**
   - Enter your main keyword/phrase
   - Yoast will analyze your content

3. **Meta Description:**
   - Write a compelling description (150-160 characters)
   - This appears in search engine results

4. **Readability:**
   - Yoast also checks readability
   - Follow suggestions for better content structure

### Best Practices

1. **Images:**
   - Use descriptive file names
   - Add alt text for accessibility
   - Optimize images before uploading (recommended: WebP format)

2. **Links:**
   - Use descriptive link text (not "click here")
   - Check that links work after publishing

3. **Content Structure:**
   - Use headings (H2, H3) to organize content
   - Keep paragraphs short and scannable
   - Use lists for multiple items

4. **Preview Before Publishing:**
   - Always preview your content before publishing
   - Check mobile view if possible

### Common Tasks

#### Updating the Homepage

1. Go to **Pages → All Pages**
2. Find your homepage (usually titled "Home")
3. Click **Edit**
4. Make your changes
5. Click **Update**

#### Adding a New Page

1. Go to **Pages → Add New**
2. Enter page title
3. Add content using blocks
4. Set page attributes (parent page, template, etc.)
5. Click **Publish**

#### Managing Menus

1. Go to **Appearance → Menus**
2. Select a menu to edit
3. Add/remove items
4. Drag to reorder
5. Click **Save Menu**

#### Managing Users

1. Go to **Users → All Users**
2. View, edit, or delete users
3. Assign roles (Administrator, Editor, Author, etc.)

### Getting Help

- **WordPress Documentation:** [wordpress.org/support](https://wordpress.org/support)
- **Block Editor Guide:** [wordpress.org/documentation/article/wordpress-block-editor](https://wordpress.org/documentation/article/wordpress-block-editor)
- **Contact:** Reach out to the development team for technical issues

## Code Standards

This project follows strict coding standards defined in `.cursor/rules/code-standards.mdc`. Key highlights:

### PHP Standards
- **Strict Typing:** All PHP files use `declare(strict_types=1);`
- **Naming:** 
  - Functions/variables: `snake_case`
  - Classes: `PascalCase`
  - Constants: `UPPER_CASE`
- **File Size:** Max 500 lines per file
- **Function Size:** Max 50 lines per function

### CSS/SCSS Standards
- **Mobile-First:** Always write mobile styles first, then use `min-width` media queries
- **BEM Naming:** Block Element Modifier convention
- **No IDs:** Never use ID selectors in CSS
- **No !important:** Avoid `!important` declarations
- **CSS Custom Properties:** Use for theming (but not in media queries)
- **SCSS Variables:** Use for breakpoints in media queries

### JavaScript Standards
- **Vanilla JS:** ES6+, no jQuery
- **Naming:** camelCase for variables/functions, PascalCase for classes
- **Modules:** Use ES6 modules for organization
- **WordPress Integration:** Proper enqueuing with dependencies

### Git Standards
- **Commit Format:** `type(scope): description`
  - Example: `feat(user-auth): add password reset`
- **Branch Format:** `feature/description` or `bugfix/description`

For complete standards, see `.cursor/rules/code-standards.mdc` and `.cursor/rules/wordpress-blocks.mdc`.

## Troubleshooting

### Build Issues

**Problem:** `npm install` fails
- **Solution:** Ensure Node.js v20+ is installed
- Check `package.json` for correct Node version

**Problem:** Gulp tasks fail
- **Solution:** Run `npm install` in theme directory
- Check that all dependencies are installed

**Problem:** Blocks don't build
- **Solution:** Run `npm install` in each block directory
- Check `package.json` exists in block directory

### WordPress Issues

**Problem:** Styles not loading
- **Solution:** Run `npm run build` to compile assets
- Check that `assets/dist/` files exist
- Clear browser cache

**Problem:** Blocks not appearing
- **Solution:** Check `inc/register-blocks.php` is included in `functions.php`
- Verify `block.json` exists in block directory
- Check browser console for JavaScript errors

**Problem:** PHP errors
- **Solution:** Check PHP version (requires 8.3+)
- Enable WordPress debug mode: `define('WP_DEBUG', true);` in `wp-config.php`
- Check error logs

### Deployment Issues

**Problem:** Build fails in CI/CD
- **Solution:** Check `buildspec.yml` configuration
- Verify environment variables are set
- Check build logs for specific errors

**Problem:** Database migration fails
- **Solution:** Check `post_deploy.sh` script
- Verify database connection
- Check WordPress version compatibility

## Additional Resources

- **Theme README:** [`wordpress/wp-content/themes/hiforum/README.md`](wordpress/wp-content/themes/hiforum/README.md)
- **Code Standards:** `.cursor/rules/code-standards.mdc`
- **Block Development:** `.cursor/rules/wordpress-blocks.mdc`
- **WordPress Documentation:** [developer.wordpress.org](https://developer.wordpress.org)
- **Block Editor Handbook:** [developer.wordpress.org/block-editor](https://developer.wordpress.org/block-editor/)

## Support

For technical issues or questions:
- Review the troubleshooting section above
- Check WordPress and plugin documentation
- Contact the development team

---

**Last Updated:** 2026  
**Maintained by:** fjorge  
**License:** GNU General Public License v2 or later
