---
layout: distill
title: What is the Drug Discovery
description: 
date: 2025-07-07
future: true
htmlwidgets: true

# Anonymize when submitting
# authors:
#   - name: Anonymous
#     affiliations:
#       name: Anonymous

authors:
 - name: Yuchao Lin
   url: "https://scholar.google.com/citations?user=TRmx8P0AAAAJ&hl=en"
   affiliations:
     name: Lambda Labs

# must be the exact same name as your blogpost
bibliography: 2024-05-07-understanding-icl.bib

# Add a table of contents to your post.
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - please use this format rather than manually creating a markdown table of contents.
toc:
  - name: Pipeline of Drug Discovery
  - name: Where Machine Learning Intervenes
  - name: 


# Below is an example of injecting additional post-specific styles.
# This is used in the 'Layouts' section of this post.
# If you use this post as a template, delete this _styles block.
_styles: >
  
  .center {
      display: block;
      margin-left: auto;
      margin-right: auto;
  }

  .framed {
    border: 1px var(--global-text-color) dashed !important;
    padding: 20px;
  }

  d-article {
    overflow-x: visible;
  }

  .underline {
    text-decoration: underline;
  }

  .todo{
      display: block;
      margin: 12px 0;
      font-style: italic;
      color: red;
  }
  .todo:before {
      content: "TODO: ";
      font-weight: bold;
      font-style: normal;
  }
  summary {
    color: steelblue;
    font-weight: bold;
  }

  summary-math {
    text-align:center;
    color: black
  }

  [data-theme="dark"] summary-math {
    text-align:center;
    color: white
  }

  details[open] {
  --bg: #e2edfc;
  color: black;
  border-radius: 15px;
  padding-left: 8px;
  background: var(--bg);
  outline: 0.5rem solid var(--bg);
  margin: 0 0 2rem 0;
  font-size: 80%;
  line-height: 1.4;
  }

  [data-theme="dark"] details[open] {
  --bg: #112f4a;
  color: white;
  border-radius: 15px;
  padding-left: 8px;
  background: var(--bg);
  outline: 0.5rem solid var(--bg);
  margin: 0 0 2rem 0;
  font-size: 80%;
  }
  .box-note, .box-warning, .box-error, .box-important {
    padding: 15px 15px 15px 10px;
    margin: 20px 20px 20px 5px;
    border: 1px solid #eee;
    border-left-width: 5px;
    border-radius: 5px 3px 3px 5px;
  }
  d-article .box-note {
    background-color: #eee;
    border-left-color: #2980b9;
  }
  d-article .box-warning {
    background-color: #fdf5d4;
    border-left-color: #f1c40f;
  }
  d-article .box-error {
    background-color: #f4dddb;
    border-left-color: #c0392b;
  }
  d-article .box-important {
    background-color: #d4f4dd;
    border-left-color: #2bc039;
  }
  html[data-theme='dark'] d-article .box-note {
    background-color: #555555;
    border-left-color: #2980b9;
  }
  html[data-theme='dark'] d-article .box-warning {
    background-color: #7f7f00;
    border-left-color: #f1c40f;
  }
  html[data-theme='dark'] d-article .box-error {
    background-color: #800000;
    border-left-color: #c0392b;
  }
  html[data-theme='dark'] d-article .box-important {
    background-color: #006600;
    border-left-color: #2bc039;
  }
  d-article aside {
    border: 1px solid #aaa;
    border-radius: 4px;
    padding: .5em .5em 0;
    font-size: 90%;
  }
  .caption { 
    font-size: 80%;
    line-height: 1.2;
    text-align: left;
  }
---

