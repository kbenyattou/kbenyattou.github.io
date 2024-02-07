source "https://rubygems.org"
# To use a different version of Jekyll, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
# bundle exec jekyll serve

# To use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run
# bundle update github-pages`.

# The latest github-pages (228) gem depends on jekyll 3.9.3
gem "jekyll", "~> 3.9.3"
gem "github-pages", "~> 228", group: :jekyll_plugins

# More plugins
# gem "kramdown-parser-gfm"

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-toc", "~> 0.17.1"
  # gem "public_suffix", "~> 4.0.7"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem
# do not have a Java counterpart.
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
gem "webrick"
gem "faraday-retry"