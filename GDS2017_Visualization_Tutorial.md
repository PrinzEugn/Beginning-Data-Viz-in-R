R Plotting and Mapping Tutorial
================
Mark Simpson
January 30, 2017

<table style="width:4%;">
<colgroup>
<col width="4%" />
</colgroup>
<tbody>
<tr class="odd">
<td># Meta-introduction February 5, 2018 This tutorial was originally created for the <a href="https://sites.psu.edu/gds2017/">Geospatial Data Science 2017 workshop</a>, which I co-organized along with Dr. Eun-Kyeong Kim, and which was help February 3rd-5th, 2017 on the Penn State campus. It was originally given as part of a live tutorial. I have slightly updated the formatting, but it is presented as-is.</td>
</tr>
<tr class="even">
<td># Introduction</td>
</tr>
<tr class="odd">
<td>This tutorial is meant to give a very basic overview of plotting data and mapping for R. We will briefly get sense of basic R plotting using the default plotting functions. We'll also cover the basics such as how to import and make maps with point, line, and polygon data, and then make those maps not suck. This tutorial is not meant to be comprehensive (how can it be when a new package comes out every minute?), but is meant to get you started so can start building your own workflow.</td>
</tr>
<tr class="even">
<td>The general outline is:</td>
</tr>
<tr class="odd">
<td>* Basic Plots with Default R * Scratch Surface of ggplot * Mapping with GISTools * Using Real Data</td>
</tr>
<tr class="even">
<td># Working with Color</td>
</tr>
<tr class="odd">
<td>Color is fundamental to making usable, understandable, and beautiful visualizations, and is where we are going to start.</td>
</tr>
<tr class="even">
<td>Before we can mess with color, we need some graphic, and to get a graphic, it'd be best to get some data. So, we will load in a built-in dataset within R called <em>mtcars</em> (car statistics, details (here)[<a href="http://stat.ethz.ch/R-manual/R-devel/library/datasets/html/mtcars.html" class="uri">http://stat.ethz.ch/R-manual/R-devel/library/datasets/html/mtcars.html</a>], and extract some values to then graph.</td>
</tr>
<tr class="odd">
<td>```r rm(list = ls()) # Clear environment</td>
</tr>
<tr class="even">
<td># Get some data to play with data(mtcars) # Built-in dataset, motor trend car testing head(mtcars) # First 5 rows ```</td>
</tr>
<tr class="odd">
<td><code>##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb ## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 ## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 ## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1 ## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1 ## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 ## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1</code></td>
</tr>
<tr class="even">
<td><code>r mtcars$mpg # Get a column</code></td>
</tr>
<tr class="odd">
<td><code>##  [1] 21.0 21.0 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 17.8 16.4 17.3 15.2 ## [15] 10.4 10.4 14.7 32.4 30.4 33.9 21.5 15.5 15.2 13.3 19.2 27.3 26.0 30.4 ## [29] 15.8 19.7 15.0 21.4</code></td>
</tr>
<tr class="even">
<td><code>r mpg &lt;- mtcars$mpg[1:5] # Put first five values into new array mpg # Check results</code></td>
</tr>
<tr class="odd">
<td><code>## [1] 21.0 21.0 22.8 21.4 18.7</code></td>
</tr>
<tr class="even">
<td><code>r mpg &lt;- mtcars[1:5, 1] # Same as this mpg # Check Results</code></td>
</tr>
<tr class="odd">
<td><code>## [1] 21.0 21.0 22.8 21.4 18.7</code></td>
</tr>
<tr class="even">
<td>Now that we have a simple piece of data, it's easy to graph it. R's default <em>plot()</em> function will make an attempt to graph many types of datasets, but often more specialized functions are available.</td>
</tr>
<tr class="odd">
<td><code>r plot(mpg) # R makes a guess with just plot()</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-2-1.png" /></td>
</tr>
<tr class="odd">
<td><em>barplot()</em> is an example of a more specialized function. For our purposes in understanding color, let's just stick with it.</td>
</tr>
<tr class="even">
<td><code>r barplot(mpg) # More specific function</code></td>
</tr>
<tr class="odd">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-3-1.png" /></td>
</tr>
<tr class="even">
<td>There are several different ways to specify color manually, but R also has a predefined list of colors that you can call by name. While you can look this up within R, the easiest way is to use an external reference, such as this cheat (sheet)[<a href="http://research.stowers-institute.org/efg/R/Color/Chart/ColorChart.pdf" class="uri">http://research.stowers-institute.org/efg/R/Color/Chart/ColorChart.pdf</a>].</td>
</tr>
<tr class="odd">
<td>```r # colors() # All named colors in R</td>
</tr>
<tr class="even">
<td>head(colors()) # First five ```</td>
</tr>
<tr class="odd">
<td><code>## [1] &quot;white&quot;         &quot;aliceblue&quot;     &quot;antiquewhite&quot;  &quot;antiquewhite1&quot; ## [5] &quot;antiquewhite2&quot; &quot;antiquewhite3&quot;</code></td>
</tr>
<tr class="even">
<td><code>r colors()[599] # Specific color in list</code></td>
</tr>
<tr class="odd">
<td><code>## [1] &quot;slategray&quot;</code></td>
</tr>
<tr class="even">
<td>We can then assign any of these colors to our graph, which can be done in multiple ways, such as by named color, rgb value, or even the hex value of the color.</td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = &quot;slategray&quot;) # Here is our slategray barplot</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-5-1.png" /></td>
</tr>
<tr class="odd">
<td><code>r col2rgb(&quot;slategray&quot;)  # Yields RGB values</code></td>
</tr>
<tr class="even">
<td><code>##       [,1] ## red    112 ## green  128 ## blue   144</code></td>
</tr>
<tr class="odd">
<td><code>r #?rgb</code> Here are examples in RGB:</td>
</tr>
<tr class="even">
<td>Values are between 0-1, here red is at max value, which is the reddest red you can red.</td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = rgb(1, .0, .0))</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-6-1.png" /></td>
</tr>
<tr class="odd">
<td>Here red is at half value, see the difference?</td>
</tr>
<tr class="even">
<td><code>r barplot(mpg, col = rgb(.5, .0, .0))</code></td>
</tr>
<tr class="odd">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-7-1.png" /></td>
</tr>
<tr class="even">
<td>Red with alpha channel set for 25% opacity:</td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = rgb(.5, .0, .0, .25))</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-8-1.png" /></td>
</tr>
<tr class="odd">
<td>This has the range changed to 0-255, more common in graphics software like Photoshop.</td>
</tr>
<tr class="even">
<td><code>r barplot(mpg, col = rgb(255, 0, 0, max = 255))</code></td>
</tr>
<tr class="odd">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-9-1.png" /></td>
</tr>
<tr class="even">
<td>RGB Hexcodes are somewhat more common (RGB on the 0-255 scale, as FF in hex equals 255 in decimal) :</td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = &quot;#FF0000&quot;)  # Note the values (FF, 00, 00)</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-10-1.png" /></td>
</tr>
<tr class="odd">
<td>Colors can also be stored within an array and used within other functions, which can be used for categorical variables. R also has several built-in color &quot;palettes&quot; that can be accessed.</td>
</tr>
<tr class="even">
<td><code>r barplot(mpg, col = c(&quot;red&quot;, &quot;blue&quot;))  # Colors will cycle</code></td>
</tr>
<tr class="odd">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-11-1.png" /></td>
</tr>
<tr class="even">
<td>You can also store several colors in vector, like so:</td>
</tr>
<tr class="odd">
<td>```r red.blue &lt;- c(&quot;red&quot;, &quot;blue&quot;) # Vector so store color information</td>
</tr>
<tr class="even">
<td>barplot(mpg, col = red.blue) ```</td>
</tr>
<tr class="odd">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-12-1.png" /></td>
</tr>
<tr class="even">
<td>There are also several built-in color pallets available in R...</td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = 1:6)</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-13-1.png" /></td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = rainbow(6))</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-14-1.png" /></td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = heat.colors(6))</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-15-1.png" /></td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = terrain.colors(6))</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-16-1.png" /></td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = topo.colors(6))</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-17-1.png" /></td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = cm.colors(6))</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-18-1.png" /></td>
</tr>
<tr class="odd">
<td>While these built-in colors are useful, they are not necessarily pretty, easily printable, or color-blind friendly. Luckily, someone has already created a fantastic set up colors tailored to data visualization.</td>
</tr>
<tr class="even">
<td><em>RColorBrewer</em> is based off of the tool (Color Brewer)[<a href="http://colorbrewer2.org/" class="uri">http://colorbrewer2.org/</a>], created by Dr. Cindy Brewer, head of the PSU Department of Geography, and a classy lady. Originally conceived for maps, it is very useful for all kinds of data, as we shall see.</td>
</tr>
<tr class="odd">
<td>```r # install.packages(&quot;RColorBrewer&quot;)</td>
</tr>
<tr class="even">
<td>library(&quot;RColorBrewer&quot;)</td>
</tr>
<tr class="odd">
<td># List all palettes and attributes # brewer.pal.info # Note the attributes</td>
</tr>
<tr class="even">
<td>display.brewer.all() # Actually display those palettes ```</td>
</tr>
<tr class="odd">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-19-1.png" /></td>
</tr>
<tr class="even">
<td><code>r #Display only those that are colorblind friendly display.brewer.all( colorblindFriendly = TRUE)</code></td>
</tr>
<tr class="odd">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-19-2.png" /></td>
</tr>
<tr class="even">
<td>```r # Display a specific number of colors for a palettes</td>
</tr>
<tr class="odd">
<td>display.brewer.pal(7,&quot;BrBG&quot;) # Display a sequence of seven colors from BrBG ```</td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-19-3.png" /></td>
</tr>
<tr class="odd">
<td><code>r display.brewer.pal(9,&quot;PiYG&quot;) # Display a sequence of nine colors from PiYG</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-19-4.png" /></td>
</tr>
<tr class="odd">
<td><code>r brewer.pal(7,&quot;BrBG&quot;) # The actual function to use, note it returns Hex codes</code></td>
</tr>
<tr class="even">
<td><code>## [1] &quot;#8C510A&quot; &quot;#D8B365&quot; &quot;#F6E8C3&quot; &quot;#F5F5F5&quot; &quot;#C7EAE5&quot; &quot;#5AB4AC&quot; &quot;#01665E&quot;</code></td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = brewer.pal(5,&quot;BrBG&quot;)) # BrBG applied to our barplot</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-19-5.png" /></td>
</tr>
<tr class="odd">
<td><code>r barplot(mpg, col = brewer.pal(5,&quot;Set2&quot;)) # Set2 applied to our barplot</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-19-6.png" /></td>
</tr>
</tbody>
</table>

Plotting (Statistical Graphs)
=============================

Okay, now let's get to some real plotting. First, how to get that bar chart into something sensible? First, we need to do a little data processing to get the appropriate information for a bar chart. Barplot works off of a *table*, which can be created using a function of the same name.

``` r
head(mtcars) # Reminder of the data we have
```

    ##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
    ## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
    ## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

``` r
cyl.table <- table(mtcars$cyl) # Create a summary table of the cyl variable in mtcars
cyl.table # Check results
```

    ## 
    ##  4  6  8 
    ## 11  7 14

``` r
barplot(cyl.table) # Not fancy, but there are many, many other options...
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-20-1.png)

One important thing to remember is that when you do not specify a parameter, you are often just using what is set as the default. For instance, the last line above is really passing this to R:

``` r
# ?barplot

barplot(cyl.table, width = 1, 
        space = NULL,
        names.arg = NULL, 
        legend.text = NULL, 
        beside = FALSE,
        horiz = FALSE, 
        density = NULL, 
        angle = 45,
        col = NULL, 
        border = par("fg"),
        main = NULL, 
        sub = NULL, 
        xlab = NULL, ylab = NULL,
        xlim = NULL, ylim = NULL, 
        xpd = TRUE, 
        log = "",
        axes = TRUE, 
        axisnames = TRUE,
        cex.axis = par("cex.axis"), 
        cex.names = par("cex.axis"),
        plot = TRUE, 
        axis.lty = 0, 
        offset = 0,
        add = FALSE, 
        args.legend = NULL)
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-21-1.png)

