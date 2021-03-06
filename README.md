# 2017year-end-summary
2017年终总结


# 自豪的采用 [reveal.js](https://github.com/hakimel/reveal.js.git) [![Build Status](https://travis-ci.org/hakimel/reveal.js.svg?branch=master)](https://travis-ci.org/hakimel/reveal.js) <a href="https://slides.com?ref=github"><img src="https://s3.amazonaws.com/static.slid.es/images/slides-github-banner-320x40.png?1" alt="Slides" width="160" height="20"></a>

推荐在mac下运行，打包发布，windows下有问题。。。

A framework for easily creating beautiful presentations using HTML. [Check out the live demo](http://revealjs.com/).

reveal.js comes with a broad range of features including [nested slides](https://github.com/hakimel/reveal.js#markup), [Markdown contents](https://github.com/hakimel/reveal.js#markdown), [PDF export](https://github.com/hakimel/reveal.js#pdf-export), [speaker notes](https://github.com/hakimel/reveal.js#speaker-notes) and a [JavaScript API](https://github.com/hakimel/reveal.js#api). There's also a fully featured visual editor and platform for sharing reveal.js presentations at [slides.com](https://slides.com?ref=github).

### Markdown

使用markdown语法编写slides

#### External Markdown

```html
<section data-markdown="example.md"  
         data-separator="^\n\n\n"  
         data-separator-vertical="^\n\n"  
         data-separator-notes="^Note:"  
         data-charset="iso-8859-15">
</section>
```

### Full setup

Some reveal.js features, like external Markdown and speaker notes, require that presentations run from a local web server. The following instructions will set up such a server as well as all of the development tasks needed to make edits to the reveal.js source code.

1. Install [Node.js](http://nodejs.org/) (4.0.0 or later)

2. Clone the reveal.js repository
   ```sh
   $ git clone https://github.com/lisztj/2017year-end-summary.git
   ```

3. Install dependencies
   ```sh
   推荐使用淘宝镜像 $ npm install -g cnpm --registry=https://registry.npm.taobao.org
   $ cnpm install
   或
   $ npm install
   ```

4. Serve the presentation and monitor source files for changes
   ```sh
   $ npm start
   或者 $ grunt serve
   打包发布 $ grunt package
   ```

5. Open <http://localhost:8000> to view your presentation

   You can change the port by using `npm start -- --port=8001`.


### Folder Structure
- **css/** Core styles without which the project does not function
- **js/** Like above but for JavaScript
- **plugin/** Components that have been developed as extensions to reveal.js
- **lib/** All other third party assets (JavaScript, CSS, fonts)
