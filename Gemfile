# Tylko do lokalnego podglądu (bundle exec jekyll serve). GitHub Pages buduje
# stronę sam i tego pliku nie potrzebuje — gem github-pages trzyma te same
# wersje Jekylla i pluginów co Pages.
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
gem "webrick"   # Ruby 3 nie ma już wbudowanego serwera, a jekyll serve go wymaga
gem "faraday-retry"   # wycisza ostrzeżenie „To use retry middleware with Faraday v2.0+” przy starcie

# Windows: strefy czasowe i wydajne śledzenie zmian plików
platforms :windows do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
  gem "wdm", ">= 0.1.0"
end
