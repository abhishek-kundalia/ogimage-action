# OG Image Generator - GitHub Action

Generate beautiful OG images for your docs, blog, or static site automatically in your CI/CD pipeline.

## Usage

```yaml
- uses: abhishek-kundalia/ogimage-action@v1
  with:
    title: 'My Blog Post Title'
    subtitle: 'A brief description'
    template: 'gradient'
    output: 'public/og-image.png'
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `title` | Title text for the image | Yes | - |
| `subtitle` | Subtitle text | No | `''` |
| `template` | Template style | No | `gradient` |
| `size` | Output size | No | `og` |
| `font` | Font style | No | `modern` |
| `output` | Output file path | No | `og-image.png` |
| `license-key` | License key (removes watermark) | No | `''` |
| `lite` | Enable lite mode for smaller files | No | `false` |

### Templates

`gradient`, `dark`, `sunset`, `ocean`, `forest`, `midnight`, `fire`, `minimal`, `card`, `glass`, `stripe`

### Sizes

`og` (1200×630), `twitter` (1200×675), `instagram` (1080×1080), `youtube` (1280×720), `pinterest` (1000×1500)

### Fonts

`modern`, `elegant`, `bold`, `mono`

## Examples

### Generate for each blog post

```yaml
name: Generate OG Images
on:
  push:
    paths:
      - 'content/blog/**'

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Generate OG Image
        uses: abhishek-kundalia/ogimage-action@v1
        with:
          title: ${{ github.event.head_commit.message }}
          template: dark
          output: public/og/${{ github.sha }}.png
          
      - name: Commit image
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add public/og/
          git commit -m "Generate OG image"
          git push
```

### Generate for documentation

```yaml
- uses: abhishek-kundalia/ogimage-action@v1
  with:
    title: 'API Reference'
    subtitle: 'Complete documentation for our REST API'
    template: stripe
    font: mono
    output: docs/static/og-api.png
    license-key: ${{ secrets.OGIMAGE_KEY }}
```

### Generate multiple images

```yaml
- name: Generate OG images
  run: |
    pages=("Home" "Features" "Pricing" "Blog")
    for page in "${pages[@]}"; do
      curl -s "https://ogimage.art/api/og?title=${page}&template=gradient" -o "public/og/${page,,}.png"
    done
```

## Outputs

| Output | Description |
|--------|-------------|
| `image-path` | Path to the generated image |
| `image-url` | URL to generate the image dynamically |

## License

MIT - Get a license key at [ogimage.art](https://ogimage.art) to remove watermarks.
