# CLDedgelister

**CLDedgelister** is an R package that converts System Dynamics Models (SDMs) and Causal Loop Diagrams (CLDs) from popular modelling software into an **edge list** (Input → Output) that can be directly analyzed using R network analysis packages such as **igraph**.

---

## Overview

Specialized System Dynamics software stores models in proprietary or semi-structured formats that are difficult to analyze directly in R. Before performing network analysis, users typically need to manually extract every causal relationship (i.e., determine which variable influences another).

**CLDedgelister** automates this process by reading supported model files and producing a clean edge list representing the causal structure of the model.

The resulting edge list can be immediately imported into network analysis workflows or applications such as the **SDM Network Analyzer**.

---

## Features

- Automatically extracts causal relationships from multiple System Dynamics modelling platforms.
- Converts models into a standard **Input → Output** edge list.
- Supports automatic file format detection.
- Export results to CSV or Excel.
- Includes an RStudio Add-in for users who prefer a graphical interface.

---

## Supported Model Formats

| Software | Supported File |
|-----------|----------------|
| Vensim | Native `.mdl` files (diagram and equations) |
| Stella / iThink | XMILE `.stmx` and `.itmx` |
| Powersim Studio | Exported XMILE (`.xmile` or `.xml`) |
| AnyLogic | `.alp` project file |
| GoldSim | Exported XML |

---

# Functions

## `model_to_edgelist()`

Reads a supported model file and returns the edge list as an R data frame.

```r
el <- model_to_edgelist("model.mdl")
```

The file format is automatically detected from the file extension whenever possible.

---

## `model_to_edgelist_file()`

Reads a model and directly exports the resulting edge list to a CSV or Excel file.

```r
model_to_edgelist_file(
    "model.mdl",
    output = "edge_list.csv"
)
```

---

## RStudio Add-in

CLDedgelister includes an RStudio Add-in that provides a simple graphical interface.

Using the add-in you can:

- Browse for a model file
- Automatically detect or select the model format
- Preview the extracted edge list
- Return the edge list to R
- Save the edge list as CSV or Excel

No coding is required.

---

# Workflow

```text
System Dynamics Model
        │
        ▼
 CLDedgelister
        │
        ▼
 Input → Output Edge List
        │
        ▼
igraph::graph_from_data_frame()
        │
        ▼
Network Analysis
```

---

# Using the Edge List

The resulting edge list can be used directly with **igraph**.

```r
library(igraph)

g <- graph_from_data_frame(el)

plot(g)
```

---

# Powersim Studio

Powersim's native `.sip` files are proprietary binary files and cannot be read directly.

Instead:

1. Open the model in Powersim Studio.
2. Export the model to the **XMILE** format.
3. Import the exported XMILE file into CLDedgelister.

Example:

```r
el <- model_to_edgelist(
    "mymodel.xmile",
    format = "Powersim Studio (XMILE .xml)"
)
```

Alternatively, choose **Powersim Studio (XMILE .xml)** from the RStudio Add-in format list.

---

# GoldSim

GoldSim models are stored as proprietary `.gsm` files.

Export the model to XML before importing.

Steps:

1. Open the GoldSim model.
2. Export the model as XML.
3. Read the exported XML using CLDedgelister.

```r
el <- model_to_edgelist(
    "mymodel.xml",
    format = "GoldSim (exported .xml)"
)
```

Or select **GoldSim (exported .xml)** within the RStudio Add-in.

---

# Why Export is Required

Unlike Vensim, Stella, and AnyLogic, Powersim and GoldSim save models using proprietary binary formats (`.sip` and `.gsm`).

These binary formats cannot be parsed reliably by external software.

Exporting the model to **XMILE** or **XML** converts it into an open, text-based format that CLDedgelister can read and parse accurately.

---

# Available Formats

To display all supported format names accepted by the package, run:

```r
cld_formats()
```

---

# Examples

## Load the Included Example Model

```r
el <- model_to_edgelist(
    system.file(
        "extdata",
        "example_model.mdl",
        package = "CLDedgelister"
    )
)
```

---

## Read Your Own Vensim Model

```r
el <- model_to_edgelist(
    "path/to/your_model.mdl"
)
```

---

## Export an Edge List

```r
model_to_edgelist_file(
    "path/to/your_model.mdl",
    output = "edge_list.xlsx"
)
```

---

## Build an igraph Network

```r
library(igraph)

el <- model_to_edgelist("model.mdl")

g <- graph_from_data_frame(el)

plot(g)
```

---

# Typical Workflow

1. Build your System Dynamics model in your preferred software.
2. Export to an open format if using Powersim or GoldSim.
3. Read the model with `model_to_edgelist()`.
4. Generate an edge list.
5. Import the edge list into **igraph** or any other network analysis package.
6. Perform network analysis, visualization, or feed the edge list into applications such as the **SDM Network Analyzer**.

---

# License
CC
See the package `LICENSE` file for more licensing information.
