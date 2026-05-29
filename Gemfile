source "https://rubygems.org"

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!

# NOTE: The `github-pages` gem pins very old Jekyll/Liquid versions that no
# longer run on modern Ruby (Liquid 4.0.3 calls `tainted?`, removed in Ruby
# 3.2+). For local development we use a modern Jekyll directly. GitHub Pages
# builds the deployed site with its own server-side toolchain, so this does
# not affect deployment.
gem "jekyll", "~> 4.3"
# Liquid 4.0.4 removed the `tainted?` call that breaks on Ruby 3.2+.
gem "liquid", "~> 4.0.4"

gem "wdm", "~> 0.1.0" if Gem.win_platform?

# Formerly-default gems removed from the standard library in recent Ruby
# versions (3.4+/4.0); required by the older Jekyll that github-pages pins.
gem "csv"
gem "base64"
gem "bigdecimal"
gem "logger"

# If you have any plugins, put them here!
group :jekyll_plugins do
  # gem "jekyll-archives"
  gem "jekyll-feed"
  gem 'jekyll-sitemap'
  gem "jekyll-paginate"
  gem "jekyll-gist"
  gem "jekyll-redirect-from"
  gem "webrick", "~> 1.8"
end
