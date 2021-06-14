---
title: "On making and eating one's own cake: How and why I created a personal website"
use_math: true
toc: true
---

When one considers the plethora of social media outlets, abundance of blogging sites, and myriad random infotainment destinations that already checker the expanse of the internet, one may justifiably ponder whether adding one more website is the best approach to making oneself heard. Certainly, there are ways to make oneself heard and have a presence on the internet without it. Entire business ventures are built on Facebook pages, and Medium has a domineering presence in the scientific blogging world.

The reasons I decided to make my own website largely center around the themes of _extensibility_, _autonomy_, and _personal growth_.  In this post I will go over how this site meets those needs, and hopefully provide some useful insight to anyone else trying to make their own.

# Extensibility
While I do have some idea of the domain of content that will find its way to this website, I cannot be certain.  Therefore using a website builder with strict limits on the type of functionality that could be used from the outset seems like quite the ascetic exercise. Here I want to go over the way I implemented support for some of the primary features I wanted.

## Including Math
To include math, I took cues from [this blog post](http://benlansdell.github.io/computing/mathjax/) by Ben Lansdell, coincidentally a computational neuroscientist using the same site template I am. I found that a slight alteration to `mathjax_support.html` was required to get it working. [This blog post](https://www.cross-validated.com/How-to-render-math-on-Minimal-Mistakes/) by Katerina Bosko was also helpful here.
``` html
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    TeX: {
      equationNumbers: {
        autoNumber: "AMS"
      }
    },
    tex2jax: {
    inlineMath: [ ['$', '$'] ],
    displayMath: [ ['$$', '$$'] ],
    processEscapes: true,
  }
});
MathJax.Hub.Register.MessageHook("Math Processing Error",function (message) {
	  alert("Math Processing Error: "+message[1]);
	});
MathJax.Hub.Register.MessageHook("TeX Jax - parse error",function (message) {
	  alert("Math Processing Error: "+message[1]);
	});
</script>
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
<!-- <script type="text/javascript" async
    src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script> -->
```

Thanks to this, I can now include math as in the following example from [here](https://kramdown.gettalong.org/syntax.html#math-blocks)
<div>
$$
\begin{aligned}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{aligned}
$$
</div>
While this does still require a `<div>` as mentioned by Ben, I am pleased with the ability to easily include standard latex from my academic writing that this offers. This also includes paragraphs that require intermingling latex in exposition, such as $$\lambda = \frac{h}{mv}$$.

## Citation Management
Personally, I have zero interest in manually formatting my references. For this reason, in my $$\LaTeX $$ workflow, I have adopted [Better Bibtex for Zotero](https://github.com/retorquere/zotero-better-bibtex) (BBZ), which works beautifully for streamlining the citation process.  Furthermore, BBZ also works with markdown, so it is an ideal tool for use with a Jekyll blog.  To add bibtex capabilities to kramdown, I opted to add the [`jekyll-scholar`](https://github.com/inukshuk/jekyll-scholar) gem to my site.

First, I find whatever article I am interested in, and use the Zotero browser extension to add it to the blog folder of my library.

![Adding a reference to an article using the Firefox browser extension](/assets/images/add_to_zotero.png)

BBZ then automatically updates the `blog.bib` file at `./assets/bib/blog.bib`. 
  
![Blog bibliography is updated](/assets/images/blog_bib.png)

I can then press `alt+Shift+z` to bring up the Zotero search bar using the [vscode-zotero](https://github.com/mblode/vscode-zotero) vscode extension, and then easily add a reference to whatever article in the markdown for the blog post {% cite bollaertAssociationsFunctionalConnectivity2018 %}. When the site is built, `jekyll-scholar` then does the work of formatting the citation and bibliography in the APA style.

## Data Exploration
While I do a fair amount of work in MATLAB and aspire to learn Julia eventually, my current daily driver for scientific programming is Python 3.  I wanted to determine a plotting library that would allow for full-fledged interactivity while staying true to the open-source philosophy, and from what I can tell Plotly and Bokeh are the prime candidates. Below you should see a simple demo dashboard app made with Bokeh. At this point I am not entirely beholden to either, so it remains to be decided what I will primarily use in the future. Check out [this great blog post](https://pauliacomi.com/2020/06/07/plotly-v-bokeh.html) for a comparison.

<script
    src="https://demo.bokeh.org/sliders/autoload.js?bokeh-autoload-element=1000&bokeh-app-path=/sliders&bokeh-absolute-url=https://demo.bokeh.org/sliders"
    id="1000">
</script>

# Autonomy
Furthermore, the way data is managed is important to me. I want to feel like I am "in control" of my website, which connotes the ability to easily move my website to a different provider, efficiently and intuitively manage site data, and utilize version control (git).  The solution I came to was to build a site off of the excellent Minimal Mistakes theme and augment it with the pieces I need. I also opted to host with Github pages, because I like the idea of associating my website with my Github presence, it is free, and it, of course, integrates wonderfully with a git-oriented development workflow.
Personally, I despise feeling of having limits on what I can and cannot do with technology. It is one of the major reasons that I prefer to do my scientific work in Linux and support open source in general. When it comes to my own website then, I want to feel that I am in control every step of the way.

## Hosting and Content Management
I messed around with a Wordpress site for a bit when I was first considering what platform to build my website with. This, I found, was an option far preferable to options like squarespace, weebly, and other website creation tools of that ilk, due to the autonomy it granted the website owner. There is a very real case to be made that you are only really a website maintainer when you use a site like that to do your hosting, as the bulk of the website code (i.e. the template) is the [exclusive property of the website creation service](https://support.squarespace.com/hc/en-us/articles/206566687-Exporting-your-site?_ga=2.43222245.1387702869.1623619249-135761508.1623619249).  I abhor the thought, so naturally I wanted something where I would have a little bit more say if I wanted to change to a different hosting service or pursue self-hosting in the future.  Wordpress, unfortunately, has an addon library that is saturated with subscription services and is overall unappealing from this perspective. So where would I go?

The answer in retrospect was somewhat obvious.  Github, although now [unfortunately](https://jacquesmattheij.com/what-is-wrong-with-microsoft-buying-github/) owned by Microsoft, is the home of the open source community.  It is from here that I discovered Jekyll, and the opportunity for free hosting through github pages, while being able to retain my own custom domain name (purchased through porkbun, a service I strongly endorse). Motivated in no small part by the price being right, Since my primary motivation to create a website is to maintain a (somewhat) academic blog with lots of code and math, doing so through Github seemed very natural. Furthermore, it is naturally equipped with all the bells and whistles of `git` version control, which I already use extensively for academic purposes.

While I don't have any experience with ruby outside of using Jekyll, I have found it more than ideally suited to my needs.  After perusing some of the excellent, open-source templates on [jekyllthemes.io](https://jekyllthemes.io/), I eventually settled on the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) theme due. The main reasons I decided to go with this theme are the very features it adversites on the homepage:
* __Resposiveness__. The site really feels good, and the template was evidently created with care.
* __Price__ (MIT license, so free!)
* __Customizability and extensibility__. The base configuration is very minimal, as the name might indicate. As I went over in the previous section, nothing about the site hindered my ability to add the features I needed for the type of content I wanted to create, and modifying essentially any aspect of the site is a breeze. Oh and it uses [sass](https://sass-lang.com/).

# Personal Growth
The final, and indisputably most important reason, is because I wanted to build my skillset and personal brand.  I've wanted my own website for some time now, and getting something out there just might enable me to bring to live some of the random blog ideas I've had over time, and gain some much-needed practice writing and engaging in technical communication.  Creating ones own website without the aid of a pre-packaged solution (even with a tool like Jekyll and templates) is a daunting task, and even now the site is far from complete. But of course, this is only the beginning, and I intend to keep adding new components and modifying it for some time... I have a few ideas.

![calvin and hobbes final comic](https://ia601901.us.archive.org/BookReader/BookReaderImages.php?zip=/1/items/calvin-and-hobbes-complete-digital-collection/%28S%29%20Calvin%20and%20Hobbes%20Sunday%20Pages%20-%20Bill%20Watterson_jp2.zip&file=%28S%29%20Calvin%20and%20Hobbes%20Sunday%20Pages%20-%20Bill%20Watterson_jp2/%28S%29%20Calvin%20and%20Hobbes%20Sunday%20Pages%20-%20Bill%20Watterson_0082.jp2&id=calvin-and-hobbes-complete-digital-collection&scale=2&rotate=0 "Every ending marks a new beginning. Image provided through https://archive.org/details/calvin-and-hobbes-complete-digital-collection/")

# References
{% bibliography --cited %}