<div style="display: none">
$$
\definecolor{input}{rgb}{0.42, 0.55, 0.74}
\definecolor{params}{rgb}{0.51,0.70,0.40}
\definecolor{output}{rgb}{0.843, 0.608, 0}
\def\mba{\boldsymbol a}
\def\mbb{\boldsymbol b}
\def\mbc{\boldsymbol c}
\def\mbd{\boldsymbol d}
\def\mbe{\boldsymbol e}
\def\mbf{\boldsymbol f}
\def\mbg{\boldsymbol g}
\def\mbh{\boldsymbol h}
\def\mbi{\boldsymbol i}
\def\mbj{\boldsymbol j}
\def\mbk{\boldsymbol k}
\def\mbl{\boldsymbol l}
\def\mbm{\boldsymbol m}
\def\mbn{\boldsymbol n}
\def\mbo{\boldsymbol o}
\def\mbp{\boldsymbol p}
\def\mbq{\boldsymbol q}
\def\mbr{\boldsymbol r}
\def\mbs{\boldsymbol s}
\def\mbt{\boldsymbol t}
\def\mbu{\boldsymbol u}
\def\mbv{\boldsymbol v}
\def\mbw{\textcolor{params}{\boldsymbol w}}
\def\mbx{\textcolor{input}{\boldsymbol x}}
\def\mby{\boldsymbol y}
\def\mbz{\boldsymbol z}
\def\mbA{\boldsymbol A}
\def\mbB{\boldsymbol B}
\def\mbE{\boldsymbol E}
\def\mbH{\boldsymbol{H}}
\def\mbK{\boldsymbol{K}}
\def\mbP{\boldsymbol{P}}
\def\mbR{\boldsymbol{R}}
\def\mbW{\textcolor{params}{\boldsymbol W}}
\def\mbQ{\boldsymbol{Q}}
\def\mbV{\boldsymbol{V}}
\def\mbtheta{\textcolor{params}{\boldsymbol \theta}}
\def\mbzero{\boldsymbol 0}
\def\mbI{\boldsymbol I}
\def\cF{\mathcal F}
\def\cH{\mathcal H}
\def\cL{\mathcal L}
\def\cM{\mathcal M}
\def\cN{\mathcal N}
\def\cX{\mathcal X}
\def\cY{\mathcal Y}
\def\cU{\mathcal U}
\def\bbR{\mathbb R}
\def\y{\textcolor{output}{y}}
$$
</div>


***Drug discovery*** is the process by which new candidate medications are discovered. Historically the drug discovery pipeline has relied on screening of large compound libraries followed by years of chemical optimization and clinical testing; success rates remain low and average costs can surpass billions of dollars. The recent advancement of machine learning contributes to substantially **compressing the search space of candidate drugs** and so that reduces the number of compounds that must be synthesized and the corresponding time and budget, though machine learning **does not** shorten the clinical phase time and regulatory requirements.

## Pipeline of Drug Discovery 

An industrial pipeline of drug discovery is commonly partitioned into **four** phases:

- *Early drug discovery* begins by identifying a biological ***target*** (often a protein) whose altered activity is believed to influence a disease, and then searches chemical space for ***hits***, compounds that modulate that target by high‑throughput robotic assays and computer‑based ***virtual screening*** with thousands to millions of molecules. Following this, medicinal chemists refine each hit’s structure to improve potency, selectivity, solubility, and preliminary safety in simple laboratory assays, yielding only a few hundred well‑characterized ***lead*** compounds.
- *Pre-clinical phase* further refines and optimizes the candidate leads and conducts experiments *in vitro* (cell tests) or *in vivo* (animal tests). Compounds that fail on efficacy, bioavailability, or safety are discarded, so only several can survive and earn the data package needed to justify first‑in‑human trials.
- *Clinical phase* unfolds in three numbered trials for the drugs from pre-clinical phase: *Phase I*, typically a small group of healthy volunteers, establishes a safe dose range and short‑term side‑effect profile; *Phase II*, first patients, provides the first evidence of efficacy; *Phase III*, a larger group of patients, confirms efficacy across varied populations and tracks less common adverse events, producing the complete risk–benefit dataset needed for marketing approval.
- *Regulatory review* compiles all laboratory, animal, and human data into a *New Drug Application* (NDA) for small molecules or a *Biologics License Application* (BLA) for biologic therapies; agencies such as the FDA inspect raw data, and judge whether therapeutic benefit outweighs risk before granting or denying market authorization.

This top-down model is visualized in. Note that the overall **attrition of this pipeline is steep** because fewer than ten percent of lead candidates survive to approval, and every failure discovered late dramatically inflates cost.

## Where Machine Learning Intervenes

Machine‑learning enters primarily in **early drug discovery** through *target discovery*, *hit discovery*, *hit-to-lead* and *lead optimization*, where data are abundant yet experimental cycles are costly and slow. Several key tasks of machine learning are included in each stage:

- **Target discovery** identifies 
- **Hit discovery** 
- **Hit-to-lead**
- **Lead optimization**

## Scope of This Tutorial Series

Machine learning is not a replacement for biology, organic chemistry, or clinical insight, but it already reallocates experimental effort toward the most promising hypotheses. As foundation models scale and multi‑modal data sets expand—combining cryo‑EM structures, single‑cell omics, and longitudinal patient records—the fidelity of *in silico* predictions will rise. The long‑term goal is not merely faster discovery but a virtuous cycle in which every wet‑lab measurement feeds back to train models that, in turn, suggest better experiments.

In this tutorial
