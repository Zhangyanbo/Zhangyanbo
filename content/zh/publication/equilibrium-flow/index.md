---
title: 'Equilibrium flow: From Snapshots to Dynamics'

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - admin
  - Michael Levin

date: '2025-09-22'

# Schedule page publish date (NOT publication's date).
publishDate: '2025-09-22T00:00:00Z'

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ['article']

# Publication name and optional abbreviated publication name.
publication: "ArXiv preprint"
publication_short: "ArXiv"

abstract: "Scientific data, from cellular snapshots in biology to celestial distributions in cosmology, often consists of static patterns from underlying dynamical systems. These snapshots, while lacking temporal ordering, implicitly encode the processes that preserve them. This work investigates how strongly such a distribution constrains its underlying dynamics and how to recover them. We introduce the Equilibrium flow method, a framework that learns continuous dynamics that preserve a given pattern distribution. Our method successfully identifies plausible dynamics for 2-D systems and recovers the signature chaotic behavior of the Lorenz attractor. For high-dimensional Turing patterns from the Gray-Scott model, we develop an efficient, training-free variant that achieves high fidelity to the ground truth, validated both quantitatively and qualitatively. Our analysis reveals the solution space is constrained not only by the data but also by the learning model's inductive biases. This capability extends beyond recovering known systems, enabling a new paradigm of inverse design for Artificial Life. By specifying a target pattern distribution, we can discover the local interaction rules that preserve it, leading to the spontaneous emergence of complex behaviors, such as life-like flocking, attraction, and repulsion patterns, from simple, user-defined snapshots."

# Summary. An optional shortened abstract.
summary: 

tags:
  - Diffusion Models
  - Chaos
  - Artificial Life
  - Dynamical Systems

# Display this page in the Featured widget?
featured: true

# Standard identifiers for auto-linking
hugoblox:
  ids:
    doi: 10.48550/arXiv.2509.17990

# Custom links
links:
  - type: pdf
    url: "https://arxiv.org/abs/2509.17990"
  - type: code
    url: "https://github.com/Zhangyanbo/pattern_to_dynamics"

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
