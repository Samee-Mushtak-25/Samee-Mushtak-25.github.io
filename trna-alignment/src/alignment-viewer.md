---
title: Alignment Viewer
---

# Alignment Viewer

```js
const jsons = [
    FileAttachment("data/alignments/pyrFur2-manual.json"),
    FileAttachment("data/alignments/pyrFur2-clustal-omega.json"),
    FileAttachment("data/alignments/pyrFur2-mafft.json"),
    FileAttachment("data/alignments/pyrFur2-mafft-kimura.json")
];
```

```js
const display_file = view(
    Inputs.select(
        jsons, {
            value: jsons.find( (t) => t.name  === "data/alignments/pyrFur2-manual.json" ),
            format: (t) => t.name,
            label: "File"
        }
    )
);
```

```js
const aln = display_file.json()
```

<!--
TODO:
    * Add markers for consensus at the bottom of the plot
    * Add demarcations/highlights for different regions of the tRNA (D/T/Ac Loop, 5'/3' Ac Stem, ...)
-->

```js
function alignmentPlot(data, width={}) {
    return Plot.plot({
        padding: 0,
        width: 1900,
        grid: true,
        x: {axis: "top", label: ""},
        y: {label: ""},
        color: {
            domain: [0, 1, 2, 3, 4],
            range: ["#177E89", "#DB3A34", "#FFC857", "#BBDBB4", "#FF00FF"]
        },
        marks: [
            Plot.cell(data, {
                x: "base_idx",
                y: "seq_label",
                sort: {
                    y: "data",
                    reduce: ([d]) => d.seq_idx
                },
                fill: "base_color"
            }),
            Plot.text(data, {
                x: "base_idx",
                y: "seq_label",
                sort: {
                    y: "data",
                    reduce: ([d]) => d.seq_idx
                },
                text: "base_label",
                fill: "black",
                title: (d) => `${d.seq_label}: Base #${d.base_num}=${d.base_label}`
            }),
            /*
            Plot.ruleX([7], {
                stroke: "red"
            })
            */
        ]
    })
}
```

<div class="grid grid-cols-1">
    <!-- Increase padding-left to 48 to accommodate y-axis labels -->
    <div class="card">
        ${resize((width) => alignmentPlot(aln, {width}))}
    </div>
</div>