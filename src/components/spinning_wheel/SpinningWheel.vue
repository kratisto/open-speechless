<template>
    <div>
        <div id="chart"></div>
        <div id="question"><h1></h1></div>
    </div>
</template>

<script>
    import * as d3 from 'd3'
    export default {
        name: 'SpinningWheel',
        data: function () {
            return {
                container: null,
                spinData: [],
            }
        },
        props: {
            subjects: {
                type: Array
            }
        },
        methods : {
            replaceSubject(replace){
                Object.assign(this.subjects[replace.index], replace.subject)
                this.removeGraph()
                this.buildGraph()
            },
            getSubjects(){
                return this.subjects;
            },
            removeGraph(){
                d3.select('#chart svg').remove()
            },
            buildGraph(){
                // Strongly inspired by https://codepen.io/abarres/pen/KKwdNKq
                var padding = {top:20, right:40, bottom:0, left:0},
                    w = 500 - padding.left - padding.right,
                    h = 500 - padding.top  - padding.bottom,
                    r = Math.min(w, h)/2,
                    rotation = 0,
                    oldrotation = 0,
                    picked = 100000,
                    color = d3.scale.category20();
                var data = this.getSubjects();
                var svg = d3.select('#chart')
                    .append("svg")
                    .data([data])
                    .attr("width",  w + padding.left + padding.right)
                    .attr("height", h + padding.top + padding.bottom);
                var container = svg.append("g")
                    .attr("class", "chartholder")
                    .attr("transform", "translate(" + (w/2 + padding.left) + "," + (h/2 + padding.top) + ")");
                var vis = container
                    .append("g");

                var pie = d3.layout.pie().sort(null).value(()=>{return 1;});
                // declare an arc generator function
                var arc = d3.svg.arc().outerRadius(r);
                // select paths, use arc generator to draw
                var arcs = vis.selectAll("g.slice")
                    .data(pie)
                    .enter()
                    .append("g")
                    .attr("class", "slice");
                arcs.append("path")
                    .attr("fill", function(d, i){ return color(i); })
                    .attr("d", function (d) { return arc(d); });
                // add the text
                arcs.append("text").attr("transform", function(d){
                    d.innerRadius = 0;
                    d.outerRadius = r;
                    d.angle = (d.startAngle + d.endAngle)/2;
                    return "rotate(" + (d.angle * 180 / Math.PI - 90) + ")translate(" + (d.outerRadius -10) +")";
                })
                    .attr("text-anchor", "end")
                    .style("font-size", "20px")
                    .text( function(d, i) {
                        return data[i].Nom;
                    });
                container.on("click", ()=> {
                    var  ps       = 360/data.length,
                        rng      = Math.floor((Math.random() * 1440) + 360);

                    rotation = (Math.round(rng / ps) * ps);

                    picked = Math.round(data.length - (rotation % 360)/ps);
                    picked = picked >= data.length ? (picked % data.length) : picked;

                    rotation += 90 - Math.round(ps/2);
                    vis.transition()
                        .duration(3000)
                        .attrTween("transform", ()=> {
                            var i = d3.interpolate(oldrotation % 360, rotation);
                            return function(t) {
                                return "rotate(" + i(t) + ")";
                            };
                        })
                        .each("end", function(){
                            //populate link
                            d3.select("#link h1")
                                .text(data[picked].Lien);
                            oldrotation = rotation;

                        });
                });
                //make arrow
                svg.append("g")
                    .attr("transform", "translate(" + (w + padding.left + padding.right) + "," + ((h/2)+padding.top) + ")")
                    .append("path")
                    .attr("d", "M-" + (r*.15) + ",0L0," + (r*.05) + "L0,-" + (r*.05) + "Z")
                    .style({"fill":"black"});
            }
        },
        computed: {
            formTitle () {
                return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
            },
        },
        watch: {
            dialog (val) {
                val || this.close()
            },
            subjects: function() {
                this.removeGraph();
                this.buildGraph();
            },
        },
        mounted() {
            this.buildGraph()
        }
    }
</script>

<style>
    #chart{
        width:500px;
        height:500px;
        display:inline-block;
    }
    #link{
        display:inline-block;;
    }
    #link h1{
        font-size: 50px;
        font-weight: bold;
        font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
        position: absolute;
        padding: 0;
        margin: 0;
        top:50%;
        -webkit-transform:translate(0,-50%);
        transform:translate(0,-50%);
    }

</style>
