---
title: "WRI's Global Nature Watch: Geospatial AI in Plain Language"
info: |
  Demo deck for Global Nature Watch (project-zeno): Daniel Wiesmann, Soumya Ranjan, Leo Thomas, Development Seed.
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

# WRI's Global Nature Watch

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

**Global Nature Watch** (formerly Global Forest Watch): nature monitoring designed for action.

* **The goal:** data, technology and tools that help people everywhere protect forests and natural ecosystems
* **Who uses it:** governments, NGOs, journalists and companies, for alerts, country statistics and supply chain risk
* **This demo:** an AI agent, built with WRI and the Land & Carbon Lab, that answers these questions in plain language. No GIS expertise needed

<div flex gap-6 mt-5 items-end justify-center>
  <figure m-0 text-center>
    <img src="/images/gnw/discover-forests.webp" h-44 mx-auto />
    <figcaption text-sm>Explore the world's forests</figcaption>
  </figure>
  <figure m-0 text-center>
    <img src="/images/gnw/deforestation-alerts.webp" h-44 mx-auto />
    <figcaption text-sm>See new tropical deforestation</figcaption>
  </figure>
  <figure m-0 text-center>
    <img src="/images/gnw/country-stats.webp" h-44 mx-auto />
    <figcaption text-sm>Get country statistics</figcaption>
  </figure>
</div>

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

<!-- Images are from globalnaturewatch.org (older Global Forest Watch interface). -->

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

- Peer-reviewed data from the Land & Carbon Lab and Global Forest Watch
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
layout: cover
# Copernicus Sentinel-2 image of Tanezrouft Basin, Sahara
background: /images/theme/Tanezrouft_Basin.jpg
dim: true
class: px-10
---

# Live demo

<p text-2xl mt-6>How much grassland was lost in Argentina during the last global beef price boom?</p>

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

---
layout: image-right
# Landsat 9 Image of Bangladesh Coast
# https://unsplash.com/photos/eGGENWtikd0
image: /images/theme/landsat9-bangladesh-coast.jpg
class: image-narrow
---

# Your turn

* Pose your own questions
* Watch the agent reason, tool call by tool call
* See the place, dataset and timeframe it picks
* See what happens when it cannot answer

<LogoHorNegMono position="bottom-right" />

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
