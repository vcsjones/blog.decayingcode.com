---
title: "Easy Charting in JavaScript with d3js and Dimple from CSV data"
date: 2014-08-06 21:44:06
tags: [web,javascript,chart]
---

[![chart-line-148256_640](http://blog.decayingcode.com/posts/files/b8f28463-d5c3-42b1-bf3e-fb4dbf6fbe68.png "chart-line-148256_640")](http://blog.decayingcode.com/posts/files/3e4669c3-d71a-4f2e-bee8-47a5e71cbaff.png)
 > Before I go further, let me give you a link to the source for this blog post [available on Github](https://github.com/MaximRouiller/charting-blog-posts) 

### 

When we talk about doing charts, post people will think about Excel. 

Excel do provide some very rich charting but the problem is that you need a licence for Excel. Second, you need to share a file that often have over 30Mb of data to display a simple chart about your monthly sales or what not.

While it is a good way to explore your data, once you know what you want… you want to be able to share it easily. Then you use the first tool available to a Microsoft developer… SSRS.

But what if… you don’t need the huge machine that is SSRS but just want to display a simple graph in a web dashboard? It’s where simple charting with Javascript comes in.

So let’s start with d3js.

### What is d3.js? 

[d3js](http://d3js.org) is a JavaScript library for manipulation documents based on data. It will help you create HTML, CSS and SVG that will allow you&nbsp; to better display your data.

However… it’s extremely low level. You will have to create your axis, your popup, your hover, your maps and what not.

But since it’s only a building block, other libraries exist that leverage d3js… 

### Dimple

[Dimple](http://dimplejs.org) is a super simple charting library built on top of d3js. It’s what we’re going to use for this demo. But we need data…

Let’s start with a simple data set.

### Sample problem: Medal per country for the 2010 Winter Olympics

Original data can be found here: [http://www.statcrunch.com/app/index.php?dataid=418469](http://www.statcrunch.com/app/index.php?dataid=418469 "http://www.statcrunch.com/app/index.php?dataid=418469")

I’m going to just copy this into Excel (Google Spreadsheets) to clean the data a bit. We’ll remove all the “Country of ”, which will only pollute our data, as well as the Bins which could be dynamic but are otherwise useless.

First step will be to start a simple MVC project so that we can leverage basic MVC minimizing, layouts and what not.

In our _Layout.cshtml, we’ll add the following thing to the “head”:

```xml
<script src="http://d3js.org/d3.v3.min.js"></script>
<script src="http://dimplejs.org/dist/dimple.v2.1.0.min.js"></script>
```

This will allow us to start charting almost right away!

### Step one: Retrieving the csv data and parsing it

Here’s some code that will take a CSV that is on disk or generated by an API and parse it as an object.

```js
$.ajax("/2010-winter-olympics.csv", {
    success: function(data) {
        var csv = d3.csv.parse(data);
        console.table(csv);
    }
});    
```

This code is super simple and will display something along those lines:

[![image](http://blog.decayingcode.com/posts/files/7449430a-c54f-47fa-9795-4b1ce762f986.png "image")](http://blog.decayingcode.com/posts/files/310f01d2-d4fe-4bca-8b2e-9474e2b9668a.png)

Wow. So we are almost ready to go?

### Step two: Using Dimple to create our data.

As mentioned before, Dimple is a super simple tool to create chart. Let’s see how far we can go with the least amount of code.

Let’s add the following to our “success” handler:

```js
var chart = new dimple.chart(svg, csv);
chart.addCategoryAxis("x", "Country");
chart.addMeasureAxis("y", "Total");
chart.addSeries(null, dimple.plot.bar);
chart.draw();
```

Once we refresh the page, it creates this:

[![image](http://blog.decayingcode.com/posts/files/a826cff1-0300-4786-9ebd-aa2aeba3084b.png "image")](http://blog.decayingcode.com/posts/files/61642094-d59d-43a2-91f5-566ee3ff5fae.png)

Okay… not super pretty, lots of crappy data but… wow. We already have a minimum viable data source. To help us see it better… let’s clean the CSV file. We’ll remove all countries that didn’t win medals.

For our data set, that means from row 28 (Albania).

Let’s refresh.

[![image](http://blog.decayingcode.com/posts/files/f5d92215-53df-4f5e-90a8-589ffba5274f.png "image")](http://blog.decayingcode.com/posts/files/aa164a01-2661-4243-99f2-f8d16980f809.png)

And that’s it. We now have a super basic bar graph. 

### 

### Conclusion

It is now super easy to create graphs in JavaScript. If you feel the need to create graphs for your users, you should consider using d3.js with charting library that are readily available like Dimple.

Do not use d3.js as a standalone way of creating graphs. You will find it harder than it needs to be.

If you want to know more about charting, please let me know on Twitter: [@MaximRouiller](https://twitter.com/MaximRouiller)