# James Mendel's Github Pages Site
### jamesmendel.github.io
----
This site was built off of the [minimal](https://github.com/pages-themes/minimal) theme for [Jekyll](https://jekyllrb.com/) with support for Github pages.

## Compilation
This site will be compiled and go live automatically from the master branch.  

##### For local development / compilation:
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