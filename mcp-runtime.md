---
title: "Tool data, handled in middleware: the MCP runtime"
info: |
  How the geo assistant's MCP runtime keeps large tool results out of the model's context. Development Seed Big Tent. Daniel Wiesmann and Ciaran Sweet, Development Seed.
class: text-center
highlighter: shiki
# Hash URLs (#/4), so a link to any slide works on GitHub Pages. The site has one 404.html, and it belongs to slides.md.
routerMode: hash
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
# Landsat 8 image of the Ord River in Australia
# https://unsplash.com/photos/2BThgnOYoIc
image: /images/theme/landsat8-ord-river-australia.jpg
---

# Tool data, handled in middleware

::subtitle::
The geo assistant's MCP runtime

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
      Big Tent
    </h3>
    <h5 text-sm>
      Daniel Wiesmann · Ciaran Sweet
    </h5>
    <h5 text-sm>
      <code text-primary>Development Seed</code>
    </h5>
  </div>
</DecorativeRectangle>
<LogoHorPos position="top-left" height="24px" />

---
layout: two-cols
leftRatio: 55
gap: 8
class: not-eoapi
---

# This is not eoAPI

<p class="ne-but">…but right now, it makes creating agents easy</p>

- **Agents move fast.** This can be obsolete in 6 months
- **Distilled knowledge** from our projects. It has been useful until now
- **AI first.** Agents understand the system, so they can make new agents quickly

::right::

<div mt-5 />

## Ported so far

<div class="ne-apps">
  <span>CDS Assistant</span>
  <span>Ask Monty</span>
  <span>Geo Assistant</span>
  <span>openEO Studio</span>
</div>

## External validation

<div class="ne-apps ne-ext">
  <span>LandOS is adopting it</span>
</div>

<LogoHorPos position="bottom-right" height="20px" />

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
layout: default
class: four-in-one
---

# Four things in one

<div class="fio-grid">
  <div class="fio-tile">
    <div class="fio-n">1</div>
    <div class="fio-what">Python functions become <b>MCP servers</b></div>
    <code>mcp_runtime</code>
  </div>
  <div class="fio-tile">
    <div class="fio-n">2</div>
    <div class="fio-what">An <b>agent</b> that handles state and provenance</div>
    <code>mcp_agent · mcp_state</code>
  </div>
  <div class="fio-tile">
    <div class="fio-n">3</div>
    <div class="fio-what">A <b>REST API</b> hooked into the agent</div>
    <code>mcp_agent_api</code>
  </div>
  <div class="fio-tile">
    <div class="fio-n">4</div>
    <div class="fio-what">A default <b>web frontend</b></div>
    <code>the bundled web client</code>
  </div>
</div>

<p class="fio-tagline">Write your tools. Get everything else included.</p>

