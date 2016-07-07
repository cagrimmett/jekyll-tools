---
layout: page
title: Posts Heatmap Calendar
permalink: /posts-cal/
---

<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.3/jquery.min.js"></script>
<script src="https://d3js.org/d3.v3.min.js" charset="utf-8"></script>
<script type="text/javascript" src="//cdn.jsdelivr.net/cal-heatmap/3.3.10/cal-heatmap.min.js"></script>
<link href="//maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css" rel="stylesheet">
<link rel="stylesheet" href="//cdn.jsdelivr.net/cal-heatmap/3.3.10/cal-heatmap.css" />
<script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/moment.js/2.14.1/moment.min.js"></script>

<style type="text/css">
.content {
	min-width: 400px;
}
#calendar {
	width: 839px;
}
.subdomain-text {
	fill: #fff;
}
#calendar a {
	color: #999;
}
@media all and (max-width:1400px) {
	#calendar {
		width: 626px;
	}
}
@media all and (max-width:730px) {
	#calendar {
		width:365px;
	}
}
@media all and (max-width:420px) {
	#calendar {
		width:191px;
	}
}
</style>

This heatmap calendar gives you a visual representation of when you posted on your Jekyll site. It loops through all of your posts, counts how many posts you have each day, creates a JSON string to hold them, then uses [moment.js](http://momentjs.com), [D3.js](http://d3js.org) and [Cal-HeatMap](http://cal-heatmap.com) to visualize them. 

It automatically loads the current month on the right and it has responsive breakpoints at 1400px, 730px, and 420px. It will work on Github Pages because it doesn't need any additional plugins to run. It only uses Liquid to do the counting and build the JSON string.

For more info, visit [https://github.com/cagrimmett/jekyll-tools](https://github.com/cagrimmett/jekyll-tools)

<div id="calendar" style="margin:0 auto;">
	<div id="cal-heatmap"></div>
	<div style="padding-top: 10px;">
		<a href="#" style="margin-right:10px;" id="cal-heatmap-PreviousDomain-selector"><i class="fa fa-chevron-left"></i></a>
		<a href="#" style="float:right;" id="cal-heatmap-NextDomain-selector"><i class="fa fa-chevron-right"></i></a>
	</div>
</div>


<script type="text/javascript">

var data = {% assign counter = 0 %}{
{% for post in site.posts %}{% capture day %}{{ post.date | date: '%s' }}{% endcapture %}{% capture prevday %}{{ post.previous.date | date: '%s' }}{% endcapture %}{% assign counter = counter | plus: 1 %}{% if day != prevday %}"{{ post.date | date: '%s' }}": {{ counter }}{% assign counter = 0 %}{% if forloop.last == false %},{% endif %}
{% endif %}{% endfor %}};


var responsiveCal = function( options ) {
	var now = new Date();
    if( $(window).width() < 420 ) {
        options.start = now.setMonth(now.getMonth());
        options.range = 1;
        options.cellSize = 25;
    } else if ( $(window).width() < 730 ) {
        options.start = now.setMonth(now.getMonth() - 1);
        options.range = 2;
        options.cellSize = 20;
    } else if( $(window).width() < 1400 ) {
        options.start = now.setMonth(now.getMonth() - 2);
        options.range = 3;
        options.cellSize = 23;
    } else {
        options.start = now.setMonth(now.getMonth() - 3);
        options.range = 4;
        options.cellSize = 23;
    }

    if( typeof cal === "object" ) {
        $('#cal-heatmap').html('');
        cal = cal.destroy();
    }
    cal = new CalHeatMap();
    cal.init( options );

}
caloptions = {
    itemSelector: "#cal-heatmap",
	domain: "month",
	subDomain: "x_day",
	data: data,
	dataType: "json",
	cellPadding: 5,
	domainGutter: 20,
	displayLegend: false,
	range: 4,
	considerMissingDataAsZero:false,
	domainDynamicDimension: true,
	previousSelector: "#cal-heatmap-PreviousDomain-selector",
	nextSelector: "#cal-heatmap-NextDomain-selector",
	domainLabelFormat: function(date) {
		moment.locale("en");
		return moment(date).format("MMMM").toUpperCase();
	},
	subDomainTextFormat: "%d",
	legend: [0,1,2,3],
	label: {
		position: "top"
	}
};


// run first time, put in load if your scripts are in footer
responsiveCal( caloptions );

$(window).resize(function() {
    if(this.resizeTO) clearTimeout(this.resizeTO);
    this.resizeTO = setTimeout(function() {
        $(this).trigger('resizeEnd');
    }, 500);
});

//resize on resizeEnd function
$(window).bind('resizeEnd', function() {
	 responsiveCal( cal.options );
});
  
	
</script>
