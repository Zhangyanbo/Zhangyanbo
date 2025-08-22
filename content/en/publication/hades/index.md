---
title: "Heuristically Adaptive Diffusion-Model Evolutionary Strategy"
authors:
- Benedikt Hartl
- admin
- Hananel Hazan
- Michael Levin

author_notes:
  - 'Equal contribution'
  - 'Equal contribution'
  - 'Equal contribution'
  
date: "2024-11-20T00:00:00Z"

# Schedule page publish date (NOT publication's date).
# publishDate: "2025-01-01T00:00:00Z"

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ["article"]

# Publication name and optional abbreviated publication name.
publication: ""
publication_short: ""

abstract: "Diffusion Models represent a significant advancement in generative modeling, employing a dual-phase process that first degrades domain-specific information via Gaussian noise and restores it through a trainable model. This framework enables pure noise-to-data generation and modular reconstruction of, images or videos. Concurrently, evolutionary algorithms employ optimization methods inspired by biological principles to refine sets of numerical parameters encoding potential solutions to rugged objective functions. Our research reveals a fundamental connection between diffusion models and evolutionary algorithms through their shared underlying generative mechanisms: both methods generate high-quality samples via iterative refinement on random initial distributions. By employing deep learning-based diffusion models as generative models across diverse evolutionary tasks and iteratively refining diffusion models with heuristically acquired databases, we can iteratively sample potentially better-adapted offspring parameters, integrating them into successive generations of the diffusion model. This approach achieves efficient convergence toward high-fitness parameters while maintaining explorative diversity. Diffusion models introduce enhanced memory capabilities into evolutionary algorithms, retaining historical information across generations and leveraging subtle data correlations to generate refined samples. We elevate evolutionary algorithms from procedures with shallow heuristics to frameworks with deep memory. By deploying classifier-free guidance for conditional sampling at the parameter level, we achieve precise control over evolutionary search dynamics to further specific genotypical, phenotypical, or population-wide traits. Our framework marks a major heuristic and algorithmic transition, offering increased flexibility, precision, and control in evolutionary optimization processes."

# Summary. An optional shortened abstract.
summary: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis posuere tellus ac convallis placerat. Proin tincidunt magna sed ex sollicitudin condimentum.

tags:
- Evolutionary Algorithms
- Diffusion Models

featured: false

hugoblox:
  ids:
    arxiv: 2411.13420v1

links:
- type: code
  url: https://github.com/bhartl/CondEvo

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/s9CC2SKySJM)'
  focal_point: ""
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
<!-- 
This work is driven by the results in my [previous paper](/publication/conference-paper/) on LLMs.

{{% callout note %}}
Create your slides in Markdown - click the *Slides* button to check out the example.
{{% /callout %}}

Add the publication's **full text** or **supplementary notes** here. You can use rich formatting such as including [code, math, and images](https://docs.hugoblox.com/content/writing-markdown-latex/). -->
