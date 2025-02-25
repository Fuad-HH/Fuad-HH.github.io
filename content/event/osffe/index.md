---
title: Integrating OpenMC and PUMIPic to enable GPU Acceleration of Unstructured Mesh Tallies for Monte Carlo Transport Simulations

event: Open Source Software for Fusion Energy (OSSFE) Conference
event_url: https://ossfe.github.io/

location: Gather Town (Virtual)


summary: Poster presentation on the integration of PUMIPic into OpenMC to enable GPU acceleration of unstructured mesh tallies for Monte Carlo transport simulations.
abstract: 'Open-source software will play an outsized role in enabling the national academies ambitious goal to bring fusion power to the grid by 2035. Due to the highly specialized nature of fusion simulations, the closed-source software model that drives traditional computer-aided engineering (CAE) software companies will not be effective at addressing the diverse set of needs. Instead, the fusion sector needs tools that can be rapidly modified to take advantage of novel mathematical formulations and an evolving understanding of underlying plasma and material physics. In recent years, the fusion community has made strides towards supporting CAD-based models that are not easily supported through hand-coded CSG trees in open-source transport simulations through tools such as OpenMC, DAGMC, and Degas2. However, fundamental challenges such as understanding neutral recycling, heating from 14MeV neutrons, and whole facility radiation dose calculations require new developments to support larger and more geometrically complex models. Although progress has been made in CAD based transport simulations using OpenMC with DAGMC, tallies on unstructured meshes remains a bottleneck. This talk addresses that bottleneck by integrating PUMIPic, a GPU accelerated library for hybrid particle and mesh calculations, into OpenMC. Using this approach, we demonstrate better scaling with both number of particles and number of elements on CPU and GPU based calculations on unstructured meshes.This talk highlights the effectiveness of building upon the burgeoning open-source ecosystem in support of fusion power applications.'

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
date: '2025-03-18T09:30:00Z'
date_end: '2025-03-18T10:40:00Z'
all_day: false

# Schedule page publish date (NOT talk date).
publishDate: '2025-02-18T09:30:00Z'

authors: [Fuad Hasan, George Wilke, Paul Romano, Patrick Shriwise, Michael Churchill, Cameron Smith, Jacob Merson]
tags: []

# Is this a featured talk? (true/false)
featured: true

links:
  - icon: website
    icon_pack: fab
    name: Abstract Link
    url: https://ossfe.github.io/OSSFE_2025/fuad-integrating
url_code: ''
url_pdf: ''
url_slides: ''
url_video: ''

# Markdown Slides (optional).
#   Associate this talk with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects:
  - []
---

