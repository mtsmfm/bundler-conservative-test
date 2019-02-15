# Bundler conservative updating test

According to [the doc](https://bundler.io/man/bundle-install.1.html#CONSERVATIVE-UPDATING),
Bundler tries to keep the versions.

> it will continue to use the exact same versions of all dependencies as it used before the update

But I found Bundler updates dependencies if gemspec is changed.

Is this intentional?

## Steps to reproduce

```
$ git clone https://github.com/mtsmfm/bundler-conservative-test
$ cd bundler-conservative-test
$ git checkout e5ea7a4
$ bundle install
$ ruby -r bundler/setup -r rack -e 'puts Rack.release'
```

## Expected behavior

`ruby -r bundler/setup -r rack -e 'puts Rack.release'` shows `1.6.11`

## Actual behavior

`ruby -r bundler/setup -r rack -e 'puts Rack.release'` shows `2.0.6`

## Environment

```
Bundler       2.0.1
  Platforms   ruby, x86_64-linux
Ruby          2.6.1p33 (2019-01-30 revision 66950) [x86_64-linux]
  Full Path   /usr/local/bin/ruby
  Config Dir  /usr/local/etc
RubyGems      3.0.2
  Gem Home    /usr/local/bundle
  Gem Path    /root/.gem/ruby/2.6.0:/usr/local/lib/ruby/gems/2.6.0:/usr/local/bundle
  User Path   /root/.gem/ruby/2.6.0
  Bin Dir     /usr/local/bundle/bin
Tools
  Git         2.11.0
  RVM         not installed
  rbenv       not installed
  chruby      not installed
```

## Bundler Build Metadata

```
Built At          2019-01-04
Git SHA           d7ad2192f
Released Version  true
```

## Bundler settings

```
path
  Set via BUNDLE_PATH: "/usr/local/bundle"
app_config
  Set via BUNDLE_APP_CONFIG: "/usr/local/bundle"
silence_root_warning
  Set via BUNDLE_SILENCE_ROOT_WARNING: true
```

## Gemfile

### Gemfile

```ruby
# frozen_string_literal: true

source "https://rubygems.org"

git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

# gem "rails"

gem "dummy", path: "dummy"
```

### Gemfile.lock

```
PATH
  remote: dummy
  specs:
    dummy (0.1.0)
      rack (< 3.0)

GEM
  remote: https://rubygems.org/
  specs:
    rack (2.0.6)

PLATFORMS
  ruby

DEPENDENCIES
  dummy!

BUNDLED WITH
   2.0.1
```
