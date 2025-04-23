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
        <style>
            iframe { display: block !important; width: 100% !important; min-height: 600px !important; }
        </style>
        <iframe src="/maps/sensor-map/" width="100%" height="600px" style="border:none; min-height:600px; display:block;"></iframe>

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