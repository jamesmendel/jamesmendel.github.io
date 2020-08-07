# James Mendel's Github Pages Site
### jamesmendel.github.io
----
This site was built off of the [minimal](https://github.com/pages-themes/minimal) theme for [Jekyll](https://jekyllrb.com/) with support for Github pages.

# Adding content
Most pages are templated dynamically with `.yml` files containing the post content according to some schema. However, some pages (that I don't anticipate changing too often) are hardcoded in good 'ol `markdown`. 

### Coursework
I anticipate that this will be the most updated page.

#### Updating Current Course Year
The current term is stored in `_config.yml`. This probably isn't the best way to manage this, but every semester, update `currentterm` to the correct term.

#### Adding Courses
- All course data is stored in `_data/coursework.yml`
- Add new courses to the bottom of the file according to the following schema:
```yml
code : KU Class code
name : Course number and title
term : term course was completed
desc : short description
link : link to more more information (optional)
hnrs : bool - true if honors course (optional)
``` 
#### Adding new Category
`/coursework.html` renders in an alternate template from the rest of the site, `layouts/coursework.html`. Adding a new category is fairly simple:
- Create new page with `layout: coursework`
- Use the following template, replacing `[CONDITION]` with a logical expression:
```liquid
{% for course in site.data.coursework reversed %}
    {% if [CONDITION] %}

{% include course-list.md %}

    {% endif %}
{% endfor %}
```

### Projects
This section is formatted more like a blog. New projects should be added to `/_posts/` with the following prefix: `YYYY-MM-DD-`

#### Schema
```yml
layout: projects
categories: project
title: "Project Title"
description: "Project Description"
fontawesome: bool (optional - true includes FA script to footer of page)
```

# Compilation
This site will be compiled and go live automatically from the master branch.  

### Local Development + Compilation:
1. Clone repo: `git clone https://github.com/jamesmendel/jamesmendel.github.io`
2. Enter directory: `cd jamesmendel.github.io`
3. Run server: `bundle exec jekyll serve --force-polling`

*Note: you need to have a Gemfile for Jekyll to run.*
```ruby
# frozen_string_literal: true

source 'https://rubygems.org'
gemspec
```
jekyll-theme-minimal.gemspec
```ruby
# frozen_string_literal: true

Gem::Specification.new do |s|
  s.name          = 'jekyll-theme-minimal'
  s.version       = '0.1.1'
  s.license       = 'CC0-1.0'
  s.authors       = ['Steve Smith', 'GitHub, Inc.']
  s.email         = ['opensource+jekyll-theme-minimal@github.com']
  s.homepage      = 'https://github.com/pages-themes/minimal'
  s.summary       = 'Minimal is a Jekyll theme for GitHub Pages'

  s.files         = `git ls-files -z`.split("\x0").select do |f|
    f.match(%r{^((_includes|_layouts|_sass|assets)/|(LICENSE|README)((\.(txt|md|markdown)|$)))}i)
  end

  s.platform = Gem::Platform::RUBY
  s.add_runtime_dependency 'jekyll', '> 3.5', '< 5.0'
  s.add_runtime_dependency 'jekyll-seo-tag', '~> 2.0'
  s.add_development_dependency 'html-proofer', '~> 3.0'
  s.add_development_dependency 'rubocop', '~> 0.50'
  s.add_development_dependency 'w3c_validators', '~> 1.3'
end

```