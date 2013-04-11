---
title: rNVD3
subtitle: Interactive Charts from R using NVD3.js
author: Ramnath Vaidyanathan
github:
  user: ramnathv
  repo: rNVD3
framework: minimal
mode: selfcontained
widgets: nvd3
hitheme: solarized_light
assets:
  jshead: "http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"
---

## Interactive Charts from R using [NVD3](https://github.com/novus/nvd3)

<style>
.nvd3Plot {
  height: 400px;
}
body {
  background-image: url(libraries/frameworks/minimal/images/light_wool.png)
}
</style>

This R package helps users create interactive visualizations with [NVD3](https://github.com/novus/nvd3), using a plotting interface similar to the [lattice](http://cran.r-project.org/web/packages/lattice/index.html) package.

### Installation

You can install `rNVD3` from `github` using the `devtools` package

```
require(devtools)
install_github('rNVD3', 'ramnathv')
```





### Usage

`rNVD3` uses a plotting interface similar to that of the `lattice` package. Here are a few examples you can try out in your R console.

### Example 1: Discrete Bar Plot


```r
require(rNVD3)
bar1 <- nvd3Plot(~gear, data = mtcars, type = "discreteBarChart", width = 600)
bar1$printChart("chart1")
```

<div id='chart1' class='nvd3Plot'></div>
<script type='text/javascript'>
    $(document).ready(function(){
      drawchart1()
    });
    function drawchart1(){  
      var opts = {"id":"chart1","yAxis":[],"x":"gear","y":"freq","type":"discreteBarChart","width":600,"height":400},
        data = [{"gear":3,"freq":15},{"gear":4,"freq":12},{"gear":5,"freq":5}]
  
      var data = d3.nest()
        .key(function(d){
          return opts.group === undefined ? 'main' : d[opts.group]
        })
        .entries(data)
      
      nv.addGraph(function() {
        var chart = nv.models[opts.type]()
          .x(function(d) { return d[opts.x] })
          .y(function(d) { return d[opts.y] })
          .width(opts.width)
          .height(opts.height)
         
        
          
        
        
        
      
       d3.select("#" + opts.id)
        .append('svg')
        .datum(data)
        .transition().duration(500)
        .call(chart);

       nv.utils.windowResize(chart.update);
       return chart;
      });
    };
</script>



### Example 2: Stacked bar Plot


```r
hair_eye = subset(as.data.frame(HairEyeColor), Sex == "Female")
p1 <- nvd3Plot(Freq ~ Hair | Eye, data = hair_eye, type = 'multiBarChart', width = 600)
p1$chart(color = c('brown', 'blue', '#594c26', 'green'))
p1$printChart('chart2')
```

<div id='chart2' class='nvd3Plot'></div>
<script type='text/javascript'>
    $(document).ready(function(){
      drawchart2()
    });
    function drawchart2(){  
      var opts = {"id":"chart2","yAxis":[],"x":"Hair","y":"Freq","group":"Eye","type":"multiBarChart","width":600,"height":400},
        data = [{"Hair":"Black","Eye":"Brown","Sex":"Female","Freq":36},{"Hair":"Brown","Eye":"Brown","Sex":"Female","Freq":66},{"Hair":"Red","Eye":"Brown","Sex":"Female","Freq":16},{"Hair":"Blond","Eye":"Brown","Sex":"Female","Freq":4},{"Hair":"Black","Eye":"Blue","Sex":"Female","Freq":9},{"Hair":"Brown","Eye":"Blue","Sex":"Female","Freq":34},{"Hair":"Red","Eye":"Blue","Sex":"Female","Freq":7},{"Hair":"Blond","Eye":"Blue","Sex":"Female","Freq":64},{"Hair":"Black","Eye":"Hazel","Sex":"Female","Freq":5},{"Hair":"Brown","Eye":"Hazel","Sex":"Female","Freq":29},{"Hair":"Red","Eye":"Hazel","Sex":"Female","Freq":7},{"Hair":"Blond","Eye":"Hazel","Sex":"Female","Freq":5},{"Hair":"Black","Eye":"Green","Sex":"Female","Freq":2},{"Hair":"Brown","Eye":"Green","Sex":"Female","Freq":14},{"Hair":"Red","Eye":"Green","Sex":"Female","Freq":7},{"Hair":"Blond","Eye":"Green","Sex":"Female","Freq":8}]
  
      var data = d3.nest()
        .key(function(d){
          return opts.group === undefined ? 'main' : d[opts.group]
        })
        .entries(data)
      
      nv.addGraph(function() {
        var chart = nv.models[opts.type]()
          .x(function(d) { return d[opts.x] })
          .y(function(d) { return d[opts.y] })
          .width(opts.width)
          .height(opts.height)
         
        chart
  .color( ["brown","blue","#594c26","green"] )
          
        
        
        
      
       d3.select("#" + opts.id)
        .append('svg')
        .datum(data)
        .transition().duration(500)
        .call(chart);

       nv.utils.windowResize(chart.update);
       return chart;
      });
    };
</script>



`rNVD3` is merely a wrapper around nvd3.js. More documentation and examples are underway.

### Using with Shiny

rCharts is compatible with Shiny. Here is a link to the [rCharts Shiny App](http://glimmer.rstudio.com/ramnathv/rNVD3).

```r
## server.r
require(rNVD3)
shinyServer(function(input, output) {
  output$myChart <- renderChart({
    hair_eye = as.data.frame(HairEyeColor)
    p6 <- nvd3Plot(Freq ~ Hair | Eye, data = subset(hair_eye, Sex == input$gender), 
      type = input$type, id = 'myChart', width = 800)
    p6$chart(color = c('brown', 'blue', '#594c26', 'green'), stacked = input$stack)
    return(p6)
  })
})

## ui.R
require(rNVD3)
shinyUI(pageWithSidebar(
  headerPanel("rNVD3: Interactive Charts from R using NVD3.js"),
  
  sidebarPanel(
    selectInput(inputId = "gender",
      label = "Choose Gender",
      choices = c("Male", "Female"),
      selected = "Male"),
    selectInput(inputId = "type",
      label = "Choose Chart Type",
      choices = c("multiBarChart", "multiBarHorizontalChart"),
      selected = "multiBarChart"),
    checkboxInput(inputId = "stack",
      label = strong("Stack Bars?"),
      value = FALSE)
  ),
  mainPanel(
    showOutput("myChart")
  )
))
```

### Credits

Most of the implementation in `rNVD3` is inspired by [rHighcharts](https://github.com/metagraf/rHighcharts) and [rVega](https://github.com/metagraf/rVega). I have reused some code from these packages verbatim, and would like to acknowledge the efforts of its author [Thomas Reinholdsson](https://github.com/reinholdsson).

### License

`rNVD3` is licensed under the MIT License. NVD3 is licensed under Apache License, Version 2.0. You can read  Read more about its license [here](https://github.com/novus/nvd3/blob/master/LICENSE.md)


