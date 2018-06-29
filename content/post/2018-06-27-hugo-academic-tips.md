---
title: 'Tips for using the Hugo academic theme'
author: Leslie Myint
date: '2018-06-27'
slug: hugo-academic-tips
categories: ["R"]
tags: ["blogdown"]
header:
  caption: ''
  image: ''
---

I recently migrated my personal website and [Wordpress blog](https://lesliemyint.wordpress.com/) to [blogdown](https://bookdown.org/yihui/blogdown/). As an academic, it was natural to use the [Academic](https://github.com/gcushen/hugo-academic) theme. The blogdown package made the conversion fairly straighforward, but I still had to spend some time figuring out how to work with this Hugo theme.

The source and rendered files for my website are available on GitHub:

- [Public, rendered site](https://github.com/lmyint/lmyint.github.io): the `public` directory within my blogdown/Hugo project
- [Hugo content and source files](https://github.com/lmyint/personal_site): all files and directories within my blogdown/Hugo project (i.e. TOML files, `archetypes/`, `content/`, `data/`, `layouts/`, `static/`, `themes/`)

## Table of Contents

1. [Resources](#resources)
2. [Start with `config.toml`](#start-with-config-toml)
3. [Full content RSS feeds for categories](#full-content-rss)
4. [Modifying the Contact section](#modifying-contact-section)
5. [Adding a CV](#adding-cv)
6. [Version control](#version-control)

## Resources <a id="resources"></a>

This post contains a minimal set of notes that I used to configure specfic parts of the Academic theme and is not a full tutorial on starting a blogdown website. The references and tutorials below are helpful for the initial setup of your site.

- [blogdown book](https://bookdown.org/yihui/blogdown/) by Yihui Xie, Amber Thomas, and Alison Presmanes Hill
- [Up and running with blogdown](https://alison.rbind.io/post/up-and-running-with-blogdown/) by Alison Presmanes Hill
- [Making a Website Using Blogdown, Hugo, and GitHub pages](https://amber.rbind.io/blog/2016/12/19/creatingsite/) by Amber Thomas

## Start with `config.toml` <a id="start-with-config-toml"></a>

The key-value pairs in `config.toml` are pretty straightforward, and I was able to very quickly fill in basic information to populate the home page. The places I'll mention next are ones where I had to spend a little more time.

### Color theme

```toml
[params]
  # Color theme.
  #   Choose from `default`, `ocean`, `forest`, `coffee`, `dark`, or `1950s`.
  color_theme = "custom"
```

This sets the color scheme for your site. I changed the theme to "custom" and made created a file called `custom.toml` in `themes/hugo-academic/data/themes/`. I have the following in my `custom.toml` file:

```toml
# Theme metadata
name = "custom"

# Is theme light or dark?
light = true

# Primary
primary = "#328cc1"
primary_light = "#328cc1"
primary_dark = "#DA2536"

# Menu
menu_primary = "#494949"
menu_text = "#fff"
menu_text_active = "#328cc1"
menu_title = "#fff"

# Backgrounds
background = "#fff"
home_section_odd = "#fff"
home_section_even = "#f7f7f7"
```

The "Primary" section changes the color of links and icons depending on whether you want a dark or light-colored theme. The "Menu" section changes the colors in the top menu bar. The "Backgrounds" section changes the color of the section panels on the first page.

### highlight.js

In this section, you can configure the languages for which you want to support syntax highlighting. As mentioned in the comments in this section of `config.toml`, you can visit https://cdnjs.com/libraries/highlight.js/ to see the list of languages supported (URls ending in `languages/LANGUAGE_NAME.min.js`). You'll also see a list of color schemes (URLs ending in `styles/STYLE_NAME.min.css`). I wanted to know what these color schemes looked like, so I searched and found https://highlightjs.org/static/demo/.

## Full content RSS feeds for categories <a id="full-content-rss"></a>

When I first started building my site with the Academic theme, I noticed that most of my RSS feeds (e.g. https://lmyint.github.io/post/index.xml, https://lmyint.github.io/categories/r/index.xml) contained only a brief summary of my posts in the `description` tags as opposed to the full post content. Only my home page RSS feed (https://lmyint.github.io/index.xml) had full content of posts.

Following the advice [here](https://github.com/gcushen/hugo-academic/issues/346) by changing the outputs in `config.toml` to the TOML below did not fix the issue.

```toml
[outputs]
  home = [ "HTML", "CSS", "RSS" ]
  section = [ "HTML", "RSS" ]
  taxonomy = [ "HTML", "RSS" ]
  taxonomyTerm = [ "HTML", "RSS" ]
```

**The fix:** If you would like to contribute certain posts to a content aggregator that requires full post content on the RSS feed (such as [R-Bloggers](https://www.r-bloggers.com/)), do the following:

1. Put these posts in one **category** (not **tag**)
2. Go to https://gohugo.io/templates/rss/#the-embedded-rss-xml
3. Look in the third row of the table: Taxonomy list in categories
4. Create `layouts/categories/category.rss.xml` and use the default RSS template at the bottom of the page replacing

```html
<description>{{ .Summary | html }}</description>
```

with

```html
<description>{{ .Content | html }}</description>
```

After this, the RSS feeds for your category pages should have full post content.

## Modifying the Contact section <a id="modifying-contact-section"></a>

By default, the Contact section of the page will display certain items in the `params` table of your `config.toml` file. With the TOML below, the Contact section would only contain my e-mail address.

```toml
[params]
  # Some other stuff...

  email = "lmyint@macalester.edu"
  address = ""
  office_hours = ""
  phone = ""
  skype = ""
  telegram = ""
```

I wanted to modify the Contact section to also show my Twitter handle, so I changed the TOML to the following.

```toml
[params]
  # Some other stuff...

  email = "lmyint@macalester.edu"
  address = ""
  office_hours = ""
  phone = ""
  skype = ""
  telegram = ""
  twitter = "lesliemyint"
```

I also had to update `themes/hugo-academic/layouts/partials/widgets/contact.html`. I duplicated the section of the HTML that displays the e-mail address parameter:

```html
{{ with $.Site.Params.email }}
<li>
  <i class="fa-li fa fa-envelope fa-2x" aria-hidden="true"></i>
  <span id="person-email" itemprop="email">
  {{- if $autolink }}<a href="mailto:{{ . }}">{{ . }}</a>{{ else }}{{ . }}{{ end -}}
  </span>
</li>
{{ end }}
```

And I modified it to access the Twitter parameter (`$.Site.Params.twitter`), use the Twitter icon (`class="fa-twitter"`), and link to the Twitter website.

```html
{{ with $.Site.Params.twitter }}
<li>
  <i class="fa-li fa fa-twitter fa-2x" aria-hidden="true"></i>
  <span>
  <a href="https://twitter.com/{{ . }}">{{ . }}</a>
  </span>
</li>
{{ end }}
```

## Adding a CV <a id="adding-cv"></a>

I use an [HTML template](http://www.thomashardy.me.uk/free-responsive-html-css3-cv-template) for my CV and wanted to link to both the HTML and PDF versions.

First, I added a CV section to my top menu bar by adding the following TOML to `config.toml`:

```toml
[[menu.main]]
  name = "CV"
  url = "#cv"
  weight = 5
```

Next, I created a directory called `cv/` within the `content/` directory and added the HTML and PDF versions of my CV, `index.html` and `cv.pdf` respectively to `content/cv/`. Because my HTML CV relies on a stylesheet (`cv.css`), I added it to `static/css/`, and I link to it in `index.html` with the following:

```html
<link type="text/css" rel="stylesheet" href="../css/cv.css">
```

The relative linking (`../css/cv.css`) looks as such because the directory structure that is generated in `public/` looks like this:

```
|-public/
|---index.html
|---css/
|------cv.css
|---cv/
|------cv.pdf
|------index.html
```

Last, I created `cv.md` in `content/home/` by duplicating the `teaching.md` file that comes with the theme by default. You can view my `cv.md` file [on GitHub](https://raw.githubusercontent.com/lmyint/personal_site/master/content/home/cv.md). The main hurdle in linking external resources is figuruing out the correct relative paths to these files. Again, looking at the generated directory structure above, we have to specify paths relative to `index.html`. So I include the following in `cv.md`.

```markdown
My CV is available in [HTML](cv/) or [PDF](cv/cv.pdf) form.
```

## Version control <a id="version-control"></a>

My website is hosted with [GitHub pages](https://pages.github.com/), and the [associated repository](https://github.com/lmyint/lmyint.github.io) only contains the file in the `public` directory of my Hugo project.

I used the [Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/) tutorial to figure out that the `public` directory can be set up as a [git submodule](https://github.com/blog/2104-working-with-submodules) within an enclosing git repository containing source files. The enclosing git repository for my website is available at: https://github.com/lmyint/personal_site.
