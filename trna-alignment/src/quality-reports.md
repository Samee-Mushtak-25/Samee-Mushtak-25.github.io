---
title: Quality Reports
---

# Quality Reports


## Sum-of-Pairs Scoring

### _Pyrococcus furiosus_

```js
const sop_score_data = [
    {
        "algo": "Manual",
        "score": 18764,
        "order": 0
    },
    {
        "algo": "Clustal Omega",
        "score": 18827,
        "order": 1
    },
    {
        "algo": "MAFFT L-INS-i",
        "score": 15212,
        "order": 2
    },
    {
        "algo": "MAFFT L-INS-i w/ Kimura",
        "score": 18581,
        "order": 3
    }
]
```

<!--
```tex
\int_0^1 e^x d{x}
```
-->

```js
function plot_sop_scores(data, width={}) {
    return Plot.plot({
        padding: 0,
        marginLeft: 150,
        x: {label: "Score"},
        y: {label: "Algorithm"},
        marks: [
            Plot.barX(data, {
                x: "score",
                y: "algo",
                sort: {
                    y: "data",
                    reduce: ([d]) => d.order
                },
                inset: 1
            })
        ]
    })
}
```

<!--
<div class="grid grid-cols-1">
    <div class="card">
        ${resize((width) => plot_sop_scores(sop_score_data, {width}))}
    </div>
</div>
-->

```js
const sop_cost_data = [
    {
        "algo": "Manual",
        "cost": 30490,
        "order": 0
    },
    {
        "algo": "Clustal Omega",
        "cost": 30477,
        "order": 1
    },
    {
        "algo": "MAFFT L-INS-i",
        "cost": 32505,
        "order": 2
    },
    {
        "algo": "MAFFT L-INS-i w/ Kimura",
        "cost": 30621,
        "order": 3
    }
]
```

```js
function plot_sop_costs(data, width={}) {
    return Plot.plot({
        padding: 0,
        marginLeft: 150,
        x: {label: "Cost"},
        y: {label: "Algorithm"},
        marks: [
            Plot.barX(data, {
                x: "cost",
                y: "algo",
                sort: {
                    y: "data",
                    reduce: ([d]) => d.order
                },
                inset: 1
            })
        ]
    })
}
```

<div class="grid grid-cols-2">
    <div class="card">
        ${resize((width) => plot_sop_scores(sop_score_data, {width}))}
    </div>
    <div class="card">
        ${resize((width) => plot_sop_costs(sop_cost_data, {width}))}
    </div>
</div>

## MetAl Scoring

Scores calculated using Benjamin Blackburne's MetAl command line utility compiled from [source](https://github.com/benb/MetAl). See original paper: [https://doi.org/10.1093/bioinformatics/btr701](https://doi.org/10.1093/bioinformatics/btr701)

