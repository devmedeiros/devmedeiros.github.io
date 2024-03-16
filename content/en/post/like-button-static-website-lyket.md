---
title: "How to Add Like/Clap Buttons on your Static Website with Lyket"
date: 2023-08-05 07:30:00 -0300
categories: [Blog]
tags: [tutorial, like button, clap button, static site, medium clap, twitter heart, lyket, hugo, jekyll]
showtoc: true
summary: Learn how to add a like/clap button to your static website using Lyket.
cover:
    image: "https://ik.imagekit.io/devmedeiros/like-static-blog_tR0o74mLu.webp?tr=w-700"
    alt: "black and white laptop with three emojis, thumbs up, heart and clap"
---

This tutorial is focused on **Hugo**, but it can easily be adapted for other static website frameworks.

I've found many alternatives to add claps or likes to your website, but none was customizable as I wanted it to be, or it would require me to change my website hosting.

I recently found out about Lyket and started using it. I'll guide you on how to set it up on your website.

## Setting Lyket API

The Lyket API provides a free tier, with no credit card needed to sign-up. To use it on a static site you'll need to add the following tag to your website header.

```html
<script src="https://unpkg.com/@lyket/widget@latest/dist/lyket.js?apiKey=[YOUR-API-KEY]"></script>
```

You can get an API Key on their website when you sign-up << https://lyket.dev/ >>. You can use your public API token without fearing other people using it, you just need to set the allowed websites to your domain.

Then you need to add the button tag where you want it to show, in my case I wanted it to appear on every blog post, but not on the other pages. So I added it to my post footer on my hugo project it's located in `layouts/_default/single.html` file, on pasted the tag inside the `<footer class="post-footer">` tag.

The html button template will look something like this:

```html
<div
    data-lyket-type="clap"
    data-lyket-namespace="namespace"
    data-lyket-id="everybody-clap-now"
    data-lyket-template="medium"
    data-lyket-color-text="currentColor"
/>
```

You can change the `data-lyket-type` to clap, like, updown, and rate. On `data-lyket-namespace` you can choose whatever name you like, you can use it to separate and organize your pages if you use the same API token on multiple websites, or if you separate your website content into categories you could use "blog" and "projects", for example.

On `data-lyket-template` you can choose the template for your button, each button type has a different button template, this is optional, if you don't add it it'll turn to the default option, you can see the available options you can see it here: << https://lyket.dev/templates >>.

The `data-lyket-color-text` allows you to personalize the text color of your button (the color of the numbers), you can set one color, but if you have a website that has a dark and light mode, you may prefer to use 'currentColor'.

The `data-lyket-id` set the counter name, if you want two pages to share the like count it'll need to have the same id. On my Hugo website, I used Hugo to get the page slug to use it as an id, you can use `{{ replace (path.Base .RelPermalink) " /" "-" }}` template as your id, so even if you translate your content all versions will keep their like count.

```html
<div
    data-lyket-type="clap"
    data-lyket-namespace="namespace"
    data-lyket-id="{{ replace (path.Base .RelPermalink) " /" "-" }}"
    data-lyket-template="medium"
    data-lyket-color-text="currentColor"
/>
```

## Bonus Tip

You can add a Hugo parameter to hide the like button on pages you don't want to show. Simply add to your page frontmatter a parameter, in this case `noLike`, and set it to true, so your page would look like this:

```yaml
---
title: "Your Awesome Page Title"
layout: "single"
noLike: true
---
```

Then on your single.html file, you add the if clause to hide the button, in the end, it'll look like this:

```html
{{- if not .Params.noLike }}
    <div
        data-lyket-type="clap"
        data-lyket-namespace="namespace"
        data-lyket-id="{{ replace (path.Base .RelPermalink) " /" "-" }}"
        data-lyket-template="medium"
        data-lyket-color-text="currentColor"
    />
{{- end }}
```