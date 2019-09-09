---
layout: default
title: "Sharethis Wordpress Plugin - Invalid XHTML"
published: true
featured-image: ""
featured-alt: ""
categories: [Misc, Tutorials]
tags: [Invalid XHTML, Plugin, Sharethis, Wordpress]
---

Building on my previous post regarding invalid XHTML (Version 1.0 Strict), I have been trying to find out why my site has not been validating. It turns out, the last bit of the problem is the "Sharethis" plugin for Wordpress. In this plugin, the authors decided to define some custom parameter fields within the 'span' tag causing the validation problems. A post here indicates it is for "simplicity" for their less technically inclined users. So, this post is to show you how to modify the plugin to get valid XHTML out of it instead.

In the sharethis.php file, the offending lines are here:
```
$tags="<span class='st_sharethis' st_title='".strip_tags(get_the_title())."' st_url='".get_permalink($post->ID)."' displayText='ShareThis'></span>";	 	
$tags="<span class='st_sharethis' st_title='".strip_tags(get_the_title())."' st_url='".get_permalink($post->ID)."' displayText='ShareThis'></span>";
$tags.="<span class='st_".$svc."_vcount' st_title='{title}' st_url='{url}' displayText='share'></span>";
```

And here are the updated lines:
```
$tags="<span class='st_sharethis'></span>";
$tags="<span class='st_sharethis'></span>";
$tags.="<span class='st_".$svc."_vcount'></span>";
```

In this case, disabling the extra fields means that the "sharethis" text and some minor styling disappear from the sharethis button. It seems the easiest way to have compliant XHTML and still a decent looking sharethis button is to use the a custom button style (see this [page](http://help.sharethis.com/customization/custom-buttons)).

So the result is this:

```
$tags="<span class="st_sharethis_custom"><a>ShareThis</a></span>";<br />
$tags.="<span class="st_sharethis_custom"><a>ShareThis</a></span>";
```

The 'ShareThis' text is enclosed in an "a" tag so that we get the mouseover behaviour and link styling that it had previously. Additionally, the css file for your template must be changed. This is what I added to mine to make it work:

```
.st_sharethis_custom { background: url("img/sharethis.png") no-repeat scroll left top transparent; padding-left: 18px; padding-top:3px;}
```

You will want to change the url to wherever you have located the sharethis icon, and the padding and margins may need to be adjusted for your own template.

{% include featured-image.html %}
