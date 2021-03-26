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
  data(){
    return {
      numbers: [100,250,160,80, 200],
    }
  },

  mounted(){
    this.refreshChart(this.numbers);
  },

  watch:{
    numbers(newVal){
      this.refreshChart(newVal);
    }
  },
  methods: {
    refreshChart(listOfNumbers){
      // select visual enviroment: SvG
      const svg = d3.select('#viz');

      const scaleLength = d3.scaleLinear()
              .domain([0, d3.max(listOfNumbers)])
              .range([0, 600]);
      const lAxis = d3.axisTop(scaleLength);

      const scalePos = d3.scaleBand()
              .domain(d3.range(listOfNumbers.length))
              .range([0, 300])
              .round(true)
              .paddingInner(0.05)
              .paddingOuter(0.05);

      // =============== Create g groups ===============
      svg.selectAll('g.lAxis')
              .data([0])
              .join('g')
              .attr('class','lAxis')
              .attr('transform', 'translate(20, 20)')
              .call(lAxis);

      const gs = svg.selectAll('g.bars')
              .data(listOfNumbers)
              .join('g').attr('class','bars');

      gs.attr('transform', (d, i) => `translate(20, ${30 + scalePos(i)})`);

      gs.selectAll('rect')
           .data(d => [d])
           .join('rect')
              .attr('fill', '#0a8989')
              .attr('height', scalePos.bandwidth())
              .attr('width', scaleLength );

      gs.selectAll('text')
              .data(d => [d])
              .join('text')
              .text( (d) => d )
              .attr('x', scaleLength)
              .attr('y', scalePos.bandwidth() / 2 );
    }
  },
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
