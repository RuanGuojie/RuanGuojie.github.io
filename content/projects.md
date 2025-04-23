---
title: 'Projects'
date: 2024-05-19
type: landing

design:
  spacing: '5rem'

sections:

  - block: markdown
    content:
      title: Sensor Map
      markdown: |
        {{< leafletmap >}}

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

