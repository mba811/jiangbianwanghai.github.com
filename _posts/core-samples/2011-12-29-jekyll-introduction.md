---
layout: post
title: "[译文]Jekyll Introduction(Jekyll入门教程)"
category : lessons
tagline: "转载本站文章请注明作者和出处"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

This Jekyll introduction will outline specifically  what Jekyll is and why you would want to use it.
Directly following the intro we'll learn exactly _how_ Jekyll does what it does.

【翻译】这个Jekyll入门教程将大致包括：Jekyll是什么？为什么我们要使用它？之后我们将学习使用Jekyll搭建一个博客。


### Overview （概述）

#### What is Jekyll?（Jekyll是什么？）

Jekyll is a parsing engine bundled as a ruby gem used to build static websites from
dynamic components such as templates, partials, liquid code, markdown, etc. Jekyll is known as "a simple, blog aware, static site generator".

【翻译】Jekyll作为ruby的一个解析引擎插件，可以用它通过动态组件，比如：模版，partials，liquid code，markdown等创建静态站点。Jekyll被认为是一个简单的博客、静态站点生成工具。

#### Examples（实例）

This website is created with Jekyll. [Other Jekyll websites](https://github.com/mojombo/jekyll/wiki/Sites).

【翻译】这个站点就是用Jekyll创建的。其他的站Jekyll站点，请[点击这里](https://github.com/mojombo/jekyll/wiki/Sites)


#### What does Jekyll Do?（Jekyll能做什么？）

Jekyll is a ruby gem you install on your local system.
Once there you can call `jekyll --server` on a directory and provided that directory
is setup in a way jekyll expects, it will do magic stuff like parse markdown/textile files, 
compute categories, tags, permalinks, and construct your pages from layout templates and partials.

【翻译】你可以将Jekyll安装在自己的电脑上。在你站点的根目录运行`jekyll --server`后，它就可以解析markdown/textile文件了（经验分享：我是在D盘根目录创建了一个jiangbianwnaghai.github.com目录，因为我没有将ruby设置成环境变量，我每次启动的时候都是通过这个命令 `D:\Ruby200-x64\bin\ruby.exe D:\Ruby200-x64\bin\jekyll --server` 我在D盘安装的是64位的ruby）

Once parsed, Jekyll stores the result in a self-contained static `_site` folder.
The intention here is that you can serve all contents in this folder statically from a plain static web-server.

【翻译】解析后，Jekyll会将结果存储在 `_site` 目录中。静态服务器可以打开这个目录中的所有内容

You can think of Jekyll as a normalish dynamic blog but rather than parsing content, templates, and tags
on each request, Jekyll does this once _beforehand_ and caches the _entire website_ in a folder for serving statically.

【翻译】你可以将Jekyll认为成一个普通的动态博客但是不需要解析内容、模版、标签。对于每次请求，Jekyll都会从缓存目录 `_site` 中返回数据。

#### Jekyll is Not Blogging Software（Jekyll不是博客软件）

**Jekyll is a parsing engine.**（Jekyll是一个解析引擎）

Jekyll does not come with any content nor does it have any templates or design elements.
This is a common source of confusion when getting started.
Jekyll does not come with anything you actually use or see on your website - you have to make it.

【翻译】Jekyll不包含任何内容、模版和设计元素。刚入门的人总是容易理解错。Jekyll不包含你博客上看到的所有内容。请一定要记住这一点它是一个**解析引擎不是博客软件**。

#### Why Should I Care?（为什么我很在意这一点）

Jekyll is very minimalistic and very efficient.
The most important thing to realize about Jekyll is that it creates a static representation of your website requiring only a static web-server.
Traditional dynamic blogs like Wordpress require a database and server-side code.
Heavily trafficked dynamic blogs must employ a caching layer that ultimately performs the same job Jekyll sets out to do; serve static content.

【翻译】Jekyll非常的简约和高效。你一定要明白，Jekyll可以通过动态的内容创建一个静态的网站，而我们仅仅需要一个静态的服务器来运行它。想wordpres这种传统的博客程序，需要执行数据库和解析动态程序，为了提供高并发必须添加缓存层，最终的目的是将静态内容返回给用户，而这跟Jekyll原理没有什么差别。

Therefore if you like to keep things simple and you prefer the command-line over an admin panel UI then give Jekyll a try.

【翻译】因此，如果你是一个简约主义者又希望在命令行下管理你的博客不如试一试Jekyll。

**Developers like Jekyll because we can write content like we write code:**

【翻译】开发者喜欢Jekyll是因为我们可以像写代码一样写文章。

- Ability to write content in markdown or textile in your favorite text-editor.（你可以使用你喜欢的编辑器来写markdown或者textile）
- Ability to write and preview your content via localhost.（你可以在本地环境预览你的内容）
- No internet connection required.（不需要有网络连接）
- Ability to publish via git.（可以发布到git上）
- Ability to host your blog on a static web-server.（只需要一个静态服务器就能运行你的博客）
- Ability to host freely on GitHub Pages.（可以使用GitHub搭建一个免费的博客）
- No database required.（不需要数据库）

### How Jekyll Works（Jekyll是怎么工作的？）

The following is a complete but concise outline of exactly how Jekyll works.

【翻译】下面将详细的介绍Jekyll是怎么工作的

Be aware that core concepts are introduced in rapid succession without code examples.
This information is not intended to specifically teach you how to do anything, rather it
is intended to give you the _full picture_ relative to what is going on in Jekyll-world.

【翻译】注意，介绍核心概念时没有代码示例。这些内容不是教你做任何事情的，而是给你勾勒出一张Jekyll能做什么的图而已。

Learning these core concepts should help you avoid common frustrations and ultimately 
help you better understand the code examples contained throughout Jekyll-Bootstrap.

【翻译】学习这些核心概念将会帮你避免一些错误，更好的理解Jekyll-Bootstrap中的示例代码。

#### Initial Setup（初始化安装）

After [installing jekyll](http://jekyllbootstrap.com/index.html#start-now) you'll need to format your website directory in a way jekyll expects.
Jekyll-bootstrap conveniently provides the base directory format.

【翻译】Jekyll[安装](http://jekyllbootstrap.com/index.html#start-now)完成后，你必须将站点根目录格式化成Jekyll需要的格式。Jekyll-Bootstrap已经提供了完整的格式化目录。


#### The Jekyll Application Base Format（Jekyll的基本结构）

Jekyll expects your website directory to be laid out like so:（希望您的目录结构跟Jekyll的一样）

    .
    |-- _config.yml（配置文件）
    |-- _includes
    |-- _layouts（布局文件）
    |   |-- default.html（默认模版）
    |   |-- post.html（文章模版）
    |-- _posts（markdwon文件）
    |   |-- 2011-10-25-open-source-is-good.markdown
    |   |-- 2011-04-26-hello-world.markdown
    |-- _site（生成的静态文件）
    |-- index.html（首页）
    |-- assets（宝翰js,css,img）
        |-- css
            |-- style.css
        |-- javascripts


- **\_config.yml**  
  Stores configuration data.（存储配置信息）

- **\_includes**  
  This folder is for partial views.（这个文件保存着需要解析的视图文件）

- **\_layouts**   
  This folder is for the main templates your content will be inserted into.（这个文件主要包含一些模版）
  You can have different layouts for different pages or page sections.（你可以将不同的视图文件放到不同的目录下）

- **\_posts**  
  This folder contains your dynamic content/posts.（包含你的动态内容）
  the naming format is required to be `@YEAR-MONTH-DATE-title.MARKUP@`.（名字已经被格式化成了`@YEAR-MONTH-DATE-title.MARKUP@`）

- **\_site**  
  This is where the generated site will be placed once Jekyll is done transforming it. （这是使用Jekyll生成的静态文件）

- **assets**  
  This folder is not part of the standard jekyll structure.（这个目录不是Jekyll结构的一部分）
  The assets folder represents _any generic_ folder you happen to create in your root directory.（你可以在根目录下的任何一个地方创建这个目录）
  Directories and files not properly formatted for jekyll will be left untouched for you to serve normally.（通常保存一些不需要Jekyll格式化的文件，比如：js,css,picture等）

(read more: <https://github.com/mojombo/jekyll/wiki/Usage>)

（扩展阅读: <https://github.com/mojombo/jekyll/wiki/Usage>）


#### Jekyll Configuration（Jekyll的配置）

Jekyll supports various configuration options that are fully outlined here:
<https://github.com/mojombo/jekyll/wiki/Configuration>

【翻译】Jekyll的完整配置请看这里：<https://github.com/mojombo/jekyll/wiki/Configuration>


### Content in Jekyll（Jekyll的内容）

Content in Jekyll is either a post or a page.
These content "objects" get inserted into one or more templates to build the final output for its respective static-page.

【翻译】Jekyll中的内容要么是一片文章要么是一个页面。这些内容“对象”最终被插入到多个模版中，最终生成的将是静态页面。

#### Posts and Pages（文章和页面）

Both posts and pages should be written in markdown, textile, or HTML and may also contain Liquid templating syntax.
Both posts and pages can have meta-data assigned on a per-page basis such as title, url path, as well as arbitrary custom meta-data.

【翻译】这些文章和页码都可以以markdown,textile,html编写在致辞Liquid语法的模版中，它们也可以设置meta-data，如：title,url路径，甚至是自定义的meta-data

#### Working With Posts（文章的创建过程）

**Creating a Post** （创建一篇文章）
Posts are created by properly formatting a file and placing it the `_posts` folder.

【翻译】新建一个放在 `_posts` 目录下的格式化文件

**Formatting** （格式化）

A post must have a valid filename in the form `YEAR-MONTH-DATE-title.MARKUP` and be placed in the `_posts` directory. 
If the data format is invalid Jekyll will not recognize the file as a post. The date and title are automatically parsed from the filename of the post file.
Additionally, each file must have [YAML Front-Matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter) prepended to its content.
YAML Front-Matter is a valid YAML syntax specifying meta-data for the given file.

【翻译】在 `_posts` 目录下的文件必须有一个有效的文件名`YEAR-MONTH-DATE-title.MARKUP`如果这个文件名无效，这Jekyll无法识别出来， 日期和标题会被自动解析成文件名。此外，每个文件必须符合[YAML Front-Matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter)要返回的内容。可以按照YAML语法给每个文件定义meta-data。

**Order**（排序）

Ordering is an important part of Jekyll but it is hard to specify a custom ordering strategy.
Only reverse chronological and chronological ordering is supported in Jekyll.

Since the date is hard-coded into the filename format, to change the order, you must change the dates in the filenames.

【翻译】排序是Jekyll的重要功能，但是很难实现用户自定义规则。在Jekyll中只支持按时间顺序和倒序排列。由于文件名是被日期格式化的，改变属性只需修改文件名。

**Tags** （标签）

Posts can have tags associated with them as part of their meta-data.
Tags may be placed on posts by providing them in the post's YAML front matter.
You have access to the post-specific tags in the templates. These tags also get added to the sitewide collection.

【翻译】通常将标签等信息按照YAML格式添加到文件的顶部，你也可以在模版中查看这些标签，这些标签也会集中存放在sitewide中。

**Categories** （分类）

Posts may be categorized by providing one or more categories in the YAML front matter.
Categories offer more significance over tags in that they can be reflected in the URL path to the given post.
Note categories in Jekyll work in a specific way.
If you define more than one category you are defining a category hierarchy "set".

【翻译】文章可以被归类，可以在文件头部按照YAML格式定义一个或多个分类。类别提供更多的意义在于标签，它可以通过URL反链到文章。注意类别在Jekyll在以一种特殊的方式运行。如果要定义多个分类，可以在文件头部以一下格式设置。

Example:

    ---
    title :  Hello World
    categories : [lessons, beginner]
    ---

This defines the category hierarchy "lessons/beginner". Note this is _one category_ node in Jekyll.
You won't find "lessons" and "beginner" as two separate categories unless you define them elsewhere as singular categories.

【翻译】定义类目结构“lessons/beginner”。注意是类别_节点这种结构，你不会发现"lessons" 和 "beginner"是两个不同的类别，除非你在其他地方定义了它们作为单一类别。

#### Working With Pages（页面的创建过程）

**Creating a Page**  （创建一个页面）

Pages are created by properly formatting a file and placing it anywhere in the root directory or subdirectories that do _not_ start with an underscore.

【翻译】页面是由正确格式化文件并将其放置在任何地方在根目录或子目录

**Formatting**  （格式化）

In order to register as a Jekyll page the file must contain [YAML Front-Matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter).
Registering a page means 1) that Jekyll will process the page and 2) that the page object will be available in the `site.pages` array for inclusion into your templates.

【翻译】页面必须包含[YAML Front-Matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter)。Jekyll将会访问在`site.pages`的所有模版页面。

**Categories and Tags**  （分类和标签）

Pages do not compute categories nor tags so defining them will have no effect.

【翻译】页面不具备分类和标签功能，因此定义它们也没有意义。

**Sub-Directories**  （子目录）

If pages are defined in sub-directories, the path to the page will be reflected in the url.
Example:

    .
    |-- people
        |-- bob
            |-- essay.html

This page will be available at `http://yourdomain.com/people/bob/essay.html`

【翻译】如上例，如果页面被定义了目录结构，那么这个页面的url地址将会是这样的的`http://yourdomain.com/people/bob/essay.html`


**Recommended Pages**  （默认页面）

- **index.html**  
  You will always want to define the root index.html page as this will display on your root URL.（这个就不用说了是一个默认文件）
- **404.html**  
  Create a root 404.html page and GitHub Pages will serve it as your 404 response.（404页面，你懂的）
- **sitemap.html**  
  Generating a sitemap is good practice for SEO.（站点地图，SEO专用）
- **about.html**  
  A nice about page is easy to do and gives the human perspective to your website.（个人简介）


### Templates in Jekyll（模版）

Templates are used to contain a page's or post's content.
All templates have access to a global site object variable: `site` as well as a page object variable: `page`.
The site variable holds all accessible content and metadata relative to the site.
The page variable holds accessible data for the given page or post being rendered at that point.

【翻译】文章和页面都会使用到模版。模版可以访问站点的全局变量`site`以及`page`变量。站点变量保存着访问内容和metadata.

**Create a Template**  （创建模版）

Templates are created by properly formatting a file and placing it in the `_layouts` directory.

【翻译】编写好的模版存放在`_layouts`目录中。

**Formatting**  （格式化）

Templates should be coded in HTML and contain YAML Front Matter. 
All templates can contain Liquid code to work with your site's data.

模版可以使用HTML和YRML进行编写。所有的模版可以通过Liquid代码调用您的站点数据。

**Rending Page/Post Content in a Template**  （在模版中渲染页面/文章的内容）

There is a special variable in all templates named : `content`.
The `content` variable holds the page/post content including any sub-template content previously defined.
Render the content variable wherever you want your main content to be injected into your template:

【翻译】所有模版都可以引用`content`变量。`content`变量可以将你的内容替换到模版当中。

{% capture text %}...
<body>
  <div id="sidebar"> ... </div>
  <div id="main">
    |.{content}.|
  </div>
</body>
...{% endcapture %}
{% include JB/liquid_raw %}

### Sub-Templates（子模版）

Sub-templates are exactly templates with the only difference being they 
define another "root" layout/template within their YAML Front Matter.
This essentially means a template will render inside of another template.

【翻译】其实子模版就是这个模版可以被另外一个模版包含。

### Includes（包含目录）
In Jekyll you can define include files by placing them in the `_includes` folder.
Includes are NOT templates, rather they are just code snippets that get included into templates.
In this way, you can treat the code inside includes as if it was native to the parent template.

【翻译】你可以在`_includes`定义一些包含文件，包含文件不是模版。而只是一些代码片段，可以被模版引用。通过这种方式，你可以在模版中根据逻辑判断来引入不同的包含文件。

Any valid template code may be used in includes.

【翻译】任何模版都可以使用包含文件。

### Using Liquid for Templating（在模版中使用Liquid）

Templating is perhaps the most confusing and frustrating part of Jekyll.
This is mainly due to the fact that Jekyll templates must use the Liquid Templating Language.

【翻译】模版是最重要的一部分也是最难的。因为，模版需要使用Liquid语言。

#### What is Liquid?（什么是Liquid）

[Liquid](https://github.com/Shopify/liquid) is a secure templating language developed by [Shopify](http://shopify.com).
Liquid is designed for end-users to be able to execute logic within template files 
without imposing any security risk on the hosting server.

【翻译】Liquid是Shopify开发的一种安全的模版语言。Liquid被设计用来提供给终端用户在模版中实现业务逻辑的。在服务器上不存在任何风险。

Jekyll uses Liquid to generate the post content within the final page layout structure and as the primary interface for working with
your site and post/page data. 

【翻译】Jekyll使用Liquid在模版中用来实现业务逻辑和数据调取。

#### Why Do We Have to Use Liquid?（为什么我们要使用Liquid）

GitHub uses Jekyll to power [GitHub Pages](http://pages.github.com/). 
GitHub cannot afford to run arbitrary code on their servers so they lock developers down via Liquid.

【翻译】GitHub使用Jekyll驱动[GitHub Pages](http://pages.github.com/). GitHub不运行除了Liquid代码以外的任何代码。

#### Liquid is Not Programmer-Friendly. （Liquid是不友好的）

The short story is liquid is not real code and its not intended to execute real code.
The point being you can't do jackshit in liquid that hasn't been allowed explicitly by the implementation.
What's more you can only access data-structures that have been explicitly passed to the template. 

【翻译】这一点儿不是很理解，回头再翻译。

In Jekyll's case it is not possible to alter what is passed to Liquid without hacking the gem or running custom plugins. 
Both of which cannot be supported by GitHub Pages.

【翻译】这一点儿不是很理解，回头再翻译。

As a programmer - this is very frustrating.（作为一个程序员这是一个坏消息）

But rather than look a gift horse in the mouth we are going to 
suck it up and view it as an opportunity to work around limitations and adopt client-side solutions when possible.

【翻译】这一点儿不是很理解，回头再翻译。

**Aside** （题外话）  
My personal stance is to not invest time trying to hack liquid. It's really unnecessary
_from a programmer's_ perspective. That is to say if you have the ability to run custom plugins (i.e. run arbitrary ruby code)
you are better off sticking with ruby. Toward that end I've built [Mustache-with-Jekyll](http://github.com/plusjade/mustache-with-jekyll)

【翻译】个人认为没有必要花时间去研究hack liquid。如果你有能力开发插件的话。你最好使用ruby，我们已经做了这些工作[Mustache-with-Jekyll](http://github.com/plusjade/mustache-with-jekyll)


### Static Assets（静态资源）

Static assets are any file in the root or non-underscored subfolders that are not pages.
That is they have no valid YAML Front Matter and are thus not treated as Jekyll Pages.

Static assets should be used for images, css, and javascript files. 

【翻译】静态资源可以放在根目录的任何地方。他们没有YAML头格式是不被Jekyll解析的页面。静态资源可以是images,css,javascript文件。


### How Jekyll Parses Files（Jekyll是怎么解析文件的）

Remember Jekyll is a processing engine. There are two main types of parsing in Jekyll.

【翻译】谨记Jekyll是一个处理引擎。Jekyll可以解析两种类型。

- **Content parsing.**   （内容解析）

  This is done with textile or markdown.（可以是textile和markdown文件）
- **Template parsing.**   （模版解析）


  This is done with the liquid templating language.（可以是含有liquid语法的模版文件）

And thus there are two main types of file formats needed for this parsing.

【上诉两种类型的文件都将被解析，老外有时候说话就是这么啰嗦。呵呵】

- **Post and Page files.**  （文章和页面）

  All content in Jekyll is either a post or a page so valid posts and pages are parsed with markdown or textile.

  【翻译】Jekyll将会支持格式为文章和页面的markdown、textile的内容解析出来。

- **Template files.**    （模版文件）

  These files go in `_layouts` folder and contain your blogs **templates**. They should be made in HTML with the help of Liquid syntax.
  Since include files are simply injected into templates they are essentially parsed as if they were native to the template.

  【翻译】`_layouts`存放着博客的模版，这些模版会将解析成html.包含文件一旦被载入到模版中后其实就是模版文件，一样会被解析的。

**Arbitrary files and folders.**  （其他文件和目录）

Files that _are not_ valid pages are treated as static content and pass through 
Jekyll untouched and reside on your blog in the exact structure and format they originally existed in.

【翻译】放心吧，Jekyll不会解析他不支持的文件。

#### Formatting Files for Parsing.（格式化文件解析）

We've outlined the need for valid formatting using **YAML Front Matter**.
Templates, posts, and pages all need to provide valid YAML Front Matter even if the Matter is empty.
This is the only way Jekyll knows you want the file processed.

【翻译】我们列出来了需要声明文件的头。模版，文章和页面都需要提供AML Front Matter。即便是为空的。

YAML Front Matter must be prepended to the top of template/post/page files:

【翻译】YAML Front Matter必须添加到模版、文章、页面的最顶端。

    ---
    layout: post
    category : pages
    tags : [how-to, jekyll]
    ---

    ... contents ...

Three hyphens on a new line start the Front-Matter block and three hyphens on a new line end the block.
The data inside the block must be valid YAML.

【翻译】上下都必须有三个连字符将YAML隔开。里面按照YAML格式进行编写。如上所示。

Configuration parameters for YAML Front-Matter is outlined here:
[A comprehensive explanation of YAML Front Matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter)

【翻译】这里有跟详细的配置参数：[A comprehensive explanation of YAML Front Matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter)

##### Defining Layouts for Posts and Templates Parsing.（设定布局）

The `layout` parameter in the YAML Front Matter defines the template file for which the given post or template should be injected into.
If a template file specifies its own layout, it is effectively being used as a `sub-template.`
That is to say loading a post file into a template file that refers to another template file with work in the way you'd expect; as a nested sub-template.

【翻译】布局参数也需要提前在模版页面顶部声明。如果一个模板文件指定它自己的布局,它实际上是被用作“子模板。也就是说加载一篇文件成一个模板文件,指的是另一个模板文件,以这种方式工作或者作为一个嵌套的子模板。



### How Jekyll Generates the Final Static Files.

Ultimately, Jekyll's job is to generate a static representation of your website. 
The following is an outline of how that's done:

1. **Jekyll collects data.**   
  Jekyll scans the posts directory and collects all posts files as post objects. It then scans the layout assets and collects those and finally scans other directories in search of pages.

2. **Jekyll computes data.**   
  Jekyll takes these objects, computes metadata (permalinks, tags, categories, titles, dates) from them and constructs one 
  big `site` object that holds all the posts, pages, layouts, and respective metadata.
  At this stage your site is one big computed ruby object.

3. **Jekyll liquifies posts and templates.**  
  Next jekyll loops through each post file and converts (through markdown or textile) and **liquifies** the post inside of its respective layout(s).
  Once the post is parsed and liquified inside the the proper layout structure, the layout itself is "liquified".   
  **Liquification** is defined as follows: Jekyll initiates a Liquid template, and passes a simpler hash representation of the ruby site object as well as a simpler
  hash representation of the ruby post object. These simplified data structures are what you have access to in the templates.
  
3. **Jekyll generates output.**   
  Finally the liquid templates are "rendered", thereby processing any liquid syntax provided in the templates
  and saving the final, static representation of the file.
 
**Notes.**  
Because Jekyll computes the entire site in one fell swoop, each template is given access to 
a global `site` hash that contains useful data. It is this data that you'll iterate through and format 
using the Liquid tags and filters in order to render it onto a given page.

Remember, in Jekyll you are an end-user. Your API has only two components:

1. The manner in which you setup your directory.
2. The liquid syntax and variables passed into the liquid templates.

All the data objects available to you in the templates via Liquid are outlined in the **API Section** of Jekyll-Bootstrap.
You can also read the original documentation here: <https://github.com/mojombo/jekyll/wiki/Template-Data>

### Conclusion

I hope this paints a clearer picture of what Jekyll is doing and why it works the way it does.
As noted, our main programming constraint is the fact that our API is limited to what is accessible via Liquid and Liquid only.

Jekyll-bootstrap is intended to provide helper methods and strategies aimed at making it more intuitive and easier to work with Jekyll =)

**Thank you** for reading this far.

### Next Steps

Please take a look at [{{ site.categories.api.first.title }}]({{ BASE_PATH }}{{ site.categories.api.first.url }}) 
or jump right into [Usage]({{ BASE_PATH }}{{ site.categories.usage.first.url }}) if you'd like.