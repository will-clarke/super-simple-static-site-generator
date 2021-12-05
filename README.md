# Super Simple Static Site Generator (`ssssg`)

I took heavy inspiration for this script from [ssg](https://www.romanzolotarev.com/ssg.html), written by Roman Zolotarev and an updated version of this, [ssg5](https://github.com/fmash16/ssg5) by u/fmash16.

## Features:

- 115 lines of fabulous shell script!
- Home page with a description of all posts
- Post feed, archived by date
- RSS Feed
- A comprehensive tagging system (tags page, pages per tag, tag links)
- Pandoc

## How to:

## Dependencies

`ssssg` uses [pandoc](https://pandoc.org/) to generate `html` from `markdown`.

## Some stuff that I could probably improve upon

- It's not very SEO-friendly. No sitemap. No opengraph tags.
- It's kind of slow. We delete and regenerate all pages and tags each time.

## File structure

This script assumes a file structure as follows:

``` sh
├── src
│   ├── _bottom.html
│   ├── _top.html
│   ├── css
│   │   └── style.css
│   └── posts
│       ├── 2021-01-01--an-example-post.md
│       └── 2022-01-01--a-cloned-post-to-show-tags.md

```
- This structure isn't 100% necessary, but recommended.
- You can put any files in any directory (including the top-level)
- Posts should go in the `src/posts` directory. Special logic happens to posts (tags and a list of posts by date).
- You can chuck whatever CSS you want into the `src/css` directory.
- the `src/_{top,bottom}.html` files are optional but useful for making sites look half-decent.

## Post structure

You should write your posts in markdown.
Frontmatter is a vital part of how this script works. Each post should have these frontmatter fields:

`date` `title` `description` `tags`

An example

``` sh
---
date: 2022-01-01
title: An example post title
description: This is a super cool way to generate static stites
tags: ssssg simple web
---

.. the rest of the md document..
```

The rest of the post should be in markdown.

## Example
Here's an example script to get you up and running. 
Alternatively you can look at https://git.sr.ht/~will-clarke/blog for inspiration.

``` sh
mkdir -p src/posts src/css

echo "---
date: 2022-01-01
title: An example post title
description: This is a super cool way to generate static stites
tags: ssssg simple web
---

Here's the actual Markdown content
- nice
" | tee \
    src/posts/2021-01-01--an-example-post.md \
    src/posts/2022-01-01--a-cloned-post-to-show-tags.md

echo "
---
title: All about me
---

Some self-centered ramblings
" > src/about.md

echo "<h1>Hey everyone</h1>
<nav>
<a href=\"/about.html\">about</a>
<a href=\"/tags.html\">tags</a>
<a href=\"/posts.html\">posts</a>
</nav>" > src/_top.html
echo "<footer>That's all Folks!</footer>" > src/_bottom.html
echo "h1 {color: red;}" > src/css/style.css

./ssssg
```
