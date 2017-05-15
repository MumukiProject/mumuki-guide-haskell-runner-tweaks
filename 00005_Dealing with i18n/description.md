`mumukit` already comes with support for **i18n**, thanks to the standard `i18n` gem. In any hook you can use the standard `I18n.t` method, which will use the propert locale from the request (if available).

In order to add custom translations, just declare them in `/locales/` - for example `/locales/es.yml` - and then require them before doing `Mumukit.configure`. E.g.: 

```ruby
require 'mumukit'

I18n.load_translations_path File.join(__dir__, 'locales', '*.yml')

Mumukit.runner_name = 'prolog'
Mumukit.configure do |config|
   ...
end
```