```js
const dist_data = [
    {
        "row_label" : "Manual",
        "row_idx" : 0,
        "col_label" : "Manual",
        "col_idx" : 0,
        "d_pos" : 0,
        "d_ssp" : 0,
        "d_simple" : 0,
        "d_pos_inv": 1,
        "d_ssp_inv": 1,
        "d_simple_inv": 1
    },
    {
        "row_label" : "Manual",
        "row_idx" : 0,
        "col_label" : "Clustal Omega",
        "col_idx" : 1,
        "d_pos" : 0.049,
        "d_ssp" : 0.057,
        "d_simple" : 0.037,
        "d_pos_inv": 0.951,
        "d_ssp_inv": 0.943,
        "d_simple_inv": 0.963
    },
    {
        "row_label" : "Manual",
        "row_idx" : 0,
        "col_label" : "MAFFT",
        "col_idx" : 2,
        "d_pos" : 0.189,
        "d_ssp" : 0.314,
        "d_simple" : 0.187,
        "d_pos_inv": 0.819,
        "d_ssp_inv": 0.686,
        "d_simple_inv": 0.813
    },
    {
        "row_label" : "Manual",
        "row_idx" : 0,
        "col_label" : "MAFFT-Kimura",
        "col_idx" : 3,
        "d_pos" : 0.056,
        "d_ssp" : 0.061,
        "d_simple" : 0.040,
        "d_pos_inv": 0.944,
        "d_ssp_inv": 0.939,
        "d_simple_inv": 0.960
    },
    {
        "row_label" : "Manual",
        "row_idx" : 0,
        "col_label" : "Infernal",
        "col_idx" : 4,
        "d_pos" : 0.094,
        "d_ssp" : 0.106,
        "d_simple" : 0.076,
        "d_pos_inv": 0.906,
        "d_ssp_inv": 0.894,
        "d_simple_inv": 0.924
    },
    {
        "row_label" : "Clustal Omega",
        "row_idx" : 1,
        "col_label" : "Manual",
        "col_idx" : 0,
        "d_pos" : 0.049,
        "d_ssp" : 0.057,
        "d_simple" : 0.037,
        "d_pos_inv": 0.951,
        "d_ssp_inv": 0.943,
        "d_simple_inv": 0.963
    },
    {
        "row_label" : "Clustal Omega",
        "row_idx" : 1,
        "col_label" : "Clustal Omega",
        "col_idx" : 1,
        "d_pos" : 0,
        "d_ssp" : 0,
        "d_simple" : 0,
        "d_pos_inv": 1,
        "d_ssp_inv": 1,
        "d_simple_inv": 1
    },
    {
        "row_label" : "Clustal Omega",
        "row_idx" : 1,
        "col_label" : "MAFFT",
        "col_idx" : 2,
        "d_pos" : 0.204,
        "d_ssp" : 0.325,
        "d_simple" : 0.191,
        "d_pos_inv": 0.796,
        "d_ssp_inv": 0.675,
        "d_simple_inv": 0.809
    },
    {
        "row_label" : "Clustal Omega",
        "row_idx" : 1,
        "col_label" : "MAFFT-Kimura",
        "col_idx" : 3,
        "d_pos" : 0.039,
        "d_ssp" : 0.026,
        "d_simple" : 0.019,
        "d_pos_inv": 0.961,
        "d_ssp_inv": 0.974,
        "d_simple_inv": 0.981
    },
    {
        "row_label" : "Clustal Omega",
        "row_idx" : 1,
        "col_label" : "Infernal",
        "col_idx" : 4,
        "d_pos" : 0.094,
        "d_ssp" : 0.086,
        "d_simple" : 0.062,
        "d_pos_inv": 0.906,
        "d_ssp_inv": 0.914,
        "d_simple_inv": 0.938
    },
    {
        "row_label" : "MAFFT",
        "row_idx" : 2,
        "col_label" : "Manual",
        "col_idx" : 0,
        "d_pos" : 0.189,
        "d_ssp" : 0.314,
        "d_simple" : 0.187,
        "d_pos_inv": 0.819,
        "d_ssp_inv": 0.686,
        "d_simple_inv": 0.813
    },
    {
        "row_label" : "MAFFT",
        "row_idx" : 2,
        "col_label" : "Clustal Omega",
        "col_idx" : 1,
        "d_pos" : 0.204,
        "d_ssp" : 0.325,
        "d_simple" : 0.191,
        "d_pos_inv": 0.796,
        "d_ssp_inv": 0.675,
        "d_simple_inv": 0.809
    },
    {
        "row_label" : "MAFFT",
        "row_idx" : 2,
        "col_label" : "MAFFT",
        "col_idx" : 2,
        "d_pos" : 0,
        "d_ssp" : 0,
        "d_simple" : 0,
        "d_pos_inv": 1,
        "d_ssp_inv": 1,
        "d_simple_inv": 1
    },
    {
        "row_label" : "MAFFT",
        "row_idx" : 2,
        "col_label" : "MAFFT-Kimura",
        "col_idx" : 3,
        "d_pos" : 0.216,
        "d_ssp" : 0.329,
        "d_simple" : 0.199,
        "d_pos_inv": 0.784,
        "d_ssp_inv": 0.671,
        "d_simple_inv": 0.801
    },
    {
        "row_label" : "MAFFT",
        "row_idx" : 2,
        "col_label" : "Infernal",
        "col_idx" : 4,
        "d_pos" : 0.242,
        "d_ssp" : 0.351,
        "d_simple" : 0.224,
        "d_pos_inv": 0.758,
        "d_ssp_inv": 0.649,
        "d_simple_inv": 0.776
    },
    {
        "row_label" : "MAFFT-Kimura",
        "row_idx" : 3,
        "col_label" : "Manual",
        "col_idx" : 0,
        "d_pos" : 0.056,
        "d_ssp" : 0.061,
        "d_simple" : 0.040,
        "d_pos_inv": 0.944,
        "d_ssp_inv": 0.939,
        "d_simple_inv": 0.960
    },
    {
        "row_label" : "MAFFT-Kimura",
        "row_idx" : 3,
        "col_label" : "Clustal Omega",
        "col_idx" : 1,
        "d_pos" : 0.039,
        "d_ssp" : 0.026,
        "d_simple" : 0.019,
        "d_pos_inv": 0.961,
        "d_ssp_inv": 0.974,
        "d_simple_inv": 0.981
    },
    {
        "row_label" : "MAFFT-Kimura",
        "row_idx" : 3,
        "col_label" : "MAFFT",
        "col_idx" : 2,
        "d_pos" : 0.216,
        "d_ssp" : 0.329,
        "d_simple" : 0.199,
        "d_pos_inv": 0.784,
        "d_ssp_inv": 0.671,
        "d_simple_inv": 0.801
    },
    {
        "row_label" : "MAFFT-Kimura",
        "row_idx" : 3,
        "col_label" : "MAFFT-Kimura",
        "col_idx" : 3,
        "d_pos" : 0,
        "d_ssp" : 0,
        "d_simple" : 0,
        "d_pos_inv": 1,
        "d_ssp_inv": 1,
        "d_simple_inv": 1
    },
    {
        "row_label" : "MAFFT-Kimura",
        "row_idx" : 3,
        "col_label" : "Infernal",
        "col_idx" : 4,
        "d_pos" : 0.089,
        "d_ssp" : 0.094,
        "d_simple" : 0.067,
        "d_pos_inv": 0.911,
        "d_ssp_inv": 0.906,
        "d_simple_inv": 0.933
    },
    {
        "row_label" : "Infernal",
        "row_idx" : 4,
        "col_label" : "Manual",
        "col_idx" : 0,
        "d_pos" : 0.094,
        "d_ssp" : 0.106,
        "d_simple" : 0.076,
        "d_pos_inv": 0.906,
        "d_ssp_inv": 0.894,
        "d_simple_inv": 0.924
    },
    {
        "row_label" : "Infernal",
        "row_idx" : 4,
        "col_label" : "Clustal Omega",
        "col_idx" : 1,
        "d_pos" : 0.094,
        "d_ssp" : 0.086,
        "d_simple" : 0.062,
        "d_pos_inv": 0.906,
        "d_ssp_inv": 0.914,
        "d_simple_inv": 0.938
    },
    {
        "row_label" : "Infernal",
        "row_idx" : 4,
        "col_label" : "MAFFT",
        "col_idx" : 2,
        "d_pos" : 0.242,
        "d_ssp" : 0.351,
        "d_simple" : 0.224,
        "d_pos_inv": 0.758,
        "d_ssp_inv": 0.649,
        "d_simple_inv": 0.776
    },
    {
        "row_label" : "Infernal",
        "row_idx" : 4,
        "col_label" : "MAFFT-Kimura",
        "col_idx" : 3,
        "d_pos" : 0.089,
        "d_ssp" : 0.094,
        "d_simple" : 0.067,
        "d_pos_inv": 0.911,
        "d_ssp_inv": 0.906,
        "d_simple_inv": 0.933
    },
    {
        "row_label" : "Infernal",
        "row_idx" : 4,
        "col_label" : "Infernal",
        "col_idx" : 4,
        "d_pos" : 0,
        "d_ssp" : 0,
        "d_simple" : 0,
        "d_pos_inv": 1,
        "d_ssp_inv": 1,
        "d_simple_inv": 1
    },
];
```


