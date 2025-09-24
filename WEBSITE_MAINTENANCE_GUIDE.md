# Academic Website Maintenance Guide

**Last Updated:** September 23, 2025  
**Website Framework:** Jekyll (academicpages theme)  
**Repository:** academicpages.github.io

---

## **Table of Contents**

1. [Overview](#overview)
2. [Website Architecture](#website-architecture)
3. [Content Management](#content-management)
4. [Theme & Styling](#theme--styling)
5. [Template System](#template-system)
6. [Performance Optimizations](#performance-optimizations)
7. [Common Tasks](#common-tasks)
8. [Troubleshooting](#troubleshooting)
9. [Development Workflow](#development-workflow)
10. [Deployment](#deployment)

---

## **Overview**

This website is built using Jekyll with a heavily customized academicpages theme. Key customizations include:

- **Dark mode compatibility** with CSS variables
- **Performance optimizations** (removed heavy external libraries)
- **Custom testimonial system** 
- **Academic-focused color scheme**
- **Optimized contact form** with Formspree integration

### **Key Features Implemented**
- ✅ Dark/Light theme switching
- ✅ Academic paper display with custom colors
- ✅ Teaching portfolio with testimonials
- ✅ Performance-optimized loading
- ✅ Contact form integration
- ✅ Clean navigation structure

---

## **Website Architecture**

### **Directory Structure**
```
├── _config.yml                 # Main Jekyll configuration
├── _data/                      # Site data files
├── _includes/                  # Reusable template components
│   ├── archive-single.html     # Main content display template
│   ├── footer.html             # Site footer
│   └── ...
├── _layouts/                   # Page layout templates
├── _pages/                     # Main site pages
│   ├── about.md
│   ├── teaching.html
│   ├── research.html
│   └── contact.md
├── _sass/                      # Styling files
│   └── theme/
│       ├── _default_light.scss # Light theme variables
│       └── _default_dark.scss  # Dark theme variables
├── _teaching/                  # Teaching content
└── _publications/              # Research publications
```

### **Core Components**

1. **archive-single.html** - Main template for displaying content items
2. **Teaching system** - Organized by type (TA, Guest Lecture, Testimonial)
3. **CSS variables** - Theme-aware color system
4. **Contact integration** - Formspree-powered contact form

---

## **Content Management**

### **Adding New Publications**

Create a new file in `_publications/` with this format:

```yaml
---
title: "Your Paper Title"
collection: publications
type: "journal" # or "conference", "preprint"
permalink: /publication/paper-slug
date: 2025-01-01
venue: "Journal Name"
paperurl: "https://link-to-paper.com"
coauthors: "with Author Name"
coauthor_urls: ["https://author-website.com"]
---

Abstract text goes here.
```

### **Adding Teaching Experience**

For **Teaching Assistant** positions:

```yaml
---
title: "Course Name (COURSE 123)"
collection: teaching
type: "Teaching Assistant - Undergraduate"
date: 2025-01-01
terms: "Fall 2024, Spring 2025"
---

Course description here.
```

For **Guest Lectures**:

```yaml
---
title: "Lecture Topic"
collection: teaching
type: "Guest Lecture"
date: 2025-01-01
course: "COURSE 123"
terms: "Fall 2024"
---

Lecture details here.
```

### **Adding Testimonials**

```yaml
---
title: "Student Feedback"
collection: teaching
type: "Testimonial"
date: 2024-01-01
venue: "University of California, Irvine"
course: "COURSE 123"
quarter: "Fall 2024"
---

Student testimonial text here (no quotes needed).
```

**Important:** Testimonials display automatically with course information inline.

---

## **Theme & Styling**

### **CSS Variables System**

The website uses CSS variables for theme compatibility:

**Light Theme** (`_sass/theme/_default_light.scss`):
```scss
--academic-paper-title-color        : #bb2815;
--academic-author-link-color        : #bb2815;
--academic-presentation-text-color  : #{$dark-gray};
--academic-button-bg-color          : #f0f0f0;
--academic-button-border-color      : #ccc;
```

**Dark Theme** (`_sass/theme/_default_dark.scss`):
```scss
--academic-paper-title-color        : #{$link};
--academic-author-link-color        : #{$link};
--academic-presentation-text-color  : #{$text};
--academic-button-bg-color          : #{$background-light};
--academic-button-border-color      : #{$light-gray};
```

### **Adding New Colors**

1. Add variable to both light and dark theme files
2. Use `var(--variable-name)` in templates
3. Always provide fallback: `var(--variable-name, #fallback)`

---

## **Template System**

### **archive-single.html Structure**

This is the main template that handles:
- Publications (with PDF/slides buttons)
- Teaching positions
- **Testimonials** (special formatting)
- Guest lectures

**Key Logic:**
```liquid
{% if post.type == "Testimonial" %}
  <!-- Testimonial display -->
  <div style="font-style: italic; color: var(--academic-presentation-text-color);">
    {{ post.content | strip }}
    {{ post.course }}{% if post.quarter %}, {{ post.quarter }}{% endif %}
  </div>
{% else %}
  <!-- Standard publication/teaching format -->
  ...
{% endif %}
```

### **Template Customization**

- **Colors**: Use CSS variables for theme compatibility
- **Conditional logic**: Check `post.type` for different content types
- **Styling**: Inline styles with CSS variables for dynamic theming

---

## **Performance Optimizations**

### **Removed Heavy Libraries**
- **Plotly.js** (~3MB) - Removed from footer
- **Mermaid.js** (~1MB) - Removed from footer  
- **MathJax** - Made conditional (only loads when needed)

### **Optimization Techniques**
1. **Conditional loading** - Scripts only load on pages that need them
2. **CSS variables** - Reduced inline style redundancy
3. **Incremental builds** - Faster development rebuilds

### **Performance Monitoring**
```bash
# Check build time
bundle exec jekyll build --profile

# Check site size
du -sh _site/
```

---

## **Common Tasks**

### **1. Update Contact Information**

Edit `_pages/contact.md`:
```yaml
---
title: "Contact"
permalink: /contact/
---

Your updated contact form and information here.
```

### **2. Add New Navigation Items**

Edit `_data/navigation.yml`:
```yaml
main:
  - title: "New Page"
    url: /new-page/
```

### **3. Modify Footer**

Edit `_includes/footer.html` and `_includes/footer/custom.html`

### **4. Update About Page**

Edit `_pages/about.md` with your current information.

### **5. Change Color Scheme**

Modify the CSS variables in:
- `_sass/theme/_default_light.scss`
- `_sass/theme/_default_dark.scss`

---

## **Troubleshooting**

### **Testimonials Not Displaying**
- Check `type: "Testimonial"` in front matter
- Verify `_pages/teaching.html` includes testimonial section
- Ensure `archive-single.html` has testimonial logic

### **Dark Mode Colors Wrong**
- Check CSS variables are defined in both theme files
- Use `var(--variable-name, fallback)` format
- Clear browser cache

### **Build Errors**
```bash
# Clean build
bundle exec jekyll clean
bundle exec jekyll build

# Check for syntax errors
bundle exec jekyll build --verbose
```

### **Performance Issues**
- Check for large images in `/images/`
- Verify external scripts are conditionally loaded
- Use browser dev tools to identify bottlenecks

---

## **Development Workflow**

### **Local Development**
```bash
# Start development server
bundle exec jekyll serve --incremental

# With live reload
bundle exec jekyll serve --livereload

# Clean build
bundle exec jekyll clean && bundle exec jekyll build
```

### **Testing Changes**
1. Make changes to content/templates
2. Check local server: `http://localhost:4000`
3. Test both light and dark modes
4. Verify mobile responsiveness
5. Check browser console for errors

### **Content Updates**
1. Add/edit markdown files in appropriate directories
2. Jekyll auto-rebuilds with `--incremental`
3. Check formatting and links
4. Test testimonial display if adding teaching content

---

## **Deployment**

### **GitHub Pages**
1. Push changes to main branch
2. GitHub automatically builds and deploys
3. Check Actions tab for build status
4. Site updates at: `https://username.github.io`

### **Manual Deployment**
```bash
# Build for production
JEKYLL_ENV=production bundle exec jekyll build

# Deploy _site/ folder to hosting service
```

---

## **Best Practices**

### **Content**
- Use descriptive filenames for teaching/publication files
- Keep abstracts concise but informative
- Include all relevant links (PDF, slides, code, data)
- Update dates to reflect current information

### **Code**
- Always use CSS variables for colors
- Test changes in both light and dark modes
- Keep inline styles minimal
- Comment complex template logic

### **Performance**
- Optimize images before adding to `/images/`
- Avoid adding large external libraries
- Use conditional loading for scripts
- Monitor build times and site size

### **SEO**
- Use descriptive page titles
- Include meta descriptions
- Ensure proper heading structure (H1, H2, H3)
- Update sitemap when adding new pages

---

## **Maintenance Checklist**

### **Monthly**
- [ ] Check all external links work
- [ ] Update contact information if changed
- [ ] Review and update about page
- [ ] Check site performance with browser dev tools

### **Semester/Quarter**
- [ ] Add new teaching experiences
- [ ] Update course information
- [ ] Add new testimonials if received
- [ ] Update CV/resume information

### **Annually**
- [ ] Review and update all content
- [ ] Update Jekyll and dependencies: `bundle update`
- [ ] Review and optimize images
- [ ] Check Google Analytics (if implemented)
- [ ] Verify all social media links

---

## **Emergency Fixes**

### **Site Not Loading**
1. Check GitHub Actions for build errors
2. Verify _config.yml syntax
3. Check recent commits for breaking changes
4. Rollback to last working commit if needed

### **Quick Rollback**
```bash
# Find last working commit
git log --oneline

# Rollback
git reset --hard COMMIT_HASH
git push --force-with-lease origin main
```

### **Contact Support**
- GitHub Issues: Check repository issues
- Jekyll Documentation: https://jekyllrb.com/docs/
- Academic Pages Theme: https://github.com/academicpages/academicpages.github.io

---

**End of Guide**

For questions or updates to this guide, please update this file with new information as you discover it.
