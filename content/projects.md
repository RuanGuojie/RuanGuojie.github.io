---
title: 'Projects'
date: 2024-05-19
type: landing

design:
  spacing: '5rem'

sections:

  # ✅ 直接嵌入 HTML，不依赖 markdown
  - block: html
    content: |
      <section>
        <h2 style="text-align: center;">Sensor Map</h2>
        <iframe src="/maps/sensor-map/" width="100%" height="600px" style="border:none; display:block;"></iframe>
      </section>

  # ✅ 你的项目展示 block
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