```js
const dists = [
    {"dist" : "d_ssp", "dist_literal" : "Symmetrized Sum of Pairs Distance"},
    {"dist" : "d_pos", "dist_literal" : "Positional Encoding Distance"},
    {"dist" : "d_simple", "dist_literal" : "Homology Distance"}
];
```

```js
const dist_selection = view(
    Inputs.select(
        dists, {
            value: dists.find( (t) => t.dist === "d_ssp" ),
            format: (t) => t.dist_literal,
            label: "Species"
        }
    )
);
```

```js
function plot_dists(data, width={}) {
    return Plot.plot({
        padding: 0,
        grid: true,
        aspectRatio: 1,
        marginLeft: 100,
        x: {
            domain: ["Manual", "Clustal Omega", "MAFFT", "MAFFT-Kimura", "Infernal"],
            axis: "top",
            label: "Algorithm 1"
        },
        y: {
            domain: ["Manual", "Clustal Omega", "MAFFT", "MAFFT-Kimura", "Infernal"],
            label: "Algorithm 2"
        },
        color: {type: "linear", scheme: "GnBu"},
        marks: [
            Plot.cell(data, {
                x : "col_label",
                y : "row_label",
                fill : dist_selection.dist + "_inv",
                inset: 0.5
            }),
            Plot.text(data, {
                x : "col_label",
                y : "row_label",
                text : dist_selection.dist,
                fill: "black"
            })
        ]
    })
}
```