This is why using the help (?) is so critical, because it will tell you what you can change. Let's experiment just a little bit with some of these options:

``` r
# Note that you can split functions across lines and comment specific parts separately
barplot(cyl.table, 
       # density = c(20, 5, -1), # Hatched fill, negative means no hatching
        space = 1.5, # Spaces between bars
        horiz = TRUE, # Flip them bars
        las = 1,  # Orientation of axis labels
        ylab = "Number of Cylinders",
        col = brewer.pal(3,"Set1"),
        border = NA,  # Remove border on bars
        main = "Engine Cylinder Counts",  # Title
        xlab = "Number of Cars with Cylinder Type")
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-22-1.png) \#\# Par()

As you can see, our graph is neat, but our labels are a little too close to the edges. These and many options like these are contained not with individual plot functions, but within the plotting system itself, which can be set by using the *par()* (parameters) function. A good description of many of thehttp://www.statmethods.net/advgraphs/parameters.html can be found at Quick-R (here)\[<http://www.statmethods.net/advgraphs/parameters.html>\].

*par()* is great if you need to pump out a bunch of graphs since you only need to change them once. On the other hand, it can be slightly annoying because there is no built-in way to restore the default values, so it's best to store them in a variable.

``` r
# par() # This will list the current settings
par()
```

    ## $xlog
    ## [1] FALSE
    ## 
    ## $ylog
    ## [1] FALSE
    ## 
    ## $adj
    ## [1] 0.5
    ## 
    ## $ann
    ## [1] TRUE
    ## 
    ## $ask
    ## [1] FALSE
    ## 
    ## $bg
    ## [1] "white"
    ## 
    ## $bty
    ## [1] "o"
    ## 
    ## $cex
    ## [1] 1
    ## 
    ## $cex.axis
    ## [1] 1
    ## 
    ## $cex.lab
    ## [1] 1
    ## 
    ## $cex.main
    ## [1] 1.2
    ## 
    ## $cex.sub
    ## [1] 1
    ## 
    ## $cin
    ## [1] 0.15 0.20
    ## 
    ## $col
    ## [1] "black"
    ## 
    ## $col.axis
    ## [1] "black"
    ## 
    ## $col.lab
    ## [1] "black"
    ## 
    ## $col.main
    ## [1] "black"
    ## 
    ## $col.sub
    ## [1] "black"
    ## 
    ## $cra
    ## [1] 14.4 19.2
    ## 
    ## $crt
    ## [1] 0
    ## 
    ## $csi
    ## [1] 0.2
    ## 
    ## $cxy
    ## [1] 0.02604167 0.06329116
    ## 
    ## $din
    ## [1] 6.999999 4.999999
    ## 
    ## $err
    ## [1] 0
    ## 
    ## $family
    ## [1] ""
    ## 
    ## $fg
    ## [1] "black"
    ## 
    ## $fig
    ## [1] 0 1 0 1
    ## 
    ## $fin
    ## [1] 6.999999 4.999999
    ## 
    ## $font
    ## [1] 1
    ## 
    ## $font.axis
    ## [1] 1
    ## 
    ## $font.lab
    ## [1] 1
    ## 
    ## $font.main
    ## [1] 2
    ## 
    ## $font.sub
    ## [1] 1
    ## 
    ## $lab
    ## [1] 5 5 7
    ## 
    ## $las
    ## [1] 0
    ## 
    ## $lend
    ## [1] "round"
    ## 
    ## $lheight
    ## [1] 1
    ## 
    ## $ljoin
    ## [1] "round"
    ## 
    ## $lmitre
    ## [1] 10
    ## 
    ## $lty
    ## [1] "solid"
    ## 
    ## $lwd
    ## [1] 1
    ## 
    ## $mai
    ## [1] 1.02 0.82 0.82 0.42
    ## 
    ## $mar
    ## [1] 5.1 4.1 4.1 2.1
    ## 
    ## $mex
    ## [1] 1
    ## 
    ## $mfcol
    ## [1] 1 1
    ## 
    ## $mfg
    ## [1] 1 1 1 1
    ## 
    ## $mfrow
    ## [1] 1 1
    ## 
    ## $mgp
    ## [1] 3 1 0
    ## 
    ## $mkh
    ## [1] 0.001
    ## 
    ## $new
    ## [1] FALSE
    ## 
    ## $oma
    ## [1] 0 0 0 0
    ## 
    ## $omd
    ## [1] 0 1 0 1
    ## 
    ## $omi
    ## [1] 0 0 0 0
    ## 
    ## $page
    ## [1] TRUE
    ## 
    ## $pch
    ## [1] 1
    ## 
    ## $pin
    ## [1] 5.759999 3.159999
    ## 
    ## $plt
    ## [1] 0.1171429 0.9400000 0.2040000 0.8360000
    ## 
    ## $ps
    ## [1] 12
    ## 
    ## $pty
    ## [1] "m"
    ## 
    ## $smo
    ## [1] 1
    ## 
    ## $srt
    ## [1] 0
    ## 
    ## $tck
    ## [1] NA
    ## 
    ## $tcl
    ## [1] -0.5
    ## 
    ## $usr
    ## [1] 0 1 0 1
    ## 
    ## $xaxp
    ## [1] 0 1 5
    ## 
    ## $xaxs
    ## [1] "r"
    ## 
    ## $xaxt
    ## [1] "s"
    ## 
    ## $xpd
    ## [1] FALSE
    ## 
    ## $yaxp
    ## [1] 0 1 5
    ## 
    ## $yaxs
    ## [1] "r"
    ## 
    ## $yaxt
    ## [1] "s"
    ## 
    ## $ylbias
    ## [1] 0.2

``` r
o.par <- par(no.readonly = T) # Store current setting in new variable, but not read only ones

par(mar = c(6,6,6,2)) # Set new margins, (bottom, left, top, right)

# Plot the same thing, but notice the left axis label
barplot(cyl.table, 
        space = 1.5, # Spaces between bars
        horiz = TRUE, # Flip them bars
        las = 1,  # Orientation of axis labels
        ylab = "Number of Cylinders",
        col = brewer.pal(3,"Set1"),
        border = NA,  # Remove border on bars
        main = "Engine Cylinder Counts",  # Title
        xlab = "Number of Cars with Cylinder Type")
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-23-1.png) Here par sets axis fonts to gray60, and bolds text.

``` r
par(col.axis="gray60", font.axis = 2) 

barplot(cyl.table, 
        space = 1.5, # Spaces between bars
        horiz = TRUE, # Flip them bars
        las = 1,  # Orientation of axis labels
        ylab = "Number of Cylinders",
        col = brewer.pal(3,"Set1"),
        border = NA,  # Remove border on bars
        main = "Engine Cylinder Counts",  # Title
        xlab = "Number of Cars with Cylinder Type")
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-24-1.png)

``` r
par(o.par) # Restore defaults
```

Helper Functions
----------------

However, not everything is contained within *par()*, and if you need to get even more particular, there are specialized functions for different graph elements. For example, the color of the tick marks themselves. To change these, we can construct a custom axis using the *axis()* function, which operates on a pre-constructed plot, ideally one without axes, which we can do by suppressing those options. Similarly, we can not specify a title and create one with the *title()* function.

Other examples include adding text annotations, adding reference lines (such as a global average), and a legend which explains visual attributes like colors (important for maps). More information can be found (here)\[<http://www.statmethods.net/advgraphs/axes.html>\].

But for now, title and axis:

``` r
par(mar = c(6,6,6,2), col.axis="gray60", font.axis = 2) # All the stuff we did before

#Let's make a plot without an axis at the bottom or a title.
barplot(cyl.table,
        xaxt ="n", # Supress x axis. yaxt for y, axes = FALSE does both
        space = 1.5, # Spaces between bars
        las = 1,
        horiz = TRUE, # Flip them bars
        ylab = "Number of Cylinders",
        col = brewer.pal(3,"Set1"),
        border = NA,  # Remove border on bars
        xlab = "Number of Cars with Cylinder Type")

# 1=bottom, 2=left, 3=top, 4=right
axis(1, col = "gray50", lty = 3) # 1 indicates position, lty is line type

