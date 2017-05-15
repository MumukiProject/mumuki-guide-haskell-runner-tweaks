By default, `mumukit` assumes that the output of your dockerized test runner command is plain text. For example, the default `rspec` output looks like the following: 

```
...
Mumukit::Hook
  should eq "bar"
  should raise NoMethodError
  should raise NoMethodError
  should raise NoMethodError

Finished in 6.78 seconds
60 examples, 0 failures, 1 pending
Coverage report generated for RSpec to /home/franco/tmp/mumuki/mumukit/coverage. 739 / 794 LOC (93.07%) covered.
```

Whih is undoubtfully plain text. However, there may be commands that return other formats, or you can even tweak the previously discussed `post_process_file` to transform the output. 

In those cases, you can specify a different content-type for your runner output. Currently the following formats -provided by the `mumukit-content-type` gem - are supported: 

* `Mumukit::ContentType::Plain`
* `Mumukit::ContentType::Html`
* `Mumukit::ContentType::Markdown` - with emoji support too :stuck_out_tongue:

As with previous tweaks, you can declare it in your `mumukit` configuration in `<runner>_runner.rb`: 

```ruby
Mumukit.configure do |config|
  ...
  config.content_type = 'markdown'
end
```