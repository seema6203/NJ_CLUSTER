# NJ_CLUSTER

Builds a bootstrapped **neighbour-joining (NJ) dendrogram** from SSR genotype data —
the kind of unrooted, colour-coded cluster figure used in population-genetic papers
(for example [this *Frontiers in Plant Science* figure](https://www.frontiersin.org/files/Articles/773572/fpls-13-773572-HTML-r1/image_m/fpls-13-773572-g002.jpg),
which is what this script was written to reproduce).

## What the script does

`nj/nj.R` defines a single function, `nj_tree(file, working_dir)`, which:

1. reads a GenAlEx-formatted CSV with `poppr::read.genalex()`
2. computes a pairwise distance matrix and builds the NJ tree with `ape::nj()`
3. runs **1000 bootstrap replicates** via `ape::boot.phylo()`
4. hides support values below 70% so the figure stays readable
5. plots the tree unrooted, with tips coloured by cluster, a legend and a scale bar
6. writes the result to a PDF

## Usage

```r
source("nj/nj.R")
nj_tree(file = "nj.csv", working_dir = "path/to/your/data")
```

Both arguments are yours to set — the values at the top of the script are the
author's local ones and will not exist on your machine.

## Input format

A **GenAlEx-formatted CSV**, as expected by `read.genalex()`: three header rows
(loci count, sample count, population sizes) followed by one row per individual
with two columns per locus. `nj/nj.csv` and `nj/structure_SSR_nouchali.csv` are
working examples.

## Cluster colours

Tip colours are assigned positionally:

```r
colr <- c(rep("purple", 2), rep("red", 39), rep("purple", 22), rep("orange", 29))
```

That vector is specific to this dataset's 92 individuals and their ordering. For
your own data, either rebuild it to match your sample order or derive it from
`pop(nou)` so it adapts automatically.

## Requirements

```r
install.packages(c("ape", "poppr", "adegenet"))
```

## Outputs

`nj/nj_nei.pdf` and `nj/nj_new.pdf` are the rendered trees; `nj/Rplot.pdf` is a
working plot.

## License

MIT — see [LICENSE](LICENSE).
