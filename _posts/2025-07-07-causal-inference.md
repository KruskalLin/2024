---
layout: distill
title: Data and Representation in Machine Learning Drug Discovery
description: 
date: 2025-07-07
future: true
htmlwidgets: true

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
  - name: What is Molecular Representation?
    subsections:
      - name: SMILES
      - name: Molecular Topological Graph
      - name: 3D Molecular Structure
  - name: Molecular Representation Learning

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


A molecular representation is a formal mapping that converts the physical reality of a chemical compound into a data structure suitable for computation. An ideal representation preserves the information needed to predict relevant observables—reactivity, binding affinity, synthetic accessibility—while discarding superfluous detail so that downstream models remain tractable. In practice, no single encoding is universally optimal. Line notations such as SMILES are compact and easily tokenized, graph structures capture connectivity and stereochemistry, and three‑dimensional coordinates expose the spatial degrees of freedom that dominate non‑covalent interactions. Modern machine‑learning pipelines move fluidly among these layers, translating one form into another as the task demands.





## Basic Molecular Representations



### SMILES

The SMILES specification encodes a molecule as a sequence of ASCII tokens that follow depth‑first traversal of its molecular graph. Atoms appear as element symbols surrounded by optional brackets that annotate isotope, charge, stereochemistry, and hydrogen count. Single bonds are implicit; double, triple, and aromatic bonds use the symbols `=`, `#`, and `:`. Branches are enclosed in parentheses, and ring closures are indicated by numerical indices that mark the two atoms joined. Canonicalization algorithms reorder traversal deterministically, giving each structure a unique “canonical SMILES” that enables rapid hashing and duplicate removal. The representation is concise—benzene reduces to the six‑character string `"c1ccccc1"`—and readily tokenized for language models, yet its one‑dimensional nature obscures conformational information and allows syntactically valid but chemically impossible strings. Augmentations such as random SMILES enumeration and SELFIES introduce robustness by ensuring all generated strings decode to valid molecules.

### Molecular Topological Graph

A topological graph models a molecule as an undirected multigraph $G=(V,E)$ where vertices correspond to atoms and edges to covalent bonds. Vertex attributes typically include atomic number, formal charge, aromaticity, degree, and hybridization state; edge attributes describe bond order, conjugation, and stereochemical parity. The graph abstraction preserves the combinatorial chemistry that governs reaction transforms and scaffold hopping while remaining invariant to translation and rotation. Algorithms for subgraph matching, cycle detection, and fingerprinting operate naturally on this structure, and it forms the backbone input for message‑passing neural networks. Chirality and cis–trans isomerism require additional edge or face labels so that enantiomers do not collapse into the same topology.

### 3D Molecular Structure

Three‑dimensional structure specifies Cartesian coordinates for each atom, optionally accompanied by partial charges, atomic radii, or multipole moments. A single molecule usually admits a thermodynamic ensemble of conformers whose populations depend on temperature, solvent, and steric constraints. Quantum chemistry and molecular‑mechanics force fields supply potential energies that rank these conformers, making geometry a central variable in docking, binding free‑energy estimation, and property prediction tasks sensitive to shape and electrostatics. Coordinate files appear in formats such as PDB, SDF, and XYZ, each with conventions for unit cell, connectivity, and metadata. Accuracy in 3D comes at computational cost: learning algorithms must be equivariant to rotations and, for flexible molecules, robust to continuous deformations across conformational space.



