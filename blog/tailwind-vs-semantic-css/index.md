---
title: Tailwind vs Semantic CSS
desc: Comparing two identically designed websites, their weight, amount of HTML and CSS, rendering speed, and best practices.
og: img/og.jpg
thumb: tw-thumb.jpg
date: 2023-10-23
include: syntax
---

# Tailwind CSS vs. Semantic CSS
This study compares two websites with a similar design: the official Tailwind "spotlight" template from the developers of Tailwind, and the same site crafted with semantic CSS and [Nue server components](/docs/nuejs/server-components.html). We'll take a deep look at what is under the hood and how the sites are built.

[.cols]
  [media]
    href: //spotlight.tailwindui.com/
    small: img/tw-home.png
    large: img/tw-home-big.png
    caption: Tailwind UI version →
  ---
  [media]
    href: /@spotlight/
    large: img/nue-home-big.png
    small: img/nue-home.png
    caption: Semantic version →



[.info]
  #### Gist
  **The semantic version is 8 &times; smaller, renders faster, and is easier to modify and extend.**


## Front page HTML
The main difference is that Tailwind uses "utility" class names and Nue uses semantic class names. That is: Nue gives meaning to the elements like `nav`, `button`, and `.gallery` and styles them with an external stylesheet. Tailwind styles elements inline — directly on the markup.

We can easily see the difference in the HTML markup by drilling down to the first element on the main navigation:

[media]
  small: img/markup.png
  large: img/markup-big.png
  caption: Drilling down to the first element on the main navigation
  class: bordered wider

The semantic version is smaller because it utilizes high-level, semantic components like `.nav` and the external CSS can target elements with CSS selectors like `body > header`. Tailwind needs significantly more markup because the utility-first approach lacks the power of CSS selectors. You are forced to wrap divs inside divs inside divs and fill the elements with Tailwind-specific class syntax.

Here is the full HTML source code of the front page.

[media]
  small: img/html.jpg
  large: img/html-big.jpg
  caption: Full HTML coding on the front page
  class: small-bordered wider

Tailwind (and Next.js) generate 75K of unminified HTML, while the semantic version is only 8K. While some parts come from Next, it's pretty clear that Tailwind requires significantly more HTML to render the same design than the semantic version.

With Tailwind the [Text to HTML Ratio][tw-ratio] is only 2.3%, which is "Very low" according to SiteGuru. [Nue ratio][nue-ratio], however, is 20.3% which is "Good".

[tw-ratio]: //www.siteguru.co/free-seo-tools/text-to-html-ratio?url=spotlight.tailwindui.com

[nue-ratio]: //www.siteguru.co/free-seo-tools/text-to-html-ratio?url=nuejs.org/@spotlight/




## Front page CSS
Let's study the difference in CSS coding:

[media]
  small: img/css.jpg
  large: img/css-big.jpg
  caption: Full CSS coding on the front page
  class: bordered super-wide

Blue is semantic CSS, gray is utility classes, and black-bordered is primary CSS (which makes your pages render faster).

Some key takes:

1. Tailwind CSS is seven times larger: 33K vs 4.6K. Overall you need eight times more HTML/CSS code with Tailwind to render the page (108K vs 12.6K). The most surprising thing is that Tailwind uses more global/semantic CSS than the semantic approach itself `¯\_(ツ)_/¯`

2. Most of the semantic CSS is re-usable on other pages and only a fraction of the CSS is specific to the front page. It's easy to create new pages when the groundwork is already done.

3. "Spotlight" is just a _theme_ extending a base design. There is an extremely minimalistic [base-version](/@base/) of the website that can be used to create new themes, like our Spotlight theme.

[media]
  small: img/extending.png
  large: img/extending-big.png
  caption: Creating a new design by extending a semantic base design
  class: tall

Theming or "skinning" is a powerful concept in semantic CSS. You can alter your design by swapping parts of your CSS with another one or overriding a [base version](/@base/). CSS theming is impossible with Tailwind because the design is coupled to the markup. If you want a new design, you must edit your markup and override your earlier work.



