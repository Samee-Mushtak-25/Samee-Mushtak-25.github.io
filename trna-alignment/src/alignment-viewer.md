---
title: Alignment Viewer
---

# Alignment Viewer

```js
const jsons = [
    {
        "species" : "Pyrococcus furiosus",
        "files" : [
            {
                "alignment" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-manual.json"),
                "splits" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-manual.splits.json")
            },
            {
                "alignment" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-clustal-omega.json"),
                "splits" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-clustal-omega.splits.json")
            },
            {
                "alignment" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-mafft.json"),
                "splits" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-mafft.splits.json")
            },
            {
                "alignment" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-mafft-kimura.json"),
                "splits" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-mafft-kimura.splits.json")
            },
            {
                "alignment" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-infernal.json"),
                "splits" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-infernal.splits.json")
            },
            {
                "alignment" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-infernal-immature.json"),
                "splits" : FileAttachment("data/alignments/Pyrococcus-furiosus/pyrFur2-infernal-immature.splits.json")
            }
        ]
    },
    {
        "species" : "Haloferax volcanii",
        "files" : [
            {
                "alignment" : FileAttachment("data/alignments/Haloferax-volcanii/haloVolc1-clustal-omega.json"),
                "splits" : FileAttachment("data/alignments/Haloferax-volcanii/haloVolc1-clustal-omega.splits.json")
            },
            {
                "alignment" : FileAttachment("data/alignments/Haloferax-volcanii/haloVolc1-mafft.json"),
                "splits" : FileAttachment("data/alignments/Haloferax-volcanii/haloVolc1-mafft.splits.json")
            },
            {
                "alignment" : FileAttachment("data/alignments/Haloferax-volcanii/haloVolc1-mafft-kimura.json"),
                "splits" : FileAttachment("data/alignments/Haloferax-volcanii/haloVolc1-mafft-kimura.splits.json")
            }
        ]
    },
    {
        "species" : "Chlorobium chlorochromatii",
        "files" : [
            {
                "alignment" : FileAttachment("data/alignments/Chlorobium-chlorochromatii/chloChlo-CAD3-clustal-omega.json"),
                "splits" : FileAttachment("data/alignments/Chlorobium-chlorochromatii/chloChlo-CAD3-clustal-omega.splits.json")
            },
            {
                "alignment" : FileAttachment("data/alignments/Chlorobium-chlorochromatii/chloChlo-CAD3-mafft.json"),
                "splits" : FileAttachment("data/alignments/Chlorobium-chlorochromatii/chloChlo-CAD3-mafft.splits.json")
            },
            {
                "alignment" : FileAttachment("data/alignments/Chlorobium-chlorochromatii/chloChlo-CAD3-mafft-kimura.json"),
                "splits" : FileAttachment("data/alignments/Chlorobium-chlorochromatii/chloChlo-CAD3-mafft-kimura.splits.json")
            }

        ]
    }
];
```

```js
const species_selection = view(
    Inputs.select(
        jsons, {
            value: jsons.find( (t) => t.species === "Pyrococcus furiosus" ),
            format: (t) => t.species,
            label: "Species"
        }
    )
);
```

```js
const sel_species = species_selection.species;
```

```js
const alignment_selection = view(
    Inputs.select(
        jsons.find( (t) => t.species === sel_species ).files, {
            format: (d) => d.alignment.name,
            label: "Alignment File"
        }
    )
);
```

```js
const aln = alignment_selection.alignment.json();
const splits = alignment_selection.splits.json();
```

```js
const isAligned = view(
    Inputs.toggle({
        label: "Align",
        value: true
    })
);
```

```js
const showSplitters = view(
    Inputs.toggle({
        label: "Show splitters",
        value: false,
        disabled: !isAligned
    })
);
```

```js
function alignmentPlot(data, showSplitters, width={}) {
    // Find maximum index in alignment
    // Because Python code generates bases in a sequential order, we can break early
    let max_idx = 0;
    for (let i = 0; i < data.length; ++i) {
        let d = data[i];
        if (d.base_idx > max_idx) {
            max_idx = d.base_idx;
        }
        else {
            break;
        }
    }
    // List of integers from 1 to max_idx to use as tick labels
    let int_ticks = [];
    for (let i = 5; i < max_idx - 1; i += 5) {
        int_ticks.push(i);
    }
    int_ticks.push(max_idx);

    let marks = [
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
        })
    ];
    if (showSplitters) {
        marks.push(
            Plot.ruleX(splits, {
                stroke: "red"
            })
        );
    }

    return Plot.plot({
        padding: 0,
        marginLeft: 100,
        width: 2000,
        height: 1500,
        grid: true,
        x: {
            axis: "top",
            label: "",
            ticks: int_ticks
        },
        y: {label: ""},
        color: {
            domain: [0, 1, 2, 3, 4],
            range: ["#177E89", "#DB3A34", "#FFC857", "#BBDBB4", "#FF00FF"]
        },
        marks: marks
    })
}
```

