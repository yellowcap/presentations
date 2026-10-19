---
title: "Global Nature Watch Horizon: Geospatial AI in Plain Language"
info: |
  Demo deck for Global Nature Watch Horizon (project-zeno): Daniel Wiesmann, Soumya Ranjan, Leo Thomas, Development Seed.
class: text-center
highlighter: shiki
drawings:
  persist: false
  enable: false
transition: slide-left
mdc: true
favicon: /meta/favicon.png
addons:
    - slidev-addon-qrcode

theme: './theme'
themeConfig:
  laserColor: '#CF3F02'
  laserWidth: 8
  laserFade: 1500
layout: title
image: /images/theme/landsat9-western-guinea-bissau.jpg
---

# Global Nature Watch Horizon

::subtitle::
Geospatial AI in Plain Language

<DecorativeRectangle
  width="50%"
  height="40%"
  zIndex=20
  :position="{
    bottom: '2%',
    right: '2%',
  }"
  :customStyle="{ mixBlendMode: 'multiply' }"
>
  <div w-full h-full relative flex flex-col items-end justify-end p-4 text-white text-right>
    <h3 text-3xl>
      Agentic AI for Earth Observation
    </h3>
    <h4 text-md font-mono>
      Berlin · 2026-10-19
    </h4>
    <h5 text-sm>
      Daniel Wiesmann · Soumya Ranjan · Leo Thomas
    </h5>
    <h5 text-sm>
      <code text-primary>Development Seed</code>
    </h5>
  </div>
</DecorativeRectangle>
<LogoHorPos position="top-left" height="24px" />

<!-- Agentic AI for Earth Observation Workshop, Berlin, 19 October 2026 (https://agentic-eo.berlin). Organised by BIFOLD and ESA. -->

---
layout: default
---

# What it is

**Global Nature Watch Horizon** is an open, AI-powered system for monitoring land and ecosystems. It is in preview.

* **The context:** Global Forest Watch is becoming **Global Nature Watch**, with monitoring that reaches beyond forests. Horizon is the AI platform preview of that vision
* **Who builds it:** the Land & Carbon Lab (Bezos Earth Fund and WRI) with Global Nature Watch, and Development Seed
* **How you use it:** ask in plain language. Get maps, charts and statistics. No GIS expertise needed
* **Live now:** anyone can try it at [horizon.globalnaturewatch.org](https://horizon.globalnaturewatch.org)

<div flex gap-4 mt-3 mb-2 items-end justify-center>
  <figure m-0 text-center>
    <img src="/images/gnw/horizon-1.webp" h-32 mx-auto />
    <figcaption text-sm>Forest loss in Acre, Brazil</figcaption>
  </figure>
  <figure m-0 text-center>
    <img src="/images/gnw/horizon-2.webp" h-32 mx-auto />
    <figcaption text-sm>Disturbance alerts in Sulawesi Barat</figcaption>
  </figure>
  <figure m-0 text-center>
    <img src="/images/gnw/horizon-3.webp" h-32 mx-auto />
    <figcaption text-sm>Brazil deforestation by president</figcaption>
  </figure>
</div>

<div text-xs opacity-70 text-center mb-2>Source: Global Nature Watch Horizon, horizon.globalnaturewatch.org</div>

<DecorativeRectangle
  width="18%"
  height="12%"
  zIndex=10
  :position="{
    top: '-3%',
    right: '-1em',
  }"
  :customStyle="{ mixBlendMode: 'multiply' }"
/>
<LogoHorPos position="bottom-left" height="24px" />

<!-- Images and wording are from horizon.globalnaturewatch.org (about page). The rebrand: Global Forest Watch is now Global Nature Watch; the AI preview is Global Nature Watch Horizon. -->

---
layout: image-left
# Landsat 9 Image of Taklimakan Desert, China
# https://unsplash.com/photos/KGzlTHjkyZM
image: /images/theme/landsat9-taklimakan-desert-china.jpg
class: image-narrow
---

# Four tool calls, one answer

1. **`pick_aoi`**: place name to geometry
2. **`pick_dataset`**: question to dataset, layer and dates
3. **`pull_data`**: statistics from the WRI analytics API
4. **`generate_insights`**: chart and short text, saved as an insight

> The model never reads pixels. Every number comes from an API.
<LogoHorNegMono position="bottom-left" />

---
layout: two-cols
gap: 8
---

# How it is built

<div mt-5 />

## The agent

- A ReAct agent on LangChain and LangGraph
- LLMs from Google and Anthropic
- **Tools**, **subagents**, **skills** and **state**
- Analysis code written and run with a CodeAct pattern

::right::

<div mt-5 />

## The platform

- Peer-reviewed data from the Land & Carbon Lab and Global Nature Watch (formerly Global Forest Watch)
- Served with eoAPI and STAC
- Next.js frontend, built for accessibility and many languages
- Every run traced, with user feedback

<LogoHorPos position="bottom-right" height="24px" />

<DecorativeRectangle
  width="30%"
  height="20%"
  zIndex=10
  :position="{
    bottom: '2%',
    left: '-1em',
  }"
  :customStyle="{ mixBlendMode: 'multiply' }"