title(main ="Engine Cylinder Counts", 
      col.main = "paleturquoise4", # Set color of main title 
      cex.main = 2, # Set relative size of title
      sub = "Aw yeah this is a dope subtitle", # Subtitle
      col.sub = "gray60")

# Title also contains options for axis labels
title(ylab = "More Y Label for some reason", 
      line = 2, # Change position in terms of text lines
      col.lab = "red")
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-25-1.png)

``` r
par(o.par) # Rest par() to original
```

Histograms
----------

Histograms are similar in looks to barplots, and often more useful. Here we will also add a line represented the mean along with a label, and a line representing density.

``` r
hist(mtcars$qsec,
     main="Distribution of Time to Quarter-mile",
     xlab="Time (Seconds)", 
     xlim = c(14,24), # Force x axis limits 
     col= rev(brewer.pal(9, "Spectral")) ) # Same number of colors; really chartjunk

abline(v = mean(mtcars$qsec), lty = 2, col = "turquoise4") # Create a line at average qsec

text(18.7, 9, labels ="Mean", col = "turquoise4") # Create a text label at specific x, y

lines(density(mtcars$qsec), col = "darkred", lwd = 2) # Kernel density line
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-26-1.png)

We can also alter the amount of breaks:

``` r
hist(mtcars$qsec,
     breaks = 5, # Set number of breaks
     main="Distribution of Time to Quarter-mile",
     xlab="Time (Seconds)", 
     col= "gray80" )

abline(v=mean(mtcars$qsec), lty = 2, col = "turquoise4") # Create a line at average qsec

text(18.7, 12, labels ="Mean", col = "turquoise4") # Note the different x, y

lines(density(mtcars$qsec), col = "darkred", lwd = 2) # Kernel density line
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-27-1.png) \#\# Boxplots

Boxplots are THE statistical graphs. So let's make some. We can make a boxplot for that qsec (time to a quarter mile) variable...

``` r
boxplot(mtcars$qsec, ylab = "Time to Quarter Mile") # All cars qsex
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-28-1.png)

...but what would be more interesting is to see it between cars with different cylinder engines. Well, sort of interesting in that it should be pretty obvious.

We can also use *legend* to give the same information as the bottom axis, but in a slightly different way.

``` r
four.qsec <- mtcars$qsec[mtcars$cyl == 4] # qsec for rows where cyl = 4 
six.qsec <- mtcars$qsec[mtcars$cyl == 6] # qsec for rows where cyl = 6 
eight.qsec <- mtcars$qsec[mtcars$cyl == 8] # qsec for rows where cyl = 8 
eight.qsec # Check results
```

    ##  [1] 17.02 15.84 17.40 17.60 18.00 17.98 17.82 17.42 16.87 17.30 15.41
    ## [12] 17.05 14.50 14.60

``` r
boxplot(four.qsec, six.qsec, eight.qsec) # Wow! Graphs! but not labelled, and not pretty.
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-29-1.png)

Now, to make it pretty:

``` r
boxplot(four.qsec, six.qsec, eight.qsec, # Data
        ylim = c(14,24), # Set Y axis limits manually
        xaxt = "n",
       # names = c("Four","Six", "Eight"), # Need to provide names since our var are simple arrays
        col = brewer.pal(3, "Set2"),   # Tasteful colors for boxes
        boxwex = 0.5,  # Width of box (as proportion of original)
        whisklty = 1,  # Whisker line type; 1 = solid line
        staplelty = 0,  # Staple (line at end) type, 0 is none
        outpch = 16,  # Symbols for outliers
        outcol =  brewer.pal(3, "Set2"), # Colors for outliers
        main = "Quarter Mile Time and Engine Type",
        col.main = "gray40", # Color of title
        ylab = "Time in Seconds",
        frame.plot = FALSE # Take away the whole frame
        )

# Create a legend
legend("topright", # Location (can also be x, y)
       legend = c("Four","Six", "Eight"), # Labels
       fill = brewer.pal(3, "Set2"), # Same colors as above
       bty = "n", # Remove box around legend
       title = "Number of Cylinders",
       title.col = "gray40",
       text.font = 3, # Italic
       cex = 1.2) # Make a little bigger

abline(h = mean(mtcars$qsec), lty = 2, col = "gray50") # Line for global mean
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-30-1.png) \#\# Scatterplot

Okay, now let's work with the simple scatter plot. The trick is altering the point symbols, which have totally different parameters attached.

Simple example, two variables in same table:

``` r
plot(mtcars$hp, mtcars$mpg) 
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-31-1.png)

Points symbol types can be changed with *pch* options:

``` r
plot(mtcars$hp, mtcars$mpg, pch = 18) # Diamond points
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-32-1.png)

You can also use these disgusting things (*pch = 13*). What are they?

``` r
plot(mtcars$hp, mtcars$mpg, pch = 13) 
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-33-1.png)

Yeah, so that's not that exciting, but there's a lot we can add for it to be more interesting. For one, it would be cool to see which dots correspond to which type of engine. To do that, however, we need to do a little bit of wrangling to create a new field that is a factor, which we can then use as our categorical engine variable.

``` r
mtcars$cyl.2 <- as.factor(mtcars$cyl) # Create a new field so we can use unclass()
mtcars$cyl.2 # Check results, note it has "levels"
```

    ##  [1] 6 6 4 6 8 6 8 4 4 6 6 8 8 8 8 8 8 4 4 4 4 8 8 8 8 4 4 4 8 6 8 4
    ## Levels: 4 6 8

``` r
# This essentially assigns color per category
# c("#00008B96", "#00640096", "#8B000096")[unclass(mtcars$cyl.2)]
```

``` r
plot(mtcars$hp, mtcars$mpg,
     pch= c(15, 16, 17)[unclass(mtcars$cyl.2)], # Change type of point
     col = c("#00008B96", "#00640096", "#8B000096")[unclass(mtcars$cyl.2)], #Semi-transparent colors, assigned per cyl.2 type
     main ="Horsepower versus Fuel Efficiency",
     col.main = "steelblue",
     cex.main = 1.5, # Increase size
     xlab = "Horsepower", # x label
     ylab = "Miles per Gallon", # y label 
     font.lab = 2 # make bold
     ) 

legend("topright", # Location (can also be x, y)
       legend = c("Four","Six", "Eight"), # Labels
       col = c("#00008B96", "#00640096", "#8B000096"), # Same colors as above
       pch = c(15, 16, 17),
       bty = "n", # Remove box around legend
       title = "Number of Cylinders",
       title.col = "gray40",
       cex = 1) # Make a little bigger
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-35-1.png) \#\# (Not very good) Lines

Line charts are slightly involved in R for some reason, since you effectively have to create a graph, then add lines for each variable. This example is pretty notional since this data doesn't really contain anything that makes particular sense to use a line graph for, like change over time.

``` r
plot(six.qsec, 
     type ="o", # Type can specify particular types of graph to force
     col = "blue", 
     pch = 18, # Set symbol to diamond
     main = "Speed... over Index?"
      )

# Add second line
lines(eight.qsec, 
      type = "o", 
      col = "red", 
      pch = 19,
      lty =4)

# Add legenve
legend("topright", 
       legend = c("Six Cylinder", "Eight Cylinder"), 
       col = c("blue", "red"), 
       lty =c(1, 4) # Need to specify line type in order to force lines
       )
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-36-1.png)

Scatterplot matrices I'll cover quickly, but they can be useful for looked at a lot of data at once. However, often they are impenetrable for anyone not immersed in your data already.

``` r
pairs(mtcars[c(1,4,7)], 
       col.axis = "blue",
       fg ="blue",
       pch = c(15, 16, 17)[unclass(mtcars$cyl.2)], 
       col = c("#00008B96", "#00640096", "#8B000096")[unclass(mtcars$cyl.2)]
       )
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-37-1.png) \#\# Multiple Plotting with mfrow()

As a bonus, you can place multiple graphs in the same plot by changing the mfrow option in *par()*

``` r
par(mfrow=c(1,2))

boxplot(mtcars$qsec, # All cars qsex
        boxwex = 0.5,  # Width of box (as proportion of original)
        whisklty = 1,  # Whisker line type; 1 = solid line
        staplelty = 0,  # Staple (line at end) type, 0 is none
        outpch = 16,  # Symbols for outliers
        col = "slategray",
        outcol =  "slategray", # Colors for outliers
        ylab = "Time to Quarter Mile", 
        main = "Quarter Mile Time and Engine Type",
        col.main = "steelblue", # Color of title
        frame.plot = FALSE # Take away the whole frame
        )

plot(mtcars$hp, mtcars$mpg,
     pch= 19, # Change type of point
     col = c("#00008B96", "#00640096", "#8B000096")[unclass(mtcars$cyl.2)], #Semi-transparent colors, assigned per cyl.2 type
     main ="Horsepower versus Fuel Efficiency",
     col.main = "steelblue",
     cex.main = 1.5, # Increase size
     xlab = "Horsepower", # x label
     ylab = "Miles per Gallon", # y label 
     font.lab = 2 # make bold
     ) 
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-38-1.png)

``` r
par(o.par) # Reset par
```

Exporting in Brief
------------------

You can save plots through R studio, but you can also write code to do it, which is useful when you have multiple plots to generate. There are several different filetypes available, and the directory can be specified more directly, but the principle is that you call the specif function, create the plot, and then use *dev.off()* to stop 'plotting' and actually export the file.

``` r
png(filename= "Neat.png")  # "Open device""

plot(mtcars$hp, mtcars$mpg,
     pch= 19, # Change type of point
     col = c("#00008B96", "#00640096", "#8B000096")[unclass(mtcars$cyl.2)], #Semi-transp. colors, assigned per cyl.2 type
     main ="Horsepower versus Fuel Efficiency",
     col.main = "steelblue",
     cex.main = 1.5, # Increase size
     xlab = "Horsepower", # x label
     ylab = "Miles per Gallon", # y label 
     font.lab = 2 # make bold
     ) 

dev.off()  # Close "device" (end writing)
```

    ## png 
    ##   2

