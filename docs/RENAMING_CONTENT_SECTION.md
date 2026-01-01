# Renaming the Content Section

The content section (currently "Guides") can be renamed by updating the configuration in `_config.yml` and a few related files.

## Steps to Rename

### 1. Update `_config.yml`

Change the `content_section` values:

```yaml
content_section:
  name: "Experiments"           # Change from "Guides"
  name_singular: "Experiment"   # Change from "Guide"
  path: "experiments"           # Change from "guides"
```

### 2. Rename the directory

```bash
mv guides experiments
```

### 3. Rename the data file

```bash
mv _data/guides.yml _data/experiments.yml
```

### 4. Rename the layout file

```bash
mv _layouts/guides.html _layouts/experiments.html
```

### 5. Update `_config.yml` defaults

Change the `path` and `layout` in the defaults section:

```yaml
defaults:
  - scope:
      path: experiments      # Change from "guides"
      type: pages
    values:
      layout: experiments    # Change from "guides"
      search: true
```

### 6. Update `_data/nav.yml`

Change the navigation title and URL:

```yaml
- title: "Experiments"       # Change from "Guides"
  url: /experiments/         # Change from "/guides/"
```

### 7. Update internal links

Update any hardcoded links in markdown files:

```bash
find experiments -name "*.md" -type f -exec sed -i '' 's|/guides/|/experiments/|g' {} \;
```

### 8. Rebuild

```bash
bundle exec jekyll clean
bundle exec jekyll build
```

## What Uses the Config

The following files automatically use `site.content_section.*`:
- `_includes/nav.html` - For search action and placeholder text
- `_layouts/[content].html` - For search action and placeholder text

The following files need manual updates:
- `_data/nav.yml` - Navigation title and URL
- `_config.yml` defaults - Path and layout name
- Directory name
- Data file name
- Layout file name
