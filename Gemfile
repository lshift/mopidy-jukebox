source 'https://rubygems.org'
gem "chef", '~> 11.12'

gem 'berkshelf', '~> 4.0'

group :integration do
  gem 'test-kitchen', '~> 1.9', '>= 1.9.2'
  gem 'foodcritic'
end

group :docker do
  gem 'kitchen-docker', '~> 2.4'
end

# Ruby 2.2 support
gem 'nokogiri', '<1.10'