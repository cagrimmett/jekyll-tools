# Jekyll Tools
A collection of templates I made for my [Jekyll](http://jekyllrb.com)-powered [blog](http://cagrimmett.com). I thought that other people might want to use them, so I tossed them up here.

The collection so far:

- [A template for adding social metadata (Open Graph and Twitter Cards) to posts and pages](#social-metadata)
- [A template for adding Disqus comments](#comments-via-disqus)
- [A template for listing posts by tag](#posts-by-tag)

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