---
title: Jekyll site from scratch on github-pages
---

# Create a jekyll blog from scratch

## Setup jekyll
Instead of using a built in jekyll theme (which is created with ```jekyll new myblog``` by default) we will use the blank option, so we can create our own local theme without interferring the default theme.

fire up the windows commandline, and navigate to a directory where you want to keep your blog, and run the following:

`jekyll new bablab-jekyll --blank`

if it throws an error, missing the eventmachine, then install it with

`gem install eventmachine --platform ruby`

> eventmachine on windows has a bug. here is a [workaround](https://httpain.com/blog/jekyll-live-reload-windows/).
> In a nutshell, run: 
> ```
> gem uninstall eventmachine --force  
> gem install eventmachine --platform ruby
> ```
> It's also a good idea to put it in your gem file. So when using ```bundle install``` it will install the lib automatically.
> ```
> gem 'eventmachine', '1.2.7', git: 'https://github.com/eventmachine/eventmachine.git', tag: 'v1.2.7'
> ```

To use our blog with github pages, create a new gem file for github pages inside the blog folder

```
source 'https://rubygems.org'
gem "github-pages", "~> 217", group: :jekyll_plugins
```

Feel free to change the github-pages version. As for today 217 is the latest..
You can find up to date information about supported library  version here: https://pages.github.com/versions/

dont forget to create a .gitignore file to ignore generated files. We wont need these, since github will automatically build out site.

**`.gitignore`**
```
_site
```

## Ready to go

fire up a local dev server by running
`bundle exec jekyll serve --livereload`

and browse to localhost:4000

## Create Navigation
open default.html in an editor.
inside the body tag we will list all our pages, posts and collections.

site.pages list files from any directory not starting with a underscore. Instead we will use site.html_pages to list out pages in the navbar.

```
<nav>
    <ul>
        {% for p in site.html_pages %}
        <li><a href="{{p.url}}">{p.name}}</a></li>
        {% endfor %}
    </ul>
</nav>
```

We also want to have a navbar for our collections. But before we need to create and define our custom collections in config.yml

create a folder named eg.: _projects and put a markdown file inside it.
create frontmatter with title, and layout, and some content
```markdown
---
title: my aesome project
layout: title
---
# Hey
```
_config.yml
```yaml
collections:
  projects:
    output: true # if each project needs an individual page. It does : 
  tutorials:
    output: true
```
> you need to restart the devserver to take effect.
its also a good idea to put this line into your gem file:
```gem 'eventmachine', '1.2.7', git: 'https://github.com/eventmachine/eventmachine.git', tag: 'v1.2.7'```
and run `bundle install; bundle exec jekyll clean;`
to display  all collection and their items in html use the following snippet.
Also at the very first line we skip the posts collection as we dont need that in the navbar.
{% raw %}
```liquid
{% assign collections = site.collections | where_exp:'item', 'item.label != "posts"' %}
{% for collection in collections %}
<li>
  {{collection.label}}
  <ul>
    {% for item in site[collection.label] %}
    <li>
      <a href="{{item.url}}">{{item.title}}</a>
    </li>
    {% endfor %}
  </ul>
</li>
{% endfor %}
```
{% endraw %}

To access post we are using a unique page for that calld News
crete a news.md with a frontmatter and list all the posts there.

also create a properly formatted news file in _posts directory like so:
2020-01-01-awesome_news.md

## Frontmatter Defaults

It is a but cumbersome to specify layout in each page. To use our default.html layout we can specify defautls for frontmatter in _config.yml like so:

```
defaults:
  -
    scope:
      path: "" # apply to all files
    values:
      layout: "default"
```

for more details about defaults and scopes, visit the official documentation: https://jekyllrb.com/docs/configuration/front-matter-defaults/

Dont forget, to the new config to take effect the server must be restarted.

## Setup github pages

first of all init git, and commit git

```
git init
git add --all
git commit -m 'initial commit'
```

create a github repo on github.com and follow the instructions there: 

- add remote origin
- set main as the default branch
- and push to github

Now setup the pages under setting/pages to host from the main branch.

And optionally create a readme.md in root and place the page's link into it.

**README.md**
```
# My cool site

demo: https://zalavariandris.github.io/bablab-jekyll/
```

Your site probably up an running on github-pages now.
But you like to have tou fancy custom domain.

## Custom domain





