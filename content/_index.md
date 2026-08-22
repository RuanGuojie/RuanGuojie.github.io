---
# Leave the homepage title empty to use the site title
title: ""
date: 2022-10-24
type: landing

design:
  # Default section spacing
  spacing: "6rem"

sections:
  - block: resume-biography-3
    content:
      # Choose a user profile to display (a folder name within `content/authors/`)
      username: admin
      text: ""
      # Show a call-to-action button under your biography? (optional)
      button:
        text: Download CV
        url: uploads/resume.pdf
    design:
      #Apply a gradient background
      css_class: hbx-bg-gradient
      avatar:
        size: medium # Picture size Options: small (150px), medium (200px, default), large (320px), xl (400px), xxl (500px)
        shape: circle # Options: circle (default), square, rounded
  - block: markdown
    content:
      title: ''
      subtitle: ''
      text: |
        <style>
        .research-intro-title{font-size:1.6rem;font-weight:600;margin:0 0 .4rem;}
        .research-intro-sub{font-size:1rem;color:var(--gray-600,#5f5e5a);line-height:1.6;margin:0 0 2rem;max-width:640px;}
        .research-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:14px;}
        .research-card{background:var(--card-bg,#fff);border:1px solid rgba(0,0,0,.08);border-radius:12px;padding:1.4rem 1.5rem;transition:box-shadow .2s ease,transform .2s ease;}
        .research-card:hover{box-shadow:0 4px 16px rgba(0,0,0,.08);transform:translateY(-2px);}
        .research-card .rc-title{font-size:1.05rem;font-weight:600;margin:0 0 .5rem;}
        .research-card .rc-desc{font-size:.9rem;color:var(--gray-600,#5f5e5a);line-height:1.6;margin:0;}
        .research-stats{display:flex;gap:3rem;margin-top:2rem;padding-top:1.5rem;border-top:1px solid rgba(0,0,0,.08);}
        .research-stats .rs-num{font-size:1.9rem;font-weight:600;line-height:1;margin:0;}
        .research-stats .rs-label{font-size:.8rem;color:var(--gray-500,#888);margin:.4rem 0 0;}
        @media (prefers-color-scheme: dark){
          .research-card{background:rgba(255,255,255,.03);border-color:rgba(255,255,255,.1);}
          .research-intro-sub,.research-card .rc-desc{color:#a3a3a3;}
        }
        </style>

        <div class="research-intro-title">My Research</div>
        <div class="research-intro-sub">Digital and smart agriculture for plant science — building trustworthy data and models to improve crop production and resilience to a changing climate.</div>

        <div class="research-grid">
          <div class="research-card">
            <div class="rc-title">Digital Soil Mapping</div>
            <p class="rc-desc">Geostatistical (EBK), machine-learning (RF, GWR), and hybrid (Regression Kriging) methods to predict soil health and fertility from multi-source sensing data (yield map, DEM, ECa, VIs).</p>
          </div>
          <div class="research-card">
            <div class="rc-title">Cross-scale Nutrient Mapping</div>
            <p class="rc-desc">In-season, cross-stage UAV–satellite corn leaf N, P, K estimation with spatial-temporal neural networks for variable-rate planting and fertilization.</p>
          </div>
          <div class="research-card">
            <div class="rc-title">Crop Phenology Monitoring</div>
            <p class="rc-desc">Vision-language object detection to track spatiotemporal variability of phenological stages (emergence rate and tasseling percentage).</p>
          </div>
          <div class="research-card">
            <div class="rc-title">Soil Sensor Dynamics</div>
            <p class="rc-desc">Linking the temporal dynamics of soil-sensor observations to soil hydrological properties.</p>
          </div>
          <div class="research-card">
            <div class="rc-title">3DGS Plant Reconstruction</div>
            <p class="rc-desc">3D Gaussian Splatting reconstruction of outdoor tomato plants using an autonomous navigation agricultural robot (UGV).</p>
          </div>
        </div>

        <div class="research-stats">
          <div><p class="rs-num">5</p><p class="rs-label">Publications</p></div>
          <div><p class="rs-num">164</p><p class="rs-label">Citations</p></div>
        </div>
    design:
      columns: '1'
  - block: collection
    id: papers
    content:
      title: Featured Publications
      filters:
        folders:
          - publication
        featured_only: true
    design:
      view: article-grid
      columns: 2
  - block: collection
    content:
      title: Recent Publications
      text: ""
      filters:
        folders:
          - publication
        exclude_featured: false
    design:
      view: citation
  - block: collection
    id: talks
    content:
      title: Recent & Upcoming Talks
      filters:
        folders:
          - event
    design:
      view: article-grid
      columns: 1
  - block: collection
    id: posters
    content:
      title: Recent posters
      subtitle: ''
      text: |
        <img src="/uploads/poster1.jpg"
          style="max-width: 100%; border-radius: 12px; margin-bottom: 1.5rem;">
        <img src="/uploads/poster2.jpg"
          style="max-width: 100%; border-radius: 12px; margin-bottom: 1.5rem;">
        <img src="/uploads/poster3.jpg"
          style="max-width: 100%; border-radius: 12px; margin-bottom: 1.5rem;">
        <img src="/uploads/poster4.jpg"
          style="max-width: 100%; border-radius: 12px; margin-bottom: 1.5rem;">
        <img src="/uploads/poster5.jpg"
          style="max-width: 100%; border-radius: 12px; margin-bottom: 1.5rem;">
      # Page type to display. E.g. post, talk, publication...
      page_type: blog
      # Choose how many pages you would like to display (0 = all pages)
      count: 5
      # Filter on criteria
      filters:
        author: ""
        category: ""
        tag: ""
        exclude_featured: false
        exclude_future: false
        exclude_past: false
        publication_type: ""
      # Choose how many pages you would like to offset by
      offset: 0
      # Page order: descending (desc) or ascending (asc) date.
      order: desc
    design:
      # Choose a layout view
      view: masonry
      # Reduce spacing
      spacing:
        padding: [0, 0, 0, 0]
  - block: cta-card
    demo: true # Only display this section in the Hugo Blox Builder demo site
    content:
      title: 👉 Build your own academic website like this
      text: |-
        This site is generated by Hugo Blox Builder - the FREE, Hugo-based open source website builder trusted by 250,000+ academics like you.

        <a class="github-button" href="https://github.com/HugoBlox/hugo-blox-builder" data-color-scheme="no-preference: light; light: light; dark: dark;" data-icon="octicon-star" data-size="large" data-show-count="true" aria-label="Star HugoBlox/hugo-blox-builder on GitHub">Star</a>

        Easily build anything with blocks - no-code required!
        
        From landing pages, second brains, and courses to academic resumés, conferences, and tech blogs.
      button:
        text: Get Started
        url: https://hugoblox.com/templates/
    design:
      card:
        # Card background color (CSS class)
        css_class: "bg-primary-700"
        css_style: ""
---