```js
function unalignedPlot(data, width={}) {
    // Consensus is not meaningful for unaligned sequences
    data = data.filter((d) => d.seq_label !== 'Consensus');
    // Find maximum length of sequence
    let max_num = 0;
    for (let i = 0; i < data.length; ++i) {
        let d = data[i];
        if (d.base_num > max_num) {
            max_num = d.base_num;
        }
        // Cannot break early anymore because diff sequences have diff lengths
    }
    // List of integers from 1 to max_idx to use as tick labels
    let int_ticks = [];
    for (let i = 5; i < max_num - 1; i += 5) {
        int_ticks.push(i);
    }
    int_ticks.push(max_num);

    return Plot.plot({
        padding: 0,
        marginLeft: 100,
        width: 2000,
        height: 1500,
        grid: true,
        x: {
            axis: "top",
            label: "",
            ticks: int_ticks
        },
        y: {label: ""},
        color: {
            domain: [0, 1, 2, 3, 4],
            range: ["#177E89", "#DB3A34", "#FFC857", "#BBDBB4", "#FF00FF"]
        },
        marks: [
            Plot.cell(data, {
                x: "base_num",
                y: "seq_label",
                sort: {
                    y: "data",
                    reduce: ([d]) => d.seq_idx
                },
                fill: "base_color"
            }),
            Plot.text(data, {
                x: "base_num",
                y: "seq_label",
                sort: {
                    y: "data",
                    reduce: ([d]) => d.seq_idx
                },
                text: "base_label",
                fill: "black",
                title: (d) => `${d.seq_label}: Base #${d.base_num}=${d.base_label}`
            }),
        ]
    })
}
```

```js
const aln_filt = aln.filter((e) => e.seq_label === 'Consensus')
```

<div class="grid grid-cols-1">
    <div class="card">
        ${combinedPlot}
    </div>
</div>

<div class="grid grid-cols-1">
    <div class="card">
        ${resize((width) => isAligned ? alignmentPlot(aln, showSplitters, {width}) : unalignedPlot(aln, {width}))}
    </div>
</div>

```js
function calculate_ticks(data, delta) {
    // Find maximum index in alignment
    // Because Python code generates bases in a sequential order, we can break early
    let max_idx = 0;
    for (let i = 0; i < data.length; ++i) {
        let d = data[i];
        if (d.base_idx > max_idx) {
            max_idx = d.base_idx;
        }
        else {
            break;
        }
    }
    // List of integers from 1 to max_idx to use as tick labels
    let ticks = [];
    for (let i = delta; i < max_idx - 1; i += delta) {
        ticks.push(i);
    }
    ticks.push(max_idx);
    return ticks
}
```

```js
const combinedPlot = Plot.plot({
    padding: 0,
    marginLeft: 100,
    width: 2000,
    height: 1500,
    grid: true,
    x: {axis: "top", label: "", ticks: calculate_ticks(aln, 5)},
    y: {label: ""},
    color: {
        domain: [0, 1, 2, 3, 4],
        range: ["#177E89", "#DB3A34", "#FFC857", "#BBDBB4", "#FF00FF"]
    },
    marks: [
        Plot.cell(aln, {
            x: "base_idx", 
            y: "seq_label",
            sort: {
                y: "data",
                reduce: ([d]) => d.seq_idx
            },
            fill: "base_color",
            render: render_cell
        }),
        Plot.text(aln, {
            x: "base_idx",
            y: "seq_label",
            sort: {
                y: "data",
                reduce: ([d]) => d.seq_idx
            },
            text: "base_label",
            fill: "black",
            title: (d) => `${d.seq_label}: Base #${d.base_num}=${d.base_label}`,
            render: render_text
        }),
        Plot.ruleX([0], {
            stroke: "red",
            strokeWidth: 0,
            render: render_rule
        })
    ]
});
```

```js
const render_cell = (index, scales, values, dimensions, context, next) => {
    const {x} = scales;
    const y0 = dimensions.height - dimensions.marginBottom; // vertical position of the y-axis
    const bars = next(index, scales, values, dimensions, context);
    const svg = context.ownerSVGElement;

    svg.update_cell = (isAligned) => {
      d3.select(bars)
          .selectAll("rect")
        .transition().duration(1000)
          .attr("x", i => x(aln[i][isAligned ? 'base_idx' : 'base_num']))
    };

    return bars;
};
```

```js
const render_text = (index, scales, values, dimensions, context, next) => {
    const {x,y} = scales;
    const y0 = dimensions.height - dimensions.marginBottom; // vertical position of the y-axis
    const bars = next(index, scales, values, dimensions, context);
    const svg = context.ownerSVGElement;

    svg.update_text = (isAligned) => {
      d3.select(bars)
          .selectAll("text")
        .transition().duration(1000)
          .attr("transform", i => 
            `translate(${x(aln[i][isAligned ? 'base_idx' : 'base_num'])}, ${y(aln[i]['seq_label'])})`
          )
    };
    // Need to make base_num null for consensus markers

    return bars;
};
```

```js
const render_rule = (index, scales, values, dimensions, context, next) => {
    const {x} = scales;
    console.log(dimensions);
    const y0 = dimensions.height - dimensions.marginBottom; // vertical position of the y-axis
    const g = next(index, scales, values, dimensions, context);
    const svg = context.ownerSVGElement;

    /*
    svg.update_cell = (isAligned) => {
      d3.select(bars)
          .selectAll("rect")
        .transition().duration(1000)
          .attr("x", i => x(aln[i][isAligned ? 'base_idx' : 'base_num']))
    };
    */
   svg.toggle_rule = (showSplitters) => {
        d3.select(g).attr("stroke-width", showSplitters ? 1 : 0);
        /*
        for (let i = 0; i < splits.length; ++i) {
            d3.select(g)
                .append("line")
                    .attr("x1", x(i))
                    .attr("x2", x(i))
                    .attr("y1", 30)
                    .attr("y2", 1480);
        }
        */
    };


    return g;
};
```

```js
combinedPlot.update_cell(isAligned);
combinedPlot.update_text(isAligned);
combinedPlot.toggle_rule(showSplitters);
```