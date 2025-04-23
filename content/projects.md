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
      text: |
        {{% sensor-map %}}

  - block: collection
    content:
      title: Selected Projects
      filters:
        folders:
          - project
    design:
      view: article-grid
      columns: 3
---