## Rendering speed
The two metrics that measure page rendering speed are [first contentful paint](//web.dev/articles/fcp) (FCP) and [largest contentful paint](//web.dev/articles/lcp) (LCP). The semantic version is faster than in both metrics and in both mobile and PC. Here's LCP on mobile for example:

[media]
  small: img/lcp-mobile.png
  large: img/lcp-mobile-big.png
  caption: Largest Contentful Paint (LCP) rendering speed on mobile
  class: tall


Please compare [Tailwind metrics](//pagespeed.web.dev/analysis/https-spotlight-tailwindui-com/cqtnf4xxoy?form_factor=mobile) with [Semantic CSS metcis ](//pagespeed.web.dev/analysis/https-nuejs-org-spotlight/6nnhwwnz8b?form_factor=mobile).

Two reasons why the semantic version is faster:

1. The primary CSS is [inlined](//imkev.dev/loading-css) on the HTML page so that all the assets for the first viewport are fetched in the initial request. This is probably the most important performance optimization for the perceived page loading experience.

2. The first request is [less than 14K][14k], which is the maximum size of the first TCP packet.

[14k]: //endtimes.dev/why-your-website-should-be-under-14kb-in-size/

The Preview tab on the Developer console is a great way to debug FCP:

[media]
  small: img/first-paint.png
  large: img/first-paint-big.png
  caption: Previewing the first paint on the developer console
  class: small-bordered


## Best practices
The key best practice of Tailwind is **tight coupling**. That is: the structure and styling are tied together. The semantic approach is the opposite: the structure and styling are loosely coupled**. Let's see what that means by studying the gallery component on the front page:

[media]
  small: img/coupling.png
  large: img/coupling-big.png
  caption: Tight coupling vs Loose coupling
  class: wider small-bordered

The semantic version, allows you to change the design of the gallery freely. You name the component and style it externally. With Tailwind the style cannot be separated from the structure.

Here's a better example. Let's look at the "Uses" or "Setup" page on both implementations:

[.cols]
  [media]
    href: //spotlight.tailwindui.com/uses
    small: img/tw-uses.png
    large: img/tw-uses-big.png
    caption: Tailwind UI version →
  ---
  [media]
    href: /@spotlight/setup/
    large: img/nue-uses-big.png
    small: img/nue-uses.png
    caption: Semantic version →

With Tailwind you must create a JavaScript component to construct a suitable HTML structure for the design. With the semantic version, we can use Markdown in place of the custom JSX component because the generated HTML is semantic and can be styled externally with CSS selectors:

[media]
  small: img/content-first.png
  large: img/content-first-big.png
  caption: Tight vs loose coupling from a different angle
  class: wider small-bordered

Loose coupling makes you think **content first**. There is no need to write a component for every situation because you can use external CSS to do the heavy lifting.

&nbsp;

*But ...*


### But naming things is unnecessary
Naming things is a skill. You name things that repeat. Think of function names in JavaScript or component names in Figma. The same goes for CSS class names. Be good at naming, and you can move from repeating things to re-using things. That is: you can move from this:

```
<!-- utility- first css -->
<button className="group mb-8 flex h-10 w-10
  items-center justify-center rounded-full
  bg-white shadow-md shadow-zinc-800/5 ring-1
  ring-zinc-900/5 transition dark:border
  dark:border-zinc-700/50 dark:bg-zinc-800
  dark:ring-0 dark:ring-white/10
  dark:hover:border-zinc-700
  dark:hover:ring-white/20
  lg:absolute lg:-left-5
  lg:mb-0 lg:-mt-2
  xl:-top-1.5
  xl:left-0
  xl:mt-0">
```

To this

```
<!-- semantic css -->
<button class="secondary">
```

Without turning into components.


### But co-location is important?
Co-location is a catchy name for tight coupling. A term to promote the idea that styling should be tied to the presentation. Repeating things vs. re-using things. See above.


### But Tailwind is a great design system
Tailwind has great defaults for colors, spacing, and responsive design. That part is roughly 3% of your Tailwind CSS file. It's easy to copy these defaults to your semantic design system if needed.


### But I move faster with Tailwind
Yes. You can move faster with Tailwind. But only when:

1. You are comparing Tailwind with your earlier, badly structured CSS or you are completely new to CSS development.

2. You don't care about building reusable modules for later use. That is: you are not naming things that repeat.

If you really want to move faster, you'll create a set of CSS components that you can reuse. Like `<button class="secondary">`.


### But why is Tailwind so popular then?
Because mastering CSS requires practice. It takes several failed attempts before you get it. Most developers haven't gone through that so they only remember the bad things. They have never built or used well-structured CSS.

But when you truly master CSS, there is no turning back.

Tailwind, however, forces you to write divs inside divs and fill your elements with an insane amount of class names. I guess that Tailwind's popularity will eventually fade and the codebases will turn to technical debt. It's just a matter of time.



### Where is the source code?
I'm working on it! I wanted to push out this blog entry quickly without the overhead of making this an open-source project. My next task is to work on the [Nue starter kit](//github.com/nuejs/create-nue) so that it generates a personal blog. I'll also write a blog entry called "Next.js vs Nue". It tackles the bigger picture and shows what's wrong with the current front-end ecosystem. You can join the mailing list, and I'll notify you when it's ready:

[join-list]

I'd appreciate your thoughts. I'd like to understand what people think of all this. You can also participate in the [discussion at Hacker News](//news.ycombinator.com/item?id=37982407).