/>

---
layout: two-cols
leftRatio: 40
gap: 6
---

# Verifiable by design

<div mt-5 />

* The agent <span v-mark.highlight.orange>never invents numbers</span>. It writes code and runs it on the pulled data (CodeAct)
* **Show working** opens the generation record: step-numbered code, outputs and sources
* Save the record to reproduce or audit the analysis
* Evals and user feedback drive refinement

::right::

<div flex gap-3 mt-4 items-start justify-center>
  <figure m-0 text-center>
    <img src="/images/gnw/analysis-panel.png" h-80 mx-auto />
    <figcaption text-sm>Every chart has "Show working"</figcaption>
  </figure>
  <figure m-0 text-center>
    <img src="/images/gnw/generation-record.png" h-80 mx-auto />
    <figcaption text-sm>The generation record</figcaption>
  </figure>
</div>

<LogoHorPos position="bottom-left" height="24px" />

<DecorativeRectangle
  width="18%"
  height="12%"
  zIndex=10
  :position="{
    top: '-3%',
    right: '-1em',
  }"
  :customStyle="{ mixBlendMode: 'multiply' }"
/>

<!--
Click "Show working" under a chart to open the generation record: code blocks and execution outputs are numbered, sources are numbered, and "Save record" downloads a Markdown file.
Screenshots: Global Nature Watch Horizon preview, tree cover loss in Para, Brazil.
-->

---
layout: image-left
# Landsat 9 Image of Apostle Islands, Lake Superior
image: /images/theme/landsat9-apostle-islands-lake-superior.jpg
class: image-narrow
---

# Fully open source

* Agent and API: `github.com/wri/project-zeno`
* Frontend: `github.com/wri/project-zeno-next`
* Infrastructure: `github.com/wri/project-zeno-deploy`
* Evaluation framework: `github.com/wri/gnw-evals`

<DecorativeRectangle
  width="23em"
  height="6rem"
  zIndex=10
  :position="{
    bottom: '2%',
    right: '-1em',
  }"
/>
<LogoHorNegMono position="bottom-left" />

---
layout: cover
# Copernicus Sentinel-2 image of Tanezrouft Basin, Sahara
background: /images/theme/Tanezrouft_Basin.jpg
dim: true
class: px-10
---

# Live demo

<p text-2xl mt-6>How much grassland was lost in Argentina during the last global beef price boom?</p>

<div flex items-center gap-8 mt-10 text-left>
  <QRCode data="https://horizon.globalnaturewatch.org" :width="140" :height="140" :dotsOptions="{ type: 'classy-rounded', color: 'white' }" />
  <div>
    <p text-xl m-0><b>Try it yourself.</b> It is live and open to everyone.</p>
    <p text-xl mt-2 mb-0><code text-primary>horizon.globalnaturewatch.org</code></p>
  </div>
</div>

<DecorativeRectangle
  width="30%"
  height="18%"
  zIndex=10
  :position="{
    bottom: '-2%',
    left: '-1em',
  }"
/>
<LogoHorNegMono position="bottom-right" />

<!--
Backup prompts to try:
- How much tree cover was lost in Odisha in the last 5 years?
- What can you do, and what data do you have?
- Show me tree cover loss in Mato Grosso for EUDR compliance.
- Now compare it with Para.
Use a new chat thread for each one.
-->
