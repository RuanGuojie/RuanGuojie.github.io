---
title: 'Projects'
date: 2024-05-19
type: landing

design:
  # Section spacing
  spacing: '5rem'

# Page sections
sections:

  - block: markdown
    content:
      title: Sensor Map
      markdown: |
        {{< rawhtml >}}
        <iframe src="/maps/sensor-map.html" width="100%" height="600px" style="border: none;"></iframe>
        {{< /rawhtml >}}

  - block: collection
    content:
      title: Selected Projects
      text: I enjoy making things. Here are a selection of projects that I have worked on over the years.
      filters:
        folders:
          - project
    design:
      view: article-grid
      fill_image: false
      columns: 3
---

