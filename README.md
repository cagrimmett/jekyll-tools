# Jekyll Tools
A collection of templates I made for my [Jekyll](http://jekyllrb.com)-powered [blog](http://cagrimmett.com). I thought that other people might want to use them, so I tossed them up here.

The collection so far:

- [A template for adding social metadata (Open Graph and Twitter Cards) to posts and pages](#social-metadata)
- [A template for adding Disqus comments](#comments-via-disqus)
- [A template for listing posts by tag](#posts-by-tag)
- [A posts heatmap calendar](#posts-heatmap-calendar)

---

## Social Metadata
The `social_metadata.html` file is a template for easily adding [Open Graph](http://ogp.me) and [Twitter Cards](https://dev.twitter.com/cards/markup) metadata for your Jekyll posts. The Liquid markup relies on some settings from your `_config.yml` file and some settings from the YAML front-matter on each post.

### Implementation
- Place the `social_metadata.html` file in the `_includes` folder [inside your Jekyll site](https://jekyllrb.com/docs/structure/).
- Include the file inside the `<head> </head>` tags on your post pages. Most Jekyll themes make the headers a separate file that is itself included in other templates. It is usually called something like `header.html` or `head.html` and usually also lives inside the `_includes` folder.
- You can include it with Liquid like this:
~~~~liquid
{% include social_metadata.html %}
~~~~

Make sure the following items are defined in your `_config.yml` file ([learn more here](https://jekyllrb.com/docs/configuration/)). Replace my examples with your own info:
~~~~yaml
title: Chuck Grimmett's GitHub
url: "" # the subpath of your site, e.g. /blog/
baseurl: "http://example.com" # the base hostname & protocol for your site
locale: en_US # language_territory is the default
theme:
	description: "This is my awesome Jekyll site"
	avatar: "/link/to/img.jpg"
	twitter: "username" # Don't include @
~~~~

Make sure the following items are defined in your posts' [front matter](https://jekyllrb.com/docs/frontmatter/):
~~~~yaml
---
title: 
author: 
date: YYYY-MM-DD
feature-img: "/link/to/img.jpg"
excerpt: This post is about blah blah blah...
---
~~~~

Don't just copy and paste the above blocks. Some of these are probably already in your `_config.yml` file or your posts' front matter. Add what is missing to the correct section. Be mindful of indentation.

### Validation
- [Validate your Open Graph metadata with Facebook's Debugger](https://developers.facebook.com/tools/debug)
- [Validate your Twitter Cards with Twitter's Card Validator](https://cards-dev.twitter.com/validator)

---

## Comments via Disqus
The `disqus.html` is a way to add Disqus commenting to your Jekyll posts. Disqus also [provides instructions for an alternative way](https://help.disqus.com/customer/portal/articles/472138-jekyll-installation-instructions).

### Implementation
- Put the `disqus.html` file in the `_includes` folder [inside your Jekyll site](https://jekyllrb.com/docs/structure/).
- Register your site on [Disqus](http://disqus.com/register)

Under `theme:` in your `_config.yml` file ([learn more here](https://jekyllrb.com/docs/configuration/)), define your [Disqus shortname](https://help.disqus.com/customer/portal/articles/466208-what-s-a-shortname-):
~~~~yaml
theme:
	disqus_shortname: "exampleshortname"
~~~~

Put this snippet in the post where you want your comments to show up. I put mine at the bottom of my posts template, just before the footer.
~~~~liquid
<!-- Disqus -->
{% if site.theme.disqus_shortname %}
<div class="comments">
  {% include disqus.html %}
</div>
{% endif %}
~~~~

---

## Posts by Tag
`posts_by_tag.md` is a page that displays the site's tags in alphabetical order and shows how many posts there are per tag, makes anchor links for each tag, then outputs posts by tag in reverse chronological order. You can see it in practice at the [bottom of my Today I Learned page](http://cagrimmett.com/til). It will end up looking like this:

![What Posts by Tag should like like after implemented](posts_by_tag.png)

### Implementation
Put the `posts_by_tag.md` file at the root of [your Jekyll site](https://jekyllrb.com/docs/structure/). After your generate your site via `$ jekyll build`, it will be available at http://yoursitename.com/tags/

Add tags to your posts by including them in your posts' [front matter](https://jekyllrb.com/docs/frontmatter/):
~~~~yaml
---
tags:
- Foo
- Bar
---
~~~~

### CSS
Here are the styles for the classes that are set in the `posts_by_tag.md` file for the layout of the tags. Include them in your site's CSS file:
~~~~css
ul.tag-box li {
	display: inline-block;
	list-style: none;
	list-style-image: none;
	margin-bottom: 7px;
}
ul.tag-box li a {
	background: #e6e6e6;
	padding: 4px 8px;
	border-radius: 3px;
	color: #F76B48;
}
ul.tag-box li span.size {
	font-weight: 300;
}
~~~~

---

## Posts Heatmap Calendar

![Posts Heatmap Calendar](posts_heatmap_calendar.png)

This heatmap calendar gives you a visual representation of when you posted on your Jekyll site. It loops through all of your posts, counts how many posts you have each day, creates a JSON string to hold them, then uses [moment.js](http://momentjs.com), [D3.js](http://d3js.org) and [Cal-HeatMap](http://cal-heatmap.com) to visualize them. 

It automatically loads the current month on the right and it has responsive breakpoints at 1400px, 730px, and 420px. It will work on Github Pages because it doesn't need any additional plugins to run. It only uses Liquid to do the counting and build the JSON string.

See it in action at [http://cagrimmett.com/2016/07/06/posts-heatmap-calendar.html](http://cagrimmett.com/2016/07/06/posts-heatmap-calendar.html)

### Implementation
Put the `posts_heatmap_calendar.md` file at the root of [your Jekyll site](https://jekyllrb.com/docs/structure/). After your generate your site via `$ jekyll build`, it will be available at http://yoursitename.com/posts-cal/

Alternatively, here is how you can include it on any Jekyll-generated page: 

1) Put these includes in the file's header:
~~~~html
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
~~~~

Note: I'm only using FontAwesome for the left and right arrows under the calendar. I included it because I use it elsewhere on my site, so it is always available in the header. If you don't want to use it, feel free to replace the arrows with `&larr;` and `&rarr;`.

2) Include this Javascript in the footer to build the JSON, generate the calendar, and drive the responsiveness: 
~~~~javascript
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
~~~~

3) Put this HTML on the page where you want the calendar to show up:
~~~~html
<div id="calendar" style="margin:0 auto;">
	<div id="cal-heatmap"></div>
	<div style="padding-top: 10px;">
		<a href="#" style="margin-right:10px;" id="cal-heatmap-PreviousDomain-selector"><i class="fa fa-chevron-left"></i></a>
		<a href="#" style="float:right;" id="cal-heatmap-NextDomain-selector"><i class="fa fa-chevron-right"></i></a>
	</div>
</div>
~~~~

### Customization

- Refer to the Cal-HeatMap documentation for how to [change the colors](http://cal-heatmap.com/#legendColors), [add a legend](http://cal-heatmap.com/#legend), and [change the cell size](http://cal-heatmap.com/#cellSize).
- To change the responsive breakpoints, change the various widths at `$(window).width() < X` in the `responsiveCal` function.
- Only want to use this for a specific category [like I do](http://cagrimmett.com/til)? Change `{% for post in site.posts %}` in the `data` variable to `{% for post in site.categories['CATEGORYNAME'] %}` (change CATEGORYNAME to the name of the category you want to use this for)