<LogoHorPos position="bottom-right" height="20px" />

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
One package, mcp-toolsets-runtime, with extras: [state], [agent], [api].
1. A toolset is a list of LangChain @tool functions; mcp-serve serves them over MCP. (Plain functions without @tool: issue #195, open.)
2. mcp_agent drives the tools, with mcp_state wired in: session state, inspect_state, receipts.
3. mcp_agent_api: the agent over HTTP, as AG-UI events plus read routes.
4. The [api] extra also serves a web chat: transcript, tool views, the state panel and receipts.
-->

---
layout: two-cols
gap: 8
---

# Big tool results, small context

<div mt-5 />

## The problem

- A tool returns a polygon or a list of places: **thousands of tokens**
- The model must copy it into the next call, and **can change it**
- Afterwards, it is hard to tell what a call ran on

::right::

<div mt-5 />

## The idea

- Keep large values in **State**, outside the model's context
- Give the model a short <span class="ref-pill">@reference</span>
- Swap the real value back in **before the call goes out**
- Do this once, in the client, for **any** MCP server

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

<!--
Two jobs in the middleware: Split publishes a large result to State, Inject consumes a reference and puts the value back.
Next: six scenes with one question. Each scene loops by itself.
-->

---
layout: default
class: mcp-scene
---

<div class="scene-n">Scene 1 of 6</div>

# The user asks, the agent calls AOI Search

<MiddlewareFlow :scene="1" />

<p class="scene-cap">The agent looks up the Taj Mahal. There is no reference yet, so the call goes straight to the tool.</p>

<div class="legend"><span class="ref-pill">@ref</span> reference · <img src="/images/mcp-runtime/poly.svg" /> area data · <img src="/images/mcp-runtime/pts.svg" /> places data</div>

<LogoHorPos position="bottom-right" height="20px" />

<!--
Question: "How many coffee shops near the Taj Mahal?" AOI Search is one box here. In the real toolset it is two tools: get_place, then get_search_area.
-->

---
layout: default
class: mcp-scene
---

<div class="scene-n">Scene 2 of 6</div>

# Split: the data goes to State, the agent gets <span class="ref-pill">@taj_mahal</span>

<MiddlewareFlow :scene="2" />

<p class="scene-cap">The polygon is big. Split publishes it to State and gives the agent a short reference. The model never sees the polygon.</p>

<div class="legend"><span class="ref-pill">@ref</span> reference · <img src="/images/mcp-runtime/poly.svg" /> area data · <img src="/images/mcp-runtime/pts.svg" /> places data</div>

<LogoHorPos position="bottom-right" height="20px" />

<!--
Split publishes. The agent now holds @taj_mahal, about ten tokens instead of the full geometry.
-->

---
layout: default
class: mcp-scene
---

<div class="scene-n">Scene 3 of 6</div>

# The agent passes on only <span class="ref-pill">@taj_mahal</span>

<MiddlewareFlow :scene="3" />

<p class="scene-cap">To find coffee shops, the agent writes the reference into the next call. It does not copy the polygon.</p>

<div class="legend"><span class="ref-pill">@ref</span> reference · <img src="/images/mcp-runtime/poly.svg" /> area data · <img src="/images/mcp-runtime/pts.svg" /> places data</div>

<LogoHorPos position="bottom-right" height="20px" />

<!--
The model chooses which value to use. It names it, it does not write it.
-->

---
layout: default
class: mcp-scene
---

<div class="scene-n">Scene 4 of 6</div>

# Inject swaps <span class="ref-pill">@taj_mahal</span> for the data

<MiddlewareFlow :scene="4" />

<p class="scene-cap">Before the call goes out, Inject reads State and puts the real polygon in. The tool gets a plain value.</p>

<div class="legend"><span class="ref-pill">@ref</span> reference · <img src="/images/mcp-runtime/poly.svg" /> area data · <img src="/images/mcp-runtime/pts.svg" /> places data</div>

<LogoHorPos position="bottom-right" height="20px" />

<!--
Inject consumes. Places Search never sees a reference and never sees State.
-->

---
layout: default
class: mcp-scene
---

<div class="scene-n">Scene 5 of 6</div>

# Split again: the places go to State, the agent gets <span class="ref-pill">@coffee_shops</span>

<MiddlewareFlow :scene="5" />

<p class="scene-cap">The places list is big too. It goes to State. The agent gets one more reference.</p>

<div class="legend"><span class="ref-pill">@ref</span> reference · <img src="/images/mcp-runtime/poly.svg" /> area data · <img src="/images/mcp-runtime/pts.svg" /> places data</div>

<LogoHorPos position="bottom-right" height="20px" />

<!--
Same mechanism, second tool. Nothing in the tool knows about it.
-->

---
layout: default
class: mcp-scene
---

<div class="scene-n">Scene 6 of 6</div>

# The agent inspects <span class="ref-pill">@coffee_shops</span> and answers

<MiddlewareFlow :scene="6" />

<p class="scene-cap">The agent can look inside a reference when it needs to. It counts the places without reading every one, and answers.</p>

<div class="legend"><span class="ref-pill">@ref</span> reference · <img src="/images/mcp-runtime/poly.svg" /> area data · <img src="/images/mcp-runtime/pts.svg" /> places data</div>

<LogoHorPos position="bottom-right" height="20px" />

<!--
A reference is not a black box: the agent can inspect it. 12 is a sample number.
-->

---
layout: two-cols
leftRatio: 55
gap: 6
---

# In the code

<div mt-3 />

What the model sees in the real geo assistant:

```text
get_search_area(place="@state:duckdb-analyst/get_place/place")
→ Search area created: 1.0 km around Taj Mahal.
  [state updated: duckdb-analyst/get_search_area/search_area]

places_within_area(category="cafe",
  area="@state:duckdb-analyst/get_search_area/search_area")
```

A key is `<toolset>/<tool>/<field>`: the only thing two toolsets share.

::right::

<div mt-12 />

| In the story | In `mcp_state` |
| --- | --- |
| Split | `StateCaptureMiddleware` |
| Inject | `bind_injected` |
| `@taj_mahal` | `@state:<key>` |
| State | `tool_state` |
| inspect | `inspect_state` |

`NotAuthored` on a parameter: the model **cannot** write the value, only a reference. So it cannot invent a polygon.

<LogoHorPos position="bottom-right" height="20px" />

<!--
Real names from developmentseed/mcp-toolsets-runtime (mcp_state) and developmentseed/geo-assistant-mcp-toolset (duckdb-analyst toolset).
get_place publishes duckdb-analyst/get_place/place; get_search_area and places_within_area take NotAuthored parameters.
The Taj Mahal example and the counts are illustrative.
-->

---
layout: image-left
# Landsat 9 image of Apostle Islands, Lake Superior
# https://unsplash.com/photos/j7HqdQqn7Jo
image: /images/theme/landsat9-apostle-islands-lake-superior.jpg
class: image-narrow
---

# Why it matters

* **Modular.** Each toolset is a standalone MCP server. It sees only plain values: no references, no State
* **Any client.** The same servers work in Claude.ai or ChatGPT, just without the savings
* **Not a black box.** The agent can inspect a value, also as it was at an earlier turn
* **Traceable.** Each injected value leaves a receipt that names the tool that made it

<div mt-4 text-sm font-mono>

github.com/developmentseed/mcp-toolsets-runtime<br>
github.com/developmentseed/geo-assistant-mcp-toolset

</div>

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

<!--
All of this is client-side work. Unmodified third-party MCP servers still take part: large returns are captured on size, and structured parameters accept an @state handle.
Read more: docs/SESSION-STATE.md in mcp-toolsets-runtime.
-->

---
layout: default
class: tag-team
---

# Tag team

<div class="tt-court">
  <div class="tt-player tt-ciaran">
    <div class="tt-avatar">CS</div>
    <div class="tt-name">Ciaran</div>
    <div class="tt-role">builds the runtime</div>
    <div class="tt-stat"><b>81</b> PRs merged</div>
  </div>

  <div class="tt-middle">
    <div class="tt-ball-track"><span class="tt-ball">@ref</span></div>
    <div class="tt-pass tt-right"><span>v0.8.0: a name is the contract</span><i>→</i><span>toolset moves to <code>NotAuthored</code></span></div>
    <div class="tt-pass tt-left"><span>"ask the user" in any toolset</span><i>←</i><span>interrupt tool</span></div>
    <div class="tt-pass tt-right"><span>v0.12.0 released</span><i>→</i><span>toolset upgrades two days later</span></div>
  </div>

  <div class="tt-player tt-daniel">
    <div class="tt-avatar">DW</div>
    <div class="tt-name">Daniel</div>
    <div class="tt-role">builds the geo assistant on it</div>
    <div class="tt-stat"><b>8</b> toolset PRs · <b>3</b> runtime PRs</div>
  </div>
</div>

<p class="tt-foot">100 days · 49 releases · two people, each with a coding agent, meeting in pull requests</p>

<LogoHorPos position="bottom-right" height="20px" />

<!--
mcp-toolsets-runtime (Ciaran) and geo-assistant-mcp-toolset (Daniel), 28 July to 5 November 2026. Merged PRs only.
Hand-offs: runtime v0.8.0 (#99, 25 Aug) and toolset #1 the same day; Daniel's interrupt tool (#147, 18 Sep) and Ciaran's toolset questions (#201, 20 Oct); runtime v0.12.0 (20 Oct) and toolset #8 (22 Oct).
-->

---
layout: cover
# Copernicus Sentinel-2 image of Tanezrouft Basin, Sahara
background: /images/theme/Tanezrouft_Basin.jpg
dim: true
class: px-10
---

# Thank you

<p text-2xl mt-6>Daniel Wiesmann · Ciaran Sweet</p>

<div flex items-center gap-8 mt-10 text-left>
  <QRCode data="https://github.com/developmentseed/mcp-toolsets-runtime" :width="140" :height="140" :dotsOptions="{ type: 'classy-rounded', color: 'white' }" />
  <div>
    <p text-xl m-0><b>Try it.</b> The runtime is open source.</p>
    <p text-xl mt-2 mb-0><code text-primary>developmentseed/mcp-toolsets-runtime</code></p>
  </div>
</div>

<LogoHorNegMono position="bottom-right" />

