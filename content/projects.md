---
title: 'Projects'
date: 2024-05-19
type: landing

design:
  spacing: '5rem'

sections:

  # ✅ 地图部分
  - block: markdown
    content:
      title: Sensor Map
      markdown: |
        <iframe src="/maps/sensor-map/" width="100%" height="600px" style="border:none; display:block; min-height:600px;"></iframe>

  # ✅ 你原来的项目集部分
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


