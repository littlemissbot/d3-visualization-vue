<template>
    <div class="bar-chart" v-bind:class="componentClass"></div>
</template>

<script>
import * as d3 from 'd3'

export default {
    name: "BarChart",
    props: ["dataset", "type", "componentClass"],
    methods: {
        horizontalChart: function(dataset) {
            var data = dataset;
            var width = 300;
            // var height = 300;
            var scaleFactor = 20;
            var barHeight = 30;

            var colors = d3.scaleOrdinal(d3.schemeAccent);

            var container = d3.select(`.${this.componentClass}`);

            var graph = container.append("svg")
                .attr("width", width)
                .attr("height", barHeight * data.length);

            var barHorizontal = graph.selectAll('g')
                .data(data)
                .enter()
                .append("g")
                .attr("transform", function(d, i) {
                    return "translate(0," + (i * barHeight) + ")";
                });

            barHorizontal.append("rect")
                .attr("width", function(d) {
                    return (d * scaleFactor);
                })
                .attr("height", barHeight - 10)
                .attr("fill", function(d) {
                    return colors(d);
                });

            barHorizontal.append("text")
                .attr("x", function(d) {
                    return (d * scaleFactor) + 5;
                })
                .attr("y", (barHeight - 10) / 2)
                .attr("dy", ".35em")
                .style("font-size", "12px")
                .style("color", "black")
                .text(function(d) { return d; });
        },
        verticalChart: function(dataset) {
            var data = dataset;
            // var width = 300;
            var height = 300;
            var scaleFactor = 20;
            var barHeight = 30;

            var colors = d3.scaleOrdinal(d3.schemeAccent);

            var container = d3.select(`.${this.componentClass}`);

            var graph = container.append("svg")
                .attr("width", barHeight * data.length)
                .attr("height", height);

            var barVetical = graph.selectAll('g')
                .data(data)
                .enter()
                .append("g")
                .attr("transform", function(d, i) {
                    return "translate(" + i * barHeight + ", 0)";
                });

            barVetical.append("rect")
                .attr("y", function(d) {
                    return height - (d * scaleFactor);
                })
                .attr("x", (barHeight - 10) / 2)
                .attr("height", function(d) {
                    return d * scaleFactor;
                })
                .attr("width", barHeight - 10)
                .attr("fill", function(d) {
                    return colors(d);
                });

            barVetical.append("text")
                .attr("y", function(d) {
                    return height - (d * scaleFactor) - 5;
                })
                .attr("x", (barHeight-10) / 2)
                .attr("dx", ".35em")
                .style("font-size", "12px")
                .style("color", "black")
                .text(function(d) { return d; });
        }
    },
    mounted() {}
}
</script>

<style>
.bar-chart {
    background: #EEE;
    border-radius: 5px;
    margin: 5px;
    padding: 25px 0;
}
</style>