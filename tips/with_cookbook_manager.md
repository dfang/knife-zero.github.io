---
layout: tips
lang: en
title: Cookbook Manager Integration
permalink: tips/with_cookbook_manager/
---


## [Librarian](https://github.com/applicationsonline/librarian-chef)

Ref: [librarian-chef#knife-integration](https://github.com/applicationsonline/librarian-chef#knife-integration)

Add `librarian-chef` to Gemfile, and bundle it.

```
gem 'knife-zero'
gem 'librarian-chef'
```

Stick the following in your knife.rb:

```ruby
require 'librarian/chef/integration/knife'
cookbook_path Librarian::Chef.install_path,
              "/path/to/chef-repo/site-cookbooks"
```

For example. `knife.rb` in your chef_repo root.

```ruby
require 'librarian/chef/integration/knife'
cookbook_path Librarian::Chef.install_path,
              File.expand_path("../site-cookbooks", __FILE__)
```

When you see `Cheffile and Cheffile.lock are out of sync!`, you should correct dependency below two ways.

- `librarian-chef install`
- `librarian-chef clean`


## [Berkshelf](http://berkshelf.com)

Resolve and download Cookbooks which are managed under Berksfile by `berks vendor` into `./cookbooks`.

```
$ berks vendor cookbooks
```

Stick the following in your knife.rb:

```ruby
cookbook_path File.expand_path("../cookbooks", __FILE__),
              "/path/to/chef-repo/site-cookbooks")
```

For example. `knife.rb` in your chef_repo root.

```ruby
cookbook_path File.expand_path("../cookbooks", __FILE__),
              File.expand_path("../site-cookbooks", __FILE__)
```

But in this way for loose integration with  Berksfile, please attention to the contradiction.

To check current cookbooks, try `berks verify`.

### cheap trick to hook.

For example add below lines to `knife.rb`.  
It will run `berks vendor` before every `knife zero converge`.

```
## knife.rb will be read twice at run. (before launch chef-zero locally and before run knife.)
## And configuration for chef-zero doesn't has key `color`.
if ARGV[0..1] == ["zero", "converge"] && ! Chef::Config.has_key?(:color)
  system('bundle exec berks vendor')
end
```

## [Batali](https://github.com/hw-labs/batali)

That is mostly the same as Berkshelf.  Use `batali install`.

## Policyfile

WIP