<div class="grid grid-cols-1">
    <div class="card">
        ${resize((width) => plot_dists(dist_data, {width}))}
    </div>
</div>

## Multiple Overlap Scoring

```js
const pyr_fur_mos_score_data = [
    {
        "algo": "Manual",
        "score": 0.901200508293205,
        "order": 0
    },
    {
        "algo": "Clustal Omega",
        "score": 0.9100704846183093,
        "order": 1
    },
    {
        "algo": "MAFFT L-INS-i",
        "score": 0.7983138457447251,
        "order": 2
    },
    {
        "algo": "MAFFT L-INS-i w/ Kimura",
        "score": 0.9107400122771632,
        "order": 3
    }
];
const halo_volc_mos_score_data = [
    {
        "algo": "Clustal Omega",
        "score": 0.8289635838530937,
        "order": 1
    },
    {
        "algo": "MAFFT L-INS-i",
        "score": 0.8895253607184923,
        "order": 2
    },
    {
        "algo": "MAFFT L-INS-i w/ Kimura",
        "score": 0.8885055777149332,
        "order": 3
    }
];
const chlo_chlo_mos_score_data = [
    {
        "algo": "Clustal Omega",
        "score": 0.9372615619385168,
        "order": 1
    },
    {
        "algo": "MAFFT L-INS-i",
        "score": 0.957567688062561,
        "order": 2
    },
    {
        "algo": "MAFFT L-INS-i w/ Kimura",
        "score": 0.9570462449308672,
        "order": 3
    }
];
```

```js
function plot_mos_scores(data, width={}) {
    return Plot.plot({
        padding: 0,
        marginLeft: 150,
        x: {label: "Score", domain: [0,1]},
        y: {label: "Algorithm"},
        marks: [
            Plot.barX(data, {
                x: "score",
                y: "algo",
                sort: {
                    y: "data",
                    reduce: ([d]) => d.order
                },
                inset: 1
            }),
            Plot.ruleX([0.8], {
                stroke: "red"
            })
        ]
    })
}
```

### _Pyrococcus furiosus_
<div class="grid grid-cols-1">
    <div class="card">
        ${resize((width) => plot_mos_scores(pyr_fur_mos_score_data, {width}))}
    </div>
</div>

### _Haloferax volcanii_
<div class="grid grid-cols-1">
    <div class="card">
        ${resize((width) => plot_mos_scores(halo_volc_mos_score_data, {width}))}
    </div>
</div>

### _Chlorobium chlorochromatii_
<div class="grid grid-cols-1">
    <div class="card">
        ${resize((width) => plot_mos_scores(chlo_chlo_mos_score_data, {width}))}
    </div>
</div>