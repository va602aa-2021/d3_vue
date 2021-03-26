<template>
  <div id="app">
    <h1>D3 Visualization within Vue</h1>
    <svg width="800" height="600" id="viz"></svg>

  </div>
</template>

<script>
const d3 = require('d3');

export default {
  name: 'App',
  components: {

  },
  mounted(){
    let numbers = [100,250,160,80, 200, 300, 120, 240, 1000];

    // select visual enviroment: SvG
    const svg = d3.select('#viz');

    const scaleLength = d3.scaleLinear()
            .domain([0, d3.max(numbers)])
            .range([0, 600]);
    const lAxis = d3.axisTop(scaleLength);

    const scalePos = d3.scaleBand()
            .domain(d3.range(numbers.length))
            .range([0, 300])
            .round(true)
            .paddingInner(0.05)
            .paddingOuter(0.05);

    // =============== Create g groups ===============
    svg.append('g')
      .attr('class','lAxis')
      .attr('transform', 'translate(20, 20)')
      .call(lAxis);

    const gs = svg.selectAll('g.bars')
      .data(numbers)
      .join('g').attr('class','bars');

    gs.attr('transform', (d, i) => `translate(20, ${30 + scalePos(i)})`);

    gs.append('rect')
            .attr('fill', '#0a8989')
            .attr('height', scalePos.bandwidth())
            .attr('width', scaleLength );

    gs.append('text')
            .text( (d) => d )
            .attr('x', scaleLength)
            .attr('y', scalePos.bandwidth() / 2 );
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