<table style="width:19%;">
<colgroup>
<col width="19%" />
</colgroup>
<tbody>
<tr class="odd">
<td># Scratching Surface of ggplot</td>
</tr>
<tr class="even">
<td>Okay let's take a brief dip into <em>ggplot</em>, which is widely used as it offers some advantages over R's default plotting in terms of customization. The downside is that it takes a fairly different approach to actually creating graphs which is much more layer-based, which can be somewhat mind-bending. Today we're going to start by recreating some of the graphs we have completed above using the <em>qplot()</em> (quick plot) function, based off those found (here)[<a href="http://www.statmethods.net/advgraphs/ggplot2.html" class="uri">http://www.statmethods.net/advgraphs/ggplot2.html</a>].</td>
</tr>
<tr class="odd">
<td>```r rm(mpg) # Remove object since it has conflicting name</td>
</tr>
<tr class="even">
<td># install.packages(&quot;ggplot2&quot;) library(ggplot2) ```</td>
</tr>
<tr class="odd">
<td><code>## Warning: package 'ggplot2' was built under R version 3.4.3</code></td>
</tr>
<tr class="even">
<td><code>r # Quickest plot qplot(mpg, data = mtcars) # Assumes histogram, note tip in console</code></td>
</tr>
<tr class="odd">
<td><code>## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-40-1.png" /> This can get real fancy, real fast:</td>
</tr>
<tr class="odd">
<td><code>r qplot(mpg, # This is our variable (Column) data = mtcars, # This is our dataset geom = &quot;histogram&quot;, # Type of geometry (type of plot) binwidth = 5, # Number of bins fill = cyl.2, # Variable setting fill, categorical alpha = I(.5), # Alpha transparency main = &quot;Distribution of Time to Quarter-mile&quot;, xlab = &quot;Time (seconds)&quot;, ylab = &quot;Density&quot; )</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-41-1.png" /> Boxplot:</td>
</tr>
<tr class="odd">
<td><code>r qplot(cyl.2, qsec, # Set x and y. data = mtcars, # Our dataframe, so x and why can reference geom = c(&quot;boxplot&quot;), fill = cyl.2, # Since this is a factor, it assumes categorical main = &quot;Speed and Cylinders&quot;, xlab = &quot;&quot;, # Empty ylab = &quot;Time to Quarter Mile (sec)&quot; )</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-42-1.png" /></td>
</tr>
<tr class="odd">
<td>It's interesting to compare these to the plots we made above. Let's take a look at our scatterplot, and then what <em>qqplot</em> spits out.</td>
</tr>
<tr class="even">
<td>```r ############# Scatterplot repeated from above plot(mtcars<span class="math inline"><em>h</em><em>p</em>, <em>m</em><em>t</em><em>c</em><em>a</em><em>r</em><em>s</em></span>mpg, pch= 19, # Change type of point col = c(&quot;#00008B96&quot;, &quot;#00640096&quot;, &quot;#8B000096&quot;)[unclass(mtcars$cyl.2)], #Semi-transparent colors, assigned per cyl.2 type main =&quot;Horsepower versus Fuel Efficiency&quot;, col.main = &quot;steelblue&quot;, cex.main = 1.5, # Increase size xlab = &quot;Horsepower&quot;, # x label ylab = &quot;Miles per Gallon&quot;, # y label font.lab = 2 # make bold )</td>
</tr>
<tr class="odd">
<td>legend(&quot;topright&quot;, # Location (can also be x, y) legend = c(&quot;Four&quot;,&quot;Six&quot;, &quot;Eight&quot;), # Labels col = c(&quot;#00008B96&quot;, &quot;#00640096&quot;, &quot;#8B000096&quot;), # Same colors as above pch = 19, bty = &quot;n&quot;, # Remove box around legend title = &quot;Number of Cylinders&quot;, title.col = &quot;gray40&quot;, cex = 1) # Make a little bigger ```</td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-43-1.png" /></td>
</tr>
<tr class="odd">
<td>```r #############</td>
</tr>
<tr class="even">
<td># qqplot Scatterplot qplot(hp, mpg, data = mtcars, shape = cyl.2, # Varies with categorical variable (factor) color = cyl.2, # Varies with categorical variable (factor) size=I(3), # I is relative to default (so *3 here) main =&quot;Horsepower versus Fuel Efficiency&quot;, xlab=&quot;Horsepower&quot;, ylab=&quot;Miles per Gallon&quot; ) ```</td>
</tr>
<tr class="odd">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-43-2.png" /></td>
</tr>
<tr class="even">
<td><em>qqplot</em> is fast, but it is not very flexible. For example, on the histogram, you cannot alter the fill or add an outline to the bars. This brings us to just touch ong the real power of ggplot. We don't have much to spend on this unfortunately, there are numerous references that can help you get started, such as this amazing (cheat sheet)[<a href="https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf" class="uri">https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf</a>]</td>
</tr>
<tr class="odd">
<td>By far the most important thing to notice is that ggplot constructs plots using multiple functions as a matter of course, and that the initial function often does nothing but define the plot area, with subsequent functions adding on the actual representations (lines, bars, colors, etc.)</td>
</tr>
<tr class="even">
<td>So we create a canvas based off our data, then &quot;add&quot; the histogram:</td>
</tr>
<tr class="odd">
<td><code>r ggplot(NULL, aes(x = mtcars$qsec)) + geom_histogram() # Create the graphic (with data from above)</code></td>
</tr>
<tr class="even">
<td><code>## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.</code></td>
</tr>
<tr class="odd">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-44-1.png" /></td>
</tr>
<tr class="even">
<td><code>r ggplot(NULL, aes(x = mtcars$qsec)) + geom_histogram(fill = &quot;white&quot;, color = &quot;black&quot;) + # Simple aesthetics geom_density(color = &quot;blue&quot;) + geom_area(stat = &quot;bin&quot;, alpha = .1, fill = &quot;blue&quot;)</code></td>
</tr>
<tr class="odd">
<td><code>## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`. ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-45-1.png" /></td>
</tr>
<tr class="odd">
<td>Scatterplot:</td>
</tr>
<tr class="even">
<td><code>r ggplot(mtcars, aes(x = hp, y = mpg, # x and y shape = cyl.2, # Change shape according to cyl.2 color = cyl.2)) + # Change color according to cyl.2 geom_point(size = 3) + # Create points geom_rug () + # Rug plot geom_smooth(alpha = .2) # regressiony</code></td>
</tr>
<tr class="odd">
<td><code>## `geom_smooth()` using method = 'loess'</code></td>
</tr>
<tr class="even">
<td><code>## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = ## parametric, : pseudoinverse used at 104.65</code></td>
</tr>
<tr class="odd">
<td><code>## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = ## parametric, : neighborhood radius 18.35</code></td>
</tr>
<tr class="even">
<td><code>## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = ## parametric, : reciprocal condition number 5.5839e-017</code></td>
</tr>
<tr class="odd">
<td><code>## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = ## parametric, : There are other near singularities as well. 4270.6</code></td>
</tr>
<tr class="even">
<td><code>## Warning in predLoess(object$y, object$x, newx = if ## (is.null(newdata)) object$x else if (is.data.frame(newdata)) ## as.matrix(model.frame(delete.response(terms(object)), : pseudoinverse used ## at 104.65</code></td>
</tr>
<tr class="odd">
<td><code>## Warning in predLoess(object$y, object$x, newx = if ## (is.null(newdata)) object$x else if (is.data.frame(newdata)) ## as.matrix(model.frame(delete.response(terms(object)), : neighborhood radius ## 18.35</code></td>
</tr>
<tr class="even">
<td><code>## Warning in predLoess(object$y, object$x, newx = if ## (is.null(newdata)) object$x else if (is.data.frame(newdata)) ## as.matrix(model.frame(delete.response(terms(object)), : reciprocal ## condition number 5.5839e-017</code></td>
</tr>
<tr class="odd">
<td><code>## Warning in predLoess(object$y, object$x, newx = if ## (is.null(newdata)) object$x else if (is.data.frame(newdata)) ## as.matrix(model.frame(delete.response(terms(object)), : There are other ## near singularities as well. 4270.6</code></td>
</tr>
<tr class="even">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-46-1.png" /></td>
</tr>
<tr class="odd">
<td>Boxplot equivalent to the one above:</td>
</tr>
<tr class="even">
<td><code>r ggplot(mtcars, aes( x = cyl.2, y = qsec)) + geom_boxplot(aes(fill = cyl.2)) + # Create boxplot, fill by cyl.2 ylab(&quot;Time to Quarter Mile (sec)&quot;)+ xlab(&quot;&quot;)+ scale_fill_discrete(name = &quot;Cylinders&quot;) + # Legend label ggtitle(&quot;Speed and Cylinders&quot;)</code></td>
</tr>
<tr class="odd">
<td><img src="GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-47-1.png" /></td>
</tr>
</tbody>
</table>

Mapping
=======

Okay, now onto mapping things. There are several packages available that will allow you to map spatial data in R with varying degrees of ease and customization, but this

Basic mapping
-------------

These are decent ways to get reasonable maps of an area quickly, but what about actually working with spatial data to make maps from scratch? This is unsurprisingly much more involved, but much of the plotting is not fundamentally different than what we've been dealing with already.

There are many different options for mapping, but we're going to focus on *GIStools*, which depends on several packages, including *maptools*, *rgdal*, and *sp*.

This section is adapted from the book (An Introduction to R for Spatial Analysis and Mapping)\[<https://www.amazon.com/Introduction-Spatial-Analysis-Mapping/dp/1446272958>\].

``` r
rm(list= ls()) # Clear environment

# install.packages("GISTools")
# library(GISTools) # Note all the dependent packages loaded

library(GISTools, suppressPackageStartupMessages("True")) # Lead package without oodles of messages
```

    ## Warning: package 'GISTools' was built under R version 3.4.3

    ## Loading required package: maptools

    ## Warning: package 'maptools' was built under R version 3.4.3

    ## Loading required package: sp

    ## Checking rgeos availability: TRUE

    ## Loading required package: MASS

    ## Loading required package: rgeos

    ## Warning: package 'rgeos' was built under R version 3.4.3

    ## rgeos version: 0.3-26, (SVN revision 560)
    ##  GEOS runtime version: 3.6.1-CAPI-1.10.1 r0 
    ##  Linking to sp version: 1.2-7 
    ##  Polygon checking: TRUE

``` r
data(newhaven) # Convenient collection of data
```

Plotting once these packages (*sp* in particular) are loaded is pretty straightforward; R now knows how to deal with these data types.

``` r
plot(breach) # Points
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-49-1.png)

``` r
plot(roads) # Lines
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-50-1.png)

``` r
plot(blocks) # Polygons
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-51-1.png)

``` r
class(breach) # SpatialPoints
```

    ## [1] "SpatialPoints"
    ## attr(,"package")
    ## [1] "sp"

``` r
class(roads) # SpatialLinesDataFrame
```

    ## [1] "SpatialLinesDataFrame"
    ## attr(,"package")
    ## [1] "sp"

``` r
class(blocks) # SpatialPolygonsDataFrame
```

    ## [1] "SpatialPolygonsDataFrame"
    ## attr(,"package")
    ## [1] "sp"

``` r
# head(blocks) # This is a MESS
head(data.frame(blocks)) # Coerce to dataframe first
```

    ##   NEWH075H_ NEWH075H_I HSE_UNITS OCCUPIED VACANT  P_VACANT P_OWNEROCC
    ## 0         2         69       763      725     38  4.980341   0.393185
    ## 1         3         72       510      480     30  5.882353  20.392157
    ## 2         4         64       389      362     27  6.940874  57.840617
    ## 3         5         68       429      397     32  7.459207  19.813520
    ## 4         6         67       443      385     58 13.092551  80.361174
    ## 5         7        133       588      548     40  6.802721  52.551020
    ##   P_RENTROCC NEWH075P_ NEWH075P_I POP1990  P_MALES P_FEMALES   P_WHITE
    ## 0  94.626474         2        380    2396 40.02504  59.97496  7.095159
    ## 1  73.725490         3        385    3071 39.07522  60.92478 87.105177
    ## 2  35.218509         4        394     996 47.38956  52.61044 32.931727
    ## 3  72.727273         5        399    1336 42.66467  57.33533 11.452096
    ## 4   6.546275         6        404     915 46.22951  53.77049 73.442623
    ## 5  40.646259         7        406    1318 50.91047  49.08953 87.784522
    ##     P_BLACK P_AMERI_ES P_ASIAN_PI  P_OTHER  P_UNDER5    P_5_13  P_14_17
    ## 0 87.020033   0.584307   0.041736 5.258765 12.813022 24.707846 7.888147
    ## 1 10.452621   0.195376   0.521003 1.725822  1.921198  2.474764 0.814067
    ## 2 66.265060   0.100402   0.200803 0.502008 10.441767 13.554217 5.722892
    ## 3 85.553892   0.523952   0.523952 1.946108 10.853293 17.739521 7.709581
    ## 4 24.371585   0.327869   1.420765 0.437158  6.229508  8.633880 2.950820
    ## 5  7.435508   0.758725   0.834598 3.186646  8.725341  8.194234 3.641882
    ##     P_18_24   P_25_34   P_35_44  P_45_54  P_55_64   P_65_74   P_75_UP
    ## 0 12.479132 16.026711  8.555927 5.759599 4.924875  4.048414  2.796327
    ## 1 71.149463  7.359166  4.037773 1.595571 1.758385  3.712146  5.177467
    ## 2  8.835341 17.670683 17.871486 8.734940 5.923695  7.931727  3.313253
    ## 3 12.425150 18.113772 10.853293 9.056886 6.287425  4.266467  2.694611
    ## 4  7.103825 17.267760 16.830601 8.415301 7.431694 14.426230 10.710383
    ## 5 10.091047 29.286798 12.898331 7.814871 7.814871  6.904401  4.628225

The major difference in plotting map elements with *plot()* is that **add = TRUE** is much more common. The idea of layers isn't exactly new for GISers (pronounce as "Jizzers" to annoy them), but might take some getting used to for others. Let's make a quick crime map

``` r
#browseURL("http://research.stowers-institute.org/efg/R/Color/Chart/ColorChart.pdf")

plot(blocks, lwd = 0.5, col = "darkseagreen1", border = "white") # Plot the 'lowest' first.

plot(roads, add = TRUE, col = "slategray3") # Roads on top

plot(breach, pch = 17, add = TRUE, col =add.alpha("#EE2C2C", .7)) # Add transparency
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-53-1.png)

The *Locator* function lets you interact directly with the plot. You click within the plot, and it returns the coordinates of where you clicked. This is a reasonable way to get the coordinates when you are placing things like legends within the plot

To use it, you run the command, then you select points within the plot (however many you want), and then hit ESC. It will return the coordinates to the console, which you can then use to create a polygon.

``` r
# locator() # Get Coordinates (commented for markdown)
plot(blocks, lwd = 0.5, col = "cornsilk", border = "antiquewhite2")
plot(roads, add = TRUE, col = "slategray3") 
plot(breach, pch = 20, add = TRUE, col ="red")

# Add a scale bar, if you're into that
map.scale(xc = 540000, yc = 152000, # Position on map, in map units
          len = miles2ft(2), # Length in feet (2 * 5,280)
          units = "Miles", 
          ndivs = 4, 
          subdiv = 0.5)

# ?map.scale # Plots scale bar on top of map
# ?miles2ft # Unit conversion function, one of many
#locator() # Click once on location, then hit finish button in plot window OR use the Esc key

# North arrow
north.arrow(xb = 540000, yb = 157000, 
            len = miles2ft(.2), # Length of base
            col = "gray60",
            border = "gray30",
            tcol = "gray60") # Color

title(main = 'New Haven, CT.') # Title

title(main = "Crime Infested Wasteland", # Informative subtitle
      line = -.2, # Move down
      col.main = "red3",
      font.main = 4, # Bold italic
      cex.main = 1) # make a little smaller
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-54-1.png)

Choropleth Maps
===============

Okay, this is essentially a basic reference map, but we can also use the attribute data within the spatial classes to make thematic maps, such as choropleth maps. There are many ways to create choropleth maps, including a whole friggin' package called *choroplethr*, but let's use *GISTools* for consistency, which has some functions specifically for that. Let's make two maps with the data we have: percent vacant and percent owner occupied.

``` r
# head(data.frame(blocks)) # Look at our data again

colnames(data.frame(blocks)) # Just the attribute names
```

    ##  [1] "NEWH075H_"  "NEWH075H_I" "HSE_UNITS"  "OCCUPIED"   "VACANT"    
    ##  [6] "P_VACANT"   "P_OWNEROCC" "P_RENTROCC" "NEWH075P_"  "NEWH075P_I"
    ## [11] "POP1990"    "P_MALES"    "P_FEMALES"  "P_WHITE"    "P_BLACK"   
    ## [16] "P_AMERI_ES" "P_ASIAN_PI" "P_OTHER"    "P_UNDER5"   "P_5_13"    
    ## [21] "P_14_17"    "P_18_24"    "P_25_34"    "P_35_44"    "P_45_54"   
    ## [26] "P_55_64"    "P_65_74"    "P_75_UP"

``` r
blocks$P_VACANT[1:5] # Can be treated like a dataframe... sometimes  
```

    ## [1]  4.980341  5.882353  6.940874  7.459207 13.092551

``` r
# hist(blocks$P_VACANT) #Same for graphing
display.brewer.pal(5, "Blues")
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-55-1.png)

``` r
# auto.shading builds off of color brewer to create classes
# Needs to be stored separately
shades.blue  <-  auto.shading(blocks$P_VACANT, cols = brewer.pal(7,'Blues')[3:7]) #Create a new color palette

shades.blue # Note class breaks
```

    ## $breaks
    ##  20%  40%  60%  80% 
    ##  5.4  7.6 10.0 13.0 
    ## 
    ## $cols
    ## [1] "#9ECAE1" "#6BAED6" "#4292C6" "#2171B5" "#084594"
    ## 
    ## attr(,"class")
    ## [1] "shading"

``` r
# ?auto.shading # Part of GIStools

choropleth(blocks, 
           v = blocks$P_VACANT, # Variable to be mapped
           shading = shades.blue, # Shading object created above
           bg = "gray30", # Background color
           border = NA # No Border
           ) 

plot(blocks,
     add = TRUE,
     col=NA, 
     border = add.alpha("#FFFFFF", .2) # partly transparent white
     )

# choropleth maps attributes held in SpatialPolygons DataFrame (e.g., 'blocks')
choro.legend(px = 533000, py = 161000, 
             sh = shades.blue,
             border = "#FFFFFF80", # Semitransparent white around boxes
             bg = NA, # No background color,
             bty = "n", # No outer box
             text.col = "red", # Broken apparently
             title.col = "white"
             )

title(main = "Percent Vacant",
      col.main = "gray20")
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-55-2.png)

Let's create new set of shades for Owner occupied percentage:

``` r
shades.yell  <-  auto.shading(blocks$P_OWNEROCC,cols=brewer.pal(5,'YlOrRd')) #Create a new color palette

choropleth(blocks, blocks$P_OWNEROCC, 
           shading = shades.yell, # Shading object created above
           bg = "gray30", # Background color
           border = NA # No Border
           ) 
           
plot(blocks,
     add = TRUE,
     col=NA, 
     border = add.alpha("#000000", .15) # partly transparent black
     )

choro.legend(px = 533000, py = 161000, 
             sh = shades.yell,
             border = NA,
             bg = NA, # No background color,
             bty = "n" # No outer box
             )
# Add title
title(main = "Percent Owner Occupied",
      col.main = "gray20")
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-56-1.png)

Now, what would be even more interesting to put these side by side. Note that color choice is important, so these would probably work better with swapped palettes, since red is usually associated with 'negative' variables.

``` r
o.par <- par(no.readonly = FALSE)

# Put them side by side, adjust margins
par(mfrow = c (1,2), mar = c(1,0,1,0)) # mar =bottom, left, top, right

# Vacant Map
choropleth(blocks, v = blocks$P_VACANT, shading = shades.blue, bg = "gray30", border = NA) 

plot(blocks, add = TRUE, col = NA, border = add.alpha("#FFFFFF", .2))

choro.legend(px = 533000, py = 161000, sh = shades.blue, border = "#FFFFFF80", bg = NA, bty = "n", text.col = "red", title.col = "white")

title(main = "Percent Vacant", col.main = "white", line = -1)
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-57-1.png)

Owner Occupied Map:

``` r
choropleth(blocks, blocks$P_OWNEROCC, shading = shades.yell, bg = "gray30", border = NA) 

plot(blocks, add = TRUE, col=NA, border = "gray15" ) # Alpha not working on second plot??

choro.legend(px = 533000, py = 161000, sh = shades.yell, border = NA, bg = NA, bty = "n" )

title(main = "Percent Owner Occupied", col.main = "white", line = -1)
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-58-1.png)

