---
title: 'Diffusion Models are Evolutionary Algorithms'

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - admin
  - Benedikt Hartl
  - Hananel Hazan
  - Michael Levin

# Author notes (optional)
author_notes:
  - 'Equal contribution'
  - 'Equal contribution'
  - 'Equal contribution'

date: '2025-01-22'

# Schedule page publish date (NOT publication's date).
publishDate: '2025-01-22T00:00:00Z'

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ['paper-conference']

# Publication name and optional abbreviated publication name.
publication: In *The Thirteenth International Conference on Learning Representations*
publication_short: In *ICLR 2025*

abstract: "In a convergence of machine learning and biology, we reveal that diffusion models are evolutionary algorithms. By considering evolution as a denoising process and reversed evolution as diffusion, we mathematically demonstrate that diffusion models inherently perform evolutionary algorithms, naturally encompassing selection, mutation, and reproductive isolation. Building on this equivalence, we propose the Diffusion Evolution method: an evolutionary algorithm utilizing iterative denoising -- as originally introduced in the context of diffusion models -- to heuristically refine solutions in parameter spaces. Unlike traditional approaches, Diffusion Evolution efficiently identifies multiple optimal solutions and outperforms prominent mainstream evolutionary algorithms. Furthermore, leveraging advanced concepts from diffusion models, namely latent space diffusion and accelerated sampling, we introduce Latent Space Diffusion Evolution, which finds solutions for evolutionary tasks in high-dimensional complex parameter space while significantly reducing computational steps. This parallel between diffusion and evolution not only bridges two different fields but also opens new avenues for mutual enhancement, raising questions about open-ended evolution and potentially utilizing non-Gaussian or discrete diffusion models in the context of Diffusion Evolution."

# Summary. An optional shortened abstract.
summary: Diffusion models are inherently evolutionary algorithms, performing selection, mutation, and reproductive isolation.

tags:
  - Diffusion Models
  - Evolutionary Algorithms

# Display this page in the Featured widget?
featured: true

# Standard identifiers for auto-linking
hugoblox:
  ids:
    doi: 10.5555/123456

# Custom links
links:
  - type: pdf
    url: "https://openreview.net/pdf?id=xVefsBbG2O"
  - type: code
    url: "https://github.com/Zhangyanbo/diffusion-evolution"

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  focal_point: ''
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: ""
---

<!-- {{% callout note %}}
Click the _Cite_ button above to demo the feature to enable visitors to import publication metadata into their reference management software.
{{% /callout %}}

{{% callout note %}}
Create your slides in Markdown - click the _Slides_ button to check out the example.
{{% /callout %}} -->

<!-- Add the publication's **full text** or **supplementary notes** here. You can use rich formatting such as including [code, math, and images](https://docs.hugoblox.com/content/writing-markdown-latex/). -->
