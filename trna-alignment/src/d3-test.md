---
title: D3 TEST
---

# D3 Test

```js
function testPlot() {
  // Declare the chart dimensions and margins.
  const width = 1000;
  const height = 400;
  const marginTop = 20;
  const marginRight = 20;
  const marginBottom = 30;
  const marginLeft = 40;

  // Declare the x (horizontal position) scale.
  const x = d3.scaleUtc()
      .domain([new Date("2023-01-01"), new Date("2024-01-01")])
      .range([marginLeft, width - marginRight]);

  // Declare the y (vertical position) scale.
  const y = d3.scaleLinear()
      .domain([0, 100])
      .range([height - marginBottom, marginTop]);

  // Create the SVG container.
  const svg = d3.create("svg")
      .attr("width", width)
      .attr("height", height);

  // Add the x-axis.
  svg.append("g")
      .attr("transform", `translate(0,${height - marginBottom})`)
      .call(d3.axisBottom(x));

  // Add the y-axis.
  svg.append("g")
      .attr("transform", `translate(${marginLeft},0)`)
      .call(d3.axisLeft(y));

  // Return the SVG element.
  return svg.node();
}
```


<div class="grid grid-cols-1">
    ${testPlot()}
</div>

# Alignment Viewer

```js echo
function d3_alignment_viewer() {
  const width = 1050;
  const height = 800;
  const margin = { top: 20, bottom: 20, left: 100, right: 20 };

  const container = htl.html`<div id="container">
    <fieldset>
      <div class="input-container">
        <input type="checkbox" id="align-checkbox" name="align" checked/>
        <label for="align-checkbox">Align</label>
      </div>
      <div class="input-container">
        <input type="checkbox" id="splitter-checkbox" name="splitter"/>
        <label for="splitter-checkbox">Show splitters</label>
      </div>
    </fieldset>
    <div id="svg-container">
    </div>
  </div>`;

  const align_checkbox = d3
    .select(container)
    .select("input#align-checkbox");

  const splitter_checkbox = d3
    .select(container)
    .select("input#splitter-checkbox");
  
  const x_domain = Array.from(new Set(aln.map((d) => d.base_idx))).sort(d3.ascending);
  const y_domain = Array.from(new Set(aln.map((d) => d.seq_label)));
  
  const x = d3
    .scaleBand()
    .domain(x_domain)
    .range([0, width]);

  const y = d3
    .scaleBand()
    .domain(y_domain)
    .range([0, height]);

  const c = d3
    .scaleOrdinal()
    .domain([-1, 0, 1, 2, 3, 4])
    .range(["none", "#177E89", "#DB3A34", "#FFC857", "#BBDBB4", "#FF00FF"]);

  const svg = d3
    .select(container)
    .select("div#svg-container")
    .append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom);

  const g = svg
    .append("g")
    .attr("transform", `translate(${margin.left}, ${margin.top})`);

  g.append("g")
    .call(
      d3.axisLeft(y)
        .tickSizeOuter(0)
    );
  
  g.append("g")
    .call(
      d3.axisTop(x)
        .tickValues(calculate_ticks(aln, 5))
        .tickSizeOuter(0)
    );
  
  const g_cell = g
    .append("g")
    .attr("id", "g-alignment-cells")
    .selectAll("rect")
    .data(aln)
    .join("rect")
    .attr("x", (d) => x(d.base_idx))
    .attr("y", (d) => y(d.seq_label))
    .attr("width", x.bandwidth())
    .attr("height", y.bandwidth())
    .attr("fill", (d) => c(d.base_color));

  const g_text = g
    .append("g")
    .attr("id", "g-alignment-text")
    .selectAll("text")
    .data(aln)
    .join("text")
    // 0.3 is a horizontal offset so text is not too close to left of box
    .attr("x", (d) => x(d.base_idx) + 0.3)
    // 2 is a vertical offset so text is not too close to bottom of box
    .attr("y", (d) => y(d.seq_label) + y.bandwidth() - 2)
    // How to make these offsets dynamic based on width and height?
    .attr("fill", "black")
    .attr("font-family", "monospace")
    .text((d) => d.base_label);

  const g_rule = g
    .append("g")
    .attr("id", "g-alignment-rules");

  align_checkbox
    .on("click", (_,__) => {
      // Splitters should only be displayed if the sequences are aligned
      splitter_checkbox.property("disabled", !align_checkbox.property("checked"));
      /*
      g_cell
        .selectAll("rect")
        .data(aln, (d) => `${d.seq_label},${d.base_idx}`)
        .join("rect")
        .attr("x", (d) => x(align_checkbox.property("checked") ? d.base_idx : d.base_num))
        .attr("y", (d) => y(d.seq_label))
        .attr("width", x.bandwidth())
        .attr("height", y.bandwidth())
        .attr("fill", (d) => c(d.base_color));
      */
    });
  
  splitter_checkbox
    .on("click", (_,__) => {
      g_rule
        .selectAll("line")
        .data(splitter_checkbox.property("checked") ? splits : [], (d) => d)
        .join("line")
        .attr("x1", d => x(d+1))
        .attr("x2", d => x(d+1))
        .attr("y1", y(y_domain[0]) - y.bandwidth())
        .attr("y2", y(y_domain[y_domain.length-1]) + y.bandwidth())
        .attr("stroke-width", 3)
        .attr("stroke", "blue");
    });

  return container;
}
```

<div class="grid grid-cols-1">
    ${d3_alignment_viewer()}
</div>

```js
function calculate_ticks(data, delta) {
  const max_idx = d3.max(data.map((d) => d.base_idx));
  let ticks = [];
  for (let i = delta; i < max_idx - 1; i += delta) {
    ticks.push(i);
  }
  ticks.push(max_idx);
  return ticks;
}
```

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
const splits_pre = alignment_selection.splits.json();
```

```js
let splits = [];
for (let i = 0; i < splits_pre.length; ++i) {
    splits.push(splits_pre[i]-0.5);
}
```