``` r
par(o.par)
```

    ## Warning in par(o.par): graphical parameter "cin" cannot be set

    ## Warning in par(o.par): graphical parameter "cra" cannot be set

    ## Warning in par(o.par): graphical parameter "csi" cannot be set

    ## Warning in par(o.par): graphical parameter "cxy" cannot be set

    ## Warning in par(o.par): graphical parameter "din" cannot be set

    ## Warning in par(o.par): graphical parameter "page" cannot be set

Raster in a Minute
==================

Okay, let's very very briefly delve into the world of rasters. A type of raster can be created using just input points called a kernel density raster, which can be used to visualize relationships, but is prone to manipulation and can become misleading very quickly.

``` r
head(breach) # Note: no underlying data
```

    ## SpatialPoints:
    ##          Long      Lat
    ## [1,] 551419.5 181266.3
    ## [2,] 556319.5 177580.7
    ## [3,] 551423.1 172304.5
    ## [4,] 550261.9 182613.2
    ## [5,] 555168.5 172163.4
    ## [6,] 549133.6 169623.6
    ## Coordinate Reference System (CRS) arguments: +proj=lcc
    ## +datum=NAD27 +lon_0=-72d45 +lat_1=41d52 +lat_2=41d12 +lat_0=40d50
    ## +x_0=182880.3657607315 +y_0=0 +units=us-ft +no_defs +ellps=clrk66
    ## +nadgrids=@conus,@alaska,@ntv2_0.gsb,@ntv1_can.dat

