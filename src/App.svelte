<script lang="ts">
  import { onMount } from "svelte";
  import * as d3 from "d3";

  let _svg: d3.Selection<d3.BaseType, unknown, HTMLElement, any>;
  let svg = _svg as unknown | SVGElement;
  let _dv: d3.Selection<d3.BaseType, unknown, HTMLElement, any>;
  let dv = _dv as unknown | SVGElement;
  let _btn: d3.Selection<d3.BaseType, unknown, HTMLElement, any>;
  let btn = _btn as unknown | SVGElement;

  onMount(() => {
    _svg = d3.select("svg");

    _svg
      .attr("width", 400)
      .attr("height", 400)
      .attr("style", "background: #AAA");

    var ellipses = [
      { cx: 150, cy: 100, rx: 150, ry: 50, fill: "yellow" },
      { cx: 150, cy: 250, rx: 150, ry: 50, fill: "lime" },
    ];

    ellipses.forEach((d, i) => {
      _svg
        .append("ellipse")
        .attr("cx", d.cx + 5)
        .attr("cy", d.cy + 10)
        .attr("rx", d.rx - 5)
        .attr("ry", d.ry)
        .attr("class", "shadow")
        .attr("fill", "rgb(238,238,238)")
        .attr("id", "sh" + i);
      _svg
        .append("ellipse")
        .attr("cx", d.cx)
        .attr("cy", d.cy)
        .attr("rx", d.rx)
        .attr("ry", d.ry)
        .attr("class", "ellipse")
        .attr("fill", d.fill)
        .attr("id", "ellipse" + i)
        .text("ellipse" + i)
        .on("mouseover", function (event: MouseEvent) {
          mouseover(event);
        })
        .on("mouseout", function (event: MouseEvent) {
          mouseout(event);
        })
        .on("mousedown", function (event: MouseEvent) {
          mousedown(event);
        })
        .on("mouseup", function (event: MouseEvent) {
          mouseup(event);
        })
        .on("click", function (event: MouseEvent) {
          mouseclick(event);
        });
    });

    _svg
      .append("text")
      .attr("fill", "#ff0000")
      .attr("font-weight", "bolder")
      .attr("font-size", "20px")
      .attr("font-family", "sans-serif")
      .attr("y", "100")
      .attr("x", "75")
      .attr("id", "display")
      .text("");

    _dv = d3.select("div");

    _btn = d3.select("button");
    _btn
      .attr("id", "clearlog")
      .attr("class", "btn btn-info")
      .text("log delete")
      .on("click", function () {
        btnclick();
      });
  });

  function btnclick() {
    (dv as HTMLDivElement).textContent = "";
    let dis = Array.from((svg as HTMLElement).querySelectorAll("text")).find(
      (v) => v.id === "display"
    );
    dis.textContent = "";
  }

  function mouseover(event: MouseEvent) {
    let event_source = (event.target as HTMLElement).id;
    let svg_source = Array.from(
      (svg as HTMLElement).querySelectorAll("ellipse")
    ).find((v) => v.id === event_source);

    let txt_x = svg_source.getAttribute("cx");
    let txt_y = svg_source.getAttribute("cy");

    let dis = Array.from((svg as HTMLElement).querySelectorAll("text")).find(
      (v) => v.id === "display"
    );

    dis.textContent = "mouseover";
    dis.setAttribute("x", txt_x);
    dis.setAttribute("y", txt_y);
    (dv as HTMLElement).insertAdjacentHTML("beforeend", "mouseover<br>");
  }

  function mouseout(event: MouseEvent) {
    let event_source = (event.target as HTMLElement).getAttribute("id");
    let svg_source = Array.from(
      (svg as HTMLElement).querySelectorAll("ellipse")
    ).find((v) => v.getAttribute("id") === event_source);

    let txt_x = svg_source.getAttribute("cx");
    let txt_y = svg_source.getAttribute("cy");

    let dis = Array.from((svg as HTMLElement).querySelectorAll("text")).find(
      (v) => v.id === "display"
    );

    dis.textContent = "mouseout";
    dis.setAttribute("x", txt_x);
    dis.setAttribute("y", txt_y);
    (dv as HTMLElement).insertAdjacentHTML("beforeend", "mouseout<br>");
  }

  function mousedown(event: MouseEvent) {
    let event_source = (event.target as SVGElement).getAttribute("id");
    let dv0 = _svg.selectAll("ellipse#" + event_source);
    dv0.transition().duration(10).attr("transform", "translate(0,10)");

    let svg_source = Array.from(
      (svg as HTMLElement).querySelectorAll("ellipse")
    ).find((v) => v.getAttribute("id") === event_source);

    let txt_x = svg_source.getAttribute("cx");
    let txt_y = svg_source.getAttribute("cy");

    let dis = Array.from((svg as HTMLElement).querySelectorAll("text")).find(
      (v) => v.id === "display"
    );

    dis.textContent = "mousedown";
    dis.setAttribute("x", txt_x);
    dis.setAttribute("y", txt_y);

    (dv as HTMLElement).insertAdjacentHTML("beforeend", "mousedown<br>");
  }

  function mouseup(event: MouseEvent) {
    let event_source = (event.target as SVGElement).getAttribute("id");
    let dv0 = _svg.selectAll("ellipse#" + event_source);
    dv0.transition().duration(10).attr("transform", "translate(0,0)");

    let svg_source = Array.from(
      (svg as HTMLElement).querySelectorAll("ellipse")
    ).find((v) => v.getAttribute("id") === event_source);

    let txt_x = svg_source.getAttribute("cx");
    let txt_y = svg_source.getAttribute("cy");

    let dis = Array.from((svg as HTMLElement).querySelectorAll("text")).find(
      (v) => v.id === "display"
    );

    dis.textContent = "mouseup";
    dis.setAttribute("x", txt_x);
    dis.setAttribute("y", txt_y);

    (dv as HTMLElement).insertAdjacentHTML("beforeend", "mouseup<br>");
  }

  function mouseclick(event: MouseEvent) {
    let event_source = (event.target as HTMLElement).getAttribute("id");
    let svg_source = Array.from(
      (svg as HTMLElement).querySelectorAll("ellipse")
    ).find((v) => v.getAttribute("id") === event_source);

    let txt_x = svg_source.getAttribute("cx");
    let txt_y = svg_source.getAttribute("cy");

    let dis = Array.from((svg as HTMLElement).querySelectorAll("text")).find(
      (v) => v.id === "display"
    );

    dis.textContent = "click";
    dis.setAttribute("x", txt_x);
    dis.setAttribute("y", txt_y);
    (dv as HTMLElement).insertAdjacentHTML("beforeend", "click<br>");
  }
</script>

<svg bind:this={svg} />
<div bind:this={dv} id="log" />
<button bind:this={btn} />

<style>
  #log {
    border: 1px solid #ccc;
    height: 200px;
    overflow: scroll;
  }
</style>
