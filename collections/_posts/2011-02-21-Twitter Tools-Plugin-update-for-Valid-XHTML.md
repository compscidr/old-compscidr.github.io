---
layout: default
title: "Twitter Tools - Plugin update for Valid XHTML"
published: true
featured-image: ""
featured-alt: ""
categories: [Misc, Tutorials]
tags: [	Fix, Plugin, Tools, Twitter, Wordpress]
---

Today I noticed my site was not generating valid XHTML (for the 1.0 Strict Standard). I isolated some of the problems to the ["Twitter Tools" plugin for Wordpress](https://crowdfavorite.com/wordpress/). The problem is that a couple of parameters are not passed using the correct encoding (The & character is not properly encoded). The changes should be made to the "```twitter-tools.php```" file. Here is the original source for a couple of the offending lines:

```
<link rel="stylesheet" type="text/css" href="'.site_url('/index.php?ak_action=aktt_css&v='.AKTT_VERSION).'" />	 	
<link rel="stylesheet" type="text/css" href="'.site_url('/index.php?ak_action=aktt_css&v='.AKTT_VERSION).'" />
<script type="text/javascript" src="'.site_url('/index.php?ak_action=aktt_js&v='.AKTT_VERSION).'"></script>	 	
<script type="text/javascript" src="'.site_url('/index.php?ak_action=aktt_js&v='.AKTT_VERSION).'"></script>
<link rel="stylesheet" type="text/css" href="'.admin_url('index.php?ak_action=aktt_css_admin&v='.AKTT_VERSION).'" />
<link rel="stylesheet" type="text/css" href="'.admin_url('index.php?ak_action=aktt_css_admin&v='.AKTT_VERSION).'" />
<script type="text/javascript" src="'.admin_url('index.php?ak_action=aktt_js_admin&v='.AKTT_VERSION).'"></script>
```

And here it is with the changes made (notice the '```&```' is replaced with '```&amp;```'):

```
<link rel="stylesheet" type="text/css" href="'.site_url('/index.php?ak_action=aktt_css&amp;v='.AKTT_VERSION).'" />	 	
<link rel="stylesheet" type="text/css" href="'.site_url('/index.php?ak_action=aktt_css&amp;v='.AKTT_VERSION).'" />
<script type="text/javascript" src="'.site_url('/index.php?ak_action=aktt_js&amp;v='.AKTT_VERSION).'"></script>	 	
<script type="text/javascript" src="'.site_url('/index.php?ak_action=aktt_js&amp;v='.AKTT_VERSION).'"></script>
<link rel="stylesheet" type="text/css" href="'.admin_url('index.php?ak_action=aktt_css_admin&amp;v='.AKTT_VERSION).'" />	 
<link rel="stylesheet" type="text/css" href="'.admin_url('index.php?ak_action=aktt_css_admin&amp;v='.AKTT_VERSION).'" />
<script type="text/javascript" src="'.admin_url('index.php?ak_action=aktt_js_admin&amp;v='.AKTT_VERSION).'"></script>
```