``` r
breach.dens  <-  kde.points(breach, lims = tracts) # Create kernel density values, to then convert to raster

class(breach.dens) # SpatialPixelDataFrame
```

    ## [1] "SpatialPixelsDataFrame"
    ## attr(,"package")
    ## [1] "sp"

``` r
head(data.frame(breach.dens)) # Note the kde values added
```

    ##            kde     Var1   Var2
    ## 1 6.538740e-35 531731.9 147854
    ## 2 9.770069e-35 531922.3 147854
    ## 3 1.448876e-34 532112.7 147854
    ## 4 2.132528e-34 532303.2 147854
    ## 5 3.115218e-34 532493.6 147854
    ## 6 4.516606e-34 532684.0 147854

``` r
breach.dense.grid <- as(breach.dens, "SpatialGridDataFrame") # Use 'as' to coerce into SpatialGridDataFrame (raster)

head(data.frame(breach.dense.grid)) # look at data
```

    ##            kde     Var1     Var2
    ## 1 2.722636e-13 531731.9 188464.6
    ## 2 3.455488e-13 531922.3 188464.6
    ## 3 4.357233e-13 532112.7 188464.6
    ## 4 5.459338e-13 532303.2 188464.6
    ## 5 6.797505e-13 532493.6 188464.6
    ## 6 8.411979e-13 532684.0 188464.6

``` r
image(breach.dense.grid, #Note the image() function for plotting
     col = colorRampPalette(brewer.pal(9, "Reds"))(100)
     )
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-60-1.png) So this is a raster "heatmap" of where breaches of the peace happen, but it doesn't have any context.

It would be nice if we could have a strong outline for the city, with lighter internal divisions, and not show anything outside the city. So, uh, let's do that. First, we need to create a masking polygon, then create an outline polygon, and then add the blocks, but with transparency.

``` r
#repeated for R markdown
image(breach.dense.grid, #Note the image() function for plotting
     col = colorRampPalette(brewer.pal(9, "Reds"))(100)
     )

# Making an outline via the Union function
blocks.outline <- gUnaryUnion(blocks, id = NULL)

# This produces a warning; but they have the same proj4string?
masker <- poly.outer(breach.dense.grid, blocks.outline) # Create mask for raster
```

    ## Warning in RGEOSBinTopoFunc(spgeom1, spgeom2, byid, id, drop_lower_td,
    ## unaryUnion_if_byid_false, : spgeom1 and spgeom2 have different proj4
    ## strings

``` r
plot(masker, 
     border = "white",
     col = "white",
     add = TRUE)

#Plot the block outlines, but mostly transparent
plot(blocks, add = TRUE, 
     border = "#00000046") # Overlay boundaries for context

# Plot a black outline
plot(blocks.outline,
     border = "black", 
     lwd = 2, # Make thicker; line weight 2
     add = TRUE)

title(main = "Breaches of the Peace Heatmap")
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-61-1.png)
---------------------------------------------------------------------------------------

Importing Shapefiles
====================

Here we will import a shapefile with attribute data using *rgdal*, subset it to the area we want using attributes and a clipping polygon (with the *GISTools* package), and make a couple simple maps.

Available spatial data often comes in the nearly-universal shapefile format (.shp), but R by default isn't able to process shapefiles.

As with everything else, you need the right package. There are several options, including *rgdal*, *maptools*, and *PBSmapping* (more information [here](https://www.nceas.ucsb.edu/scicomp/usecases/ReadWriteESRIShapeFiles))

While most of this tutorial relies on *GISTools*, which itself depends on several other spatial packages including *maptools*, *rgdal* is probably the most straightforward way as it automagically gets the projection information if provided in the shapefile.

The shapefile we will be using is from the [US Census](https://www.census.gov/geo/maps-data/data/tiger.html), and contains a significant amount of demographic attributes. Our particular data (Demographic Profile 1 of states) is hosted [here](http://www2.census.gov/geo/tiger/TIGER2010DP1/State_2010Census_DP1.zip).

First, some setup:

``` r
# install.packages("rgdal") # Uncomment and install if needed

# Probably already loaded through GISTools
# library(rgdal) 
suppressPackageStartupMessages(library(rgdal))
```

    ## Warning: package 'rgdal' was built under R version 3.4.3

``` r
rm(list=ls()) # Clear workspace

# setwd() # Set working directory (or Session -> Set Working Directory -> To Source File Location)
```

Now to actually use *readOGR*

``` r
# ?readOGR

# Note that this is one file level down, in State_Census_DP1, and that no file exension is used
murica <- readOGR(dsn = "./State_2010Census_DP1", layer = "State_2010Census_DP1")
```

    ## OGR data source with driver: ESRI Shapefile 
    ## Source: "./State_2010Census_DP1", layer: "State_2010Census_DP1"
    ## with 52 features
    ## It has 195 fields

Of course, once it's in there, it's useful to take a look at it in a couple different ways. First:

``` r
class(murica) # What is this thing?
```

    ## [1] "SpatialPolygonsDataFrame"
    ## attr(,"package")
    ## [1] "sp"

Note that the type of data is *SpatialPolygonsDataFrame*, which means there is geometry (SpatialPolgyons), and attributes (DataFrame). This is important since these can be manipulated separately. For example, you can treat it like a regular data frame.

``` r
colnames(murica) # Note that some basic functions fail due to the data structure
```

    ## NULL

``` r
colnames(murica@data)[1:8] # You need to access the right "slot" with @, in this case "data"
```

    ## [1] "GEOID10"    "STUSPS10"   "NAME10"     "ALAND10"    "AWATER10"  
    ## [6] "INTPTLAT10" "INTPTLON10" "DP0010001"

``` r
slotNames(murica)
```

    ## [1] "data"        "polygons"    "plotOrder"   "bbox"        "proj4string"

``` r
murica$NAME10 # But can be treated like a dataframe for some purposes
```

    ##  [1] Wyoming              Pennsylvania         Ohio                
    ##  [4] New Mexico           Maryland             Rhode Island        
    ##  [7] Oregon               Puerto Rico          Wisconsin           
    ## [10] North Dakota         Nevada               Georgia             
    ## [13] New York             Arkansas             Kansas              
    ## [16] Nebraska             Utah                 Alaska              
    ## [19] Mississippi          Oklahoma             West Virginia       
    ## [22] Michigan             Colorado             New Jersey          
    ## [25] Delaware             Montana              Washington          
    ## [28] Connecticut          California           Kentucky            
    ## [31] Massachusetts        Florida              Idaho               
    ## [34] Missouri             Hawaii               Alabama             
    ## [37] South Carolina       New Hampshire        South Dakota        
    ## [40] Illinois             Tennessee            Indiana             
    ## [43] Iowa                 Arizona              Minnesota           
    ## [46] Louisiana            District of Columbia Virginia            
    ## [49] Texas                Vermont              Maine               
    ## [52] North Carolina      
    ## 52 Levels: Alabama Alaska Arizona Arkansas California ... Wyoming

``` r
# str(murica) # Verbose but gives you detailed information on structure of dataset

murica$NAME10[4] # Name of the fourth, best state
```

    ## [1] New Mexico
    ## 52 Levels: Alabama Alaska Arizona Arkansas California ... Wyoming

``` r
murica@proj4string # This is the projection information, useful for later. Note "@"
```

    ## CRS arguments:
    ##  +proj=longlat +datum=NAD83 +no_defs +ellps=GRS80 +towgs84=0,0,0

``` r
proj4string(murica) # Same as above
```

    ## [1] "+proj=longlat +datum=NAD83 +no_defs +ellps=GRS80 +towgs84=0,0,0"

That's great, but how about plotting it?

``` r
# Wow, look at that extent!
plot(murica) 
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-66-1.png)

Playing with the visuals:

``` r
# Playing with the visuals
plot(murica, col = "SlateBlue", border = NA) 
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-67-1.png)
---------------------------------------------------------------------------------------

Subsetting and Clipping Data
============================

Obviously, this is a little awkward to map (thanks Aleutian islands), but we can go ahead and clip out the continental US for a more convenient mapping set. This is a common workflow, since often you are dealing with an area of interest (AOI) that does not align with all of your data. There are two ways to get the continental US: spatially, and by attribute. First, we will use the attributes to subset the desired states. Second, we will use a spatial clip and then we will subset using attributes in the data.

Subsetting using Attributes
---------------------------

Subsetting by attribute is much easier than a spatial clip, so let's start with that. We can create an array of booleans that we can use to subset the SpatialPolygonsDataFrame based off the name of the states and territories we don't want. These are Alaska, Hawaii, and Puerto Rico.

Note the use of logical operators (tips [here](http://www.statmethods.net/management/operators.html)). Super beginner stuff: Here, **!=** is "is not", and **&** is "and", so this reads more or less *"is this state name not Alaska, not Hawaii, and not Puerto Rico?"*

``` r
head(murica$NAME10) # This field contains the common names of the states
```

    ## [1] Wyoming      Pennsylvania Ohio         New Mexico   Maryland    
    ## [6] Rhode Island
    ## 52 Levels: Alabama Alaska Arizona Arkansas California ... Wyoming

``` r
real.index <- murica$NAME10 != "Alaska" & murica$NAME10 != "Hawaii" & murica$NAME10 != "Puerto Rico" # Create bool list of "real" states

real.index # Note the FALSE instances for those states/territories
```

    ##  [1]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE
    ## [12]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE
    ## [23]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
    ## [34]  TRUE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
    ## [45]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE

``` r
murica.spdf <- murica[real.index,] # Subset using real.index

plot(murica.spdf) # Plot it!
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-68-1.png)

We can also make it look a little better while we're at it:

``` r
# Fancy map of fanciness
plot(murica.spdf, 
     bg= "gray40", 
     col = "gray60", 
     border = "white") 
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-69-1.png)

See? Not that hard. Now for spatial clipping...

Subsetting by Spatial Clip
--------------------------

This method is very much more involved, but necessary for true spatial subsetting, where geometry (i.e., polygons) are required, like clipping national data to a state boundary.

The first step in a spatial clip is getting a clipping polygon, which we will define manually here, but often this another existing area such as a administrative boundary.

Creating a spatial polygon takes a few steps...

``` r
xx <- as.vector(c(-125.85075, -129.45032, -57.45885, -58.35875)) # Coordinates of corners, x
yy <- as.vector(c(23.44079, 52.25642, 51.61607, 22.80044)) # Coordinates of corners, y

crds  <-  cbind(xx, yy) # Combine x and y to make x, y table
crds # Check...
```

    ##              xx       yy
    ## [1,] -125.85075 23.44079
    ## [2,] -129.45032 52.25642
    ## [3,]  -57.45885 51.61607
    ## [4,]  -58.35875 22.80044

``` r
# ?Polygon

Pl  <-  Polygon(crds) # Create a polygon (but not a *spatial* polygon!)

ID <- "clip" # This string will be used later to extract names when we re-merge data

Pls  <- Polygons(list(Pl), ID = ID) # Needs to be a list

#Convert Polygons to SpatialPolygons
# ?SpatialPolygons
clip.raw <- SpatialPolygons(list(Pls), proj4string = CRS(proj4string(murica)) ) # Note use of proj4string()

## Let's take a look

plot(murica) 

plot(clip.raw, 
     add = TRUE, 
     border= "red", 
     lwd = 2) #overlay (add) clipping polygon, red and line weight 2
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-70-1.png) Clipping strips SpatialPolygonDataFrames of their attributes, which means you need a mechanism to restore those attributes. Long story short, but you can maintain the IDs of the features, and use those to re-join the dataframe back to the SpatialPolygon. This is easier if the clipping feature is also a SpatialPolygonDataFrame.

``` r
temp.df <- data.frame(value = 1, row.names = ID) # Create simple DF to add to SpatialPolgyon

clip.spdf <- SpatialPolygonsDataFrame(clip.raw, temp.df) # Merge the SpatialPolygon and temp.df to make SpatialPolygonDataFrame

class(clip.spdf) #Check that it is the correct class (SpatialPolygonDataFrame) and has data included
```

    ## [1] "SpatialPolygonsDataFrame"
    ## attr(,"package")
    ## [1] "sp"

Finally, we are now ready for the actual clip.

``` r
# install.packages("GISTools")
# library(GISTools) # Need GISTools Library
# suppressPackageStartupMessages(library(GISTools))

# ?gIntersection

real.murica.sp  <-  gIntersection(clip.spdf, murica, byid = TRUE) # byid maintains the original IDs

plot(real.murica.sp)# Neat! Too bad Michigan is all messed up.The census has some interesting feelings about lakes.
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-72-1.png)

``` r
class(real.murica.sp) # But no data! (just SpatialPolygon)
```

    ## [1] "SpatialPolygons"
    ## attr(,"package")
    ## [1] "sp"

As you can see, the clip worked but there is no data. Unfortunately, there is no completely straightforward way of doing a clip on a SpatialDataFrame.

What you have to do is subset get an array of the maintained polgyons and use that to subset the original dataframe, then re-join that dataframe to our new SpatialPolygons.

First, we need the array that tells us which polygons we kept. This is saved in the ID of the polygons (since we clipped with **byid =TRUE**), which is a little tricky to access.

``` r
# We can access the ID's individually like this:
real.murica.sp@polygons[[1]]@ID # First polygon, ID field
```

    ## [1] "clip 0"

``` r
#sapply however lets us do this for all polygons at once.
p.names <- sapply(real.murica.sp@polygons, function(x) x@ID) # Extracts names of polygons
p.names # Check results
```

    ##  [1] "clip 0"  "clip 1"  "clip 2"  "clip 3"  "clip 4"  "clip 5"  "clip 6" 
    ##  [8] "clip 8"  "clip 9"  "clip 10" "clip 11" "clip 12" "clip 13" "clip 14"
    ## [15] "clip 15" "clip 16" "clip 18" "clip 19" "clip 20" "clip 21" "clip 22"
    ## [22] "clip 23" "clip 24" "clip 25" "clip 26" "clip 27" "clip 28" "clip 29"
    ## [29] "clip 30" "clip 31" "clip 32" "clip 33" "clip 35" "clip 36" "clip 37"
    ## [36] "clip 38" "clip 39" "clip 40" "clip 41" "clip 42" "clip 43" "clip 44"
    ## [43] "clip 45" "clip 46" "clip 47" "clip 48" "clip 49" "clip 50" "clip 51"

``` r
# Splitting the strings to get ID number

# install.packages("stringr")
library(stringr) # for the str_split_fixed
```

    ## Warning: package 'stringr' was built under R version 3.4.3

``` r
p.names.2 <- str_split_fixed(p.names, " ", n = 2) # Split p.names using a space, return two columns
p.names.2 #Check results
```

    ##       [,1]   [,2]
    ##  [1,] "clip" "0" 
    ##  [2,] "clip" "1" 
    ##  [3,] "clip" "2" 
    ##  [4,] "clip" "3" 
    ##  [5,] "clip" "4" 
    ##  [6,] "clip" "5" 
    ##  [7,] "clip" "6" 
    ##  [8,] "clip" "8" 
    ##  [9,] "clip" "9" 
    ## [10,] "clip" "10"
    ## [11,] "clip" "11"
    ## [12,] "clip" "12"
    ## [13,] "clip" "13"
    ## [14,] "clip" "14"
    ## [15,] "clip" "15"
    ## [16,] "clip" "16"
    ## [17,] "clip" "18"
    ## [18,] "clip" "19"
    ## [19,] "clip" "20"
    ## [20,] "clip" "21"
    ## [21,] "clip" "22"
    ## [22,] "clip" "23"
    ## [23,] "clip" "24"
    ## [24,] "clip" "25"
    ## [25,] "clip" "26"
    ## [26,] "clip" "27"
    ## [27,] "clip" "28"
    ## [28,] "clip" "29"
    ## [29,] "clip" "30"
    ## [30,] "clip" "31"
    ## [31,] "clip" "32"
    ## [32,] "clip" "33"
    ## [33,] "clip" "35"
    ## [34,] "clip" "36"
    ## [35,] "clip" "37"
    ## [36,] "clip" "38"
    ## [37,] "clip" "39"
    ## [38,] "clip" "40"
    ## [39,] "clip" "41"
    ## [40,] "clip" "42"
    ## [41,] "clip" "43"
    ## [42,] "clip" "44"
    ## [43,] "clip" "45"
    ## [44,] "clip" "46"
    ## [45,] "clip" "47"
    ## [46,] "clip" "48"
    ## [47,] "clip" "49"
    ## [48,] "clip" "50"
    ## [49,] "clip" "51"

``` r
p.num <- as.numeric(p.names.2[,2])
p.num # Check results
```

    ##  [1]  0  1  2  3  4  5  6  8  9 10 11 12 13 14 15 16 18 19 20 21 22 23 24
    ## [24] 25 26 27 28 29 30 31 32 33 35 36 37 38 39 40 41 42 43 44 45 46 47 48
    ## [47] 49 50 51

``` r
# In order to use this as an index to subset our data, we need to add 1 since R starts indices at 1, not 0
p.num <- p.num + 1 # Adds 1 to all values, for use as index 
```

Now we can actually subset the data from the original *murica* dataset and combine it with the SpatialPolygon

``` r
murica.data <- data.frame(murica)[p.num,] # Use p.num as the indices to subset murica

head(murica.data[1:8]) # Check results, first 8 columns
```

    ##   GEOID10 STUSPS10       NAME10      ALAND10    AWATER10  INTPTLAT10
    ## 0      56       WY      Wyoming 251470069067  1864445306 +42.9918024
    ## 1      42       PA Pennsylvania 115883064314  3397122731 +40.9042486
    ## 2      39       OH         Ohio 105828706692 10269012119 +40.4149297
    ## 3      35       NM   New Mexico 314160748240   756659673 +34.4391265
    ## 4      24       MD     Maryland  25141638381  6989579585 +38.9466584
    ## 5      44       RI Rhode Island   2677566454  1323668539 +41.5978358
    ##     INTPTLON10 DP0010001
    ## 0 -107.5419255    563626
    ## 1 -077.8280624  12702379
    ## 2 -082.7119975  11536504
    ## 3 -106.1261511   2059179
    ## 4 -076.6744939   5773552
    ## 5 -071.5252895   1052567

``` r
nrow(data.frame(murica)) # 52 rows
```

    ## [1] 52

``` r
nrow(murica.data) # 48 rows, yay!
```

    ## [1] 49

Finally, we can smash that data back into a SpatialPolgygons**DataFrame** using the function... that is, uh, also named that.

``` r
murica.spdf.2  <-  SpatialPolygonsDataFrame(real.murica.sp, data = murica.data, match.ID = FALSE) # Create SpatialPolygonDataFrame

class(murica.spdf.2) # Check results - Note the object type
```

    ## [1] "SpatialPolygonsDataFrame"
    ## attr(,"package")
    ## [1] "sp"

``` r
slotNames(murica.spdf.2) #Check results - Note the Data slot
```

    ## [1] "data"        "polygons"    "plotOrder"   "bbox"        "proj4string"

``` r
plot(murica.spdf.2, col = "gray90") # Plot that thang
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-75-1.png)

Some Maps
---------

Awesome, we now have the real 'Murica ready to go. We can now use the census data to make a couple informative maps. The most obvious is choropleth maps. Choropleth maps should not be used to map *totals*, but instead should be used to map proportions, since the various sizes of features can mislead readers. So, to map population, let's roll with population density. Note that the rather obtuse names for the fields are explained in an .xls file that came in the same .zip as the shapefile.

``` r
head(murica.spdf$ALAND10)
```

    ## [1] 251470069067 115883064314 105828706692 314160748240  25141638381
    ## [6]   2677566454

``` r
murica.spdf$ALANDSQMI <-  (murica.spdf$ALAND10)*3.861e-07 # Conversion from meters to miles, into new field
head(murica.spdf$ALANDSQMI) # Check results
```

    ## [1]  97092.594  44742.451  40860.464 121297.465   9707.187   1033.808

``` r
murica.spdf$ALANDSQMI[murica.spdf$NAME10 == "New Mexico"] # Matches the Google
```

    ## [1] 121297.5

``` r
murica.spdf$DP0010001[murica.spdf$NAME10 == "New Mexico"] # Population
```

    ## [1] 2059179

``` r
murica.spdf$POPDENS <- murica.spdf$DP0010001 / murica.spdf$ALANDSQMI #Create POPDENS field, calculate population density

head(murica.spdf$POPDENS) # Check results - seems reasonable
```

    ## [1]    5.805036  283.899936  282.339038   16.976274  594.770890 1018.145134

Now to get mapping... first, we need to create a color scheme, then we can use the choropleth function in GISTools to quickly make a choropleth map.

``` r
#display.brewer.all()

pop.dens.shades  <-  auto.shading(murica.spdf$POPDENS,  cols = brewer.pal(5,'YlGnBu')) #Create a new color palette, 5 shades

# ?choropleth # A function within GISTools

# Create the map, use POPDENS data, use colors pop.dens.shades, and gray10 border
choropleth(murica.spdf, murica.spdf$POPDENS, pop.dens.shades, border = "gray10")

# Using the title function separately gives you more options 

title("Density of 'Muricans", line = -2, col.main = "gray20", cex.main = 2) 

choro.legend(-125.5, 29.5, pop.dens.shades, bty ="n",  title = "Folks per Square Mile") # Placed manually, use our shades array, and remove legend box
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-77-1.png)

Legends are an art unto themselves, but lots of aesthetic options are available through the various parameters listed [here](https://www.rdocumentation.org/packages/graphics/versions/3.3.2/topics/legend?).

Reprojecting
============

You may have noticed that the top of the continental US is straight, which depending on how picky you are looks awful. This is a result of the map projection used, and is something that can be changed. This is the sort of thing that is easy to code, but actually requires a lot of domain knowledge. Map projections are complicated beasts, and it's often best to use something established, such as what your source data uses. You can also Google the best projections for your particular area of interest.

Once you know the name or class of projection you need, then what? Like everything else, you need the right format.

Typically projection information in R is contained with proj4strings (PROJ.4 strings), which are seemingly structured to confuse and deceive, but often something like an EPSG code is referenced elsewhere. Fortunately, these can be translated. Let's say you have a totally sick heads up on a dope map projection for the continental US called US National Atlas Equal Area, and you have the EPSG code, which is 2163.

``` r
# These functions are from rgdal, which is loaded with GISTools

EPSG <- make_EPSG() # Create list of ESPG codes
head(EPSG) # Quite the list!
```

    ##   code                                               note
    ## 1 3819                                           # HD1909
    ## 2 3821                                            # TWD67
    ## 3 3824                                            # TWD97
    ## 4 3889                                             # IGRS
    ## 5 3906                                         # MGI 1901
    ## 6 4001 # Unknown datum based upon the Airy 1830 ellipsoid
    ##                                                                                            prj4
    ## 1 +proj=longlat +ellps=bessel +towgs84=595.48,121.69,515.35,4.115,-2.9383,0.853,-3.408 +no_defs
    ## 2                                                         +proj=longlat +ellps=aust_SA +no_defs
    ## 3                                    +proj=longlat +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +no_defs
    ## 4                                    +proj=longlat +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +no_defs
    ## 5                            +proj=longlat +ellps=bessel +towgs84=682,-203,480,0,0,0,0 +no_defs
    ## 6                                                            +proj=longlat +ellps=airy +no_defs

``` r
lambert.ea <- subset(EPSG, code==2163)$prj4 # Subset using EPSG code

lambert.ea
```

    ## [1] "+proj=laea +lat_0=45 +lon_0=-100 +x_0=0 +y_0=0 +a=6370997 +b=6370997 +units=m +no_defs"

Of course, you can naturally look this up all online, which is what (spatialreference.org)\[<http://spatialreference.org/>\] is for. You can search for projections, and pull the PROJ.4 string directly. Here is the page for the example above: (US National Atlas Equal Area)\[<http://spatialreference.org/ref/epsg/2163/>\].

``` r
murica.lambert <- spTransform(murica.spdf, CRS(lambert.ea)) # CRS interfaces with PROJ.4 and parses the projection for spTransform
# summary(murica.lambert)

plot(murica.lambert) # Oooh pretty curve!
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-79-1.png)

Okay, now to just re-do our previous map with our reprojected data.

``` r
o.par <- par(no.readonly = TRUE)

par(mar = c(1, 1, 1, 1) )

choropleth(murica.lambert, 
           murica.lambert$POPDENS, 
           pop.dens.shades, 
           border = NA) # Create the map, use POPDENS data, use colors pop.dens.shades, and gray10 border

plot(murica.lambert, 
     add = TRUE, 
     border = "#0000003C")

title("Density of 'Muricans", 
      line = -1.5, 
      col.main = "gray20", 
      cex.main = 2) # Create title, move it down two lines, make it gray, make it bigger (cex)

#Note that since the coordinate system changed, we need to change our legend coordinates:
choro.legend(-2250000, -1643980, 
             border = "gray10",
             pop.dens.shades, 
             bty ="n",  
             title = "Folks per Square Mile")
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-80-1.png)

``` r
par(o.par)
```

Extra!
======

There are some very quick ways to generate maps using existing web services and the *ggmap* package, adapted from R-bloggers (here)\[<https://www.r-bloggers.com/google-maps-and-ggmap/>\]. (Here is a useful cheatsheet for ggmap)\[<https://www.nceas.ucsb.edu/~frazier/RSpatialGuides/ggmap/ggmapCheatsheet.pdf>\].

Unfortunately, depending on various projections, it can be somewhat difficult to map your data onto these base layers (easiest for points).

``` r
#install.packages("ggmap")
#install.packages("mapproj")
library(ggmap)
```

    ## Warning: package 'ggmap' was built under R version 3.4.3

``` r
USA <- get_map(location = "United States", zoom = 3) # Create a map and store it
```

    ## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=United+States&zoom=3&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=United%20States&sensor=false

``` r
ggmap(USA) # Plot it! (begin eagle tears)
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-81-1.png)

``` r
#plot(USA) # Also works but not as friendly for advanced things
```

``` r
PA <- get_map(location = "Pennsylvania", zoom = 7) # Hey I know that place!
```

    ## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=Pennsylvania&zoom=7&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=Pennsylvania&sensor=false

``` r
ggmap(PA) # Plot it
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-82-1.png)

``` r
PA.2 <- get_map(location = "Pennsylvania", 
                source = "stamen", # Change the source
                maptype =  "watercolor", # Each source has different map types
                zoom = 7 )
```

    ## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=Pennsylvania&zoom=7&size=640x640&scale=2&maptype=terrain&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=Pennsylvania&sensor=false

    ## Map from URL : http://tile.stamen.com/watercolor/7/35/46.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/36/46.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/37/46.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/35/47.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/36/47.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/37/47.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/35/48.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/36/48.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/37/48.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/35/49.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/36/49.jpg

    ## Map from URL : http://tile.stamen.com/watercolor/7/37/49.jpg

``` r
ggmap(PA.2) # Plot it
```

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-83-1.png)

``` r
# You can also get imagery
cool.kids.place <- get_map(location = c(lat = 40.793589, lon = -77.867026), # Where, I wonder?
                         color = "color", # Can change to bw for panchromatic
                         source = "google", # 
                         maptype = "satellite", # Type
                         zoom = 17)
```

    ## note : locations should be specified in the lon/lat format, not lat/lon.

    ## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=40.793589,-77.867026&zoom=17&size=640x640&scale=2&maptype=satellite&language=en-EN&sensor=false

``` r
ggmap(cool.kids.place,extent = "device" )# Plot to extent of frame ("device")
```

    ## Warning: `panel.margin` is deprecated. Please use `panel.spacing` property
    ## instead

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-84-1.png)

``` r
cool.kids.place.roads <- get_map(location = c(lat = 40.793589, lon = -77.867026),
                         color = "color",
                         source = "google",
                         maptype = "roadmap", # This should be pretty familiar
                         zoom = 16)
```

    ## note : locations should be specified in the lon/lat format, not lat/lon.

    ## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=40.793589,-77.867026&zoom=16&size=640x640&scale=2&maptype=roadmap&language=en-EN&sensor=false

``` r
ggmap(cool.kids.place.roads, extent = "device") # Plot to extent of frame ("device")
```

    ## Warning: `panel.margin` is deprecated. Please use `panel.spacing` property
    ## instead

![](GDS2017_Visualization_Tutorial_files/figure-markdown_github/unnamed-chunk-85-1.png)
