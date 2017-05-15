`mumukit` - though the `mumukit-directives`  gem implements a _directives pipeline_. Directives are special interpolations that you can place in your tests and extra code, that allow teacher's and student's code to be combined in special ways. For example, if you send the following request to the C++ runner:

```json
{ 
  "test": "/*...content...*/ baz /*...extra...*/",
  "content": "foo"
  "extra": "bar"
}
```

The runner will treat those `/*... ...*/` comments specially, and transform the request to the following before passing it to the usual hooks: 

```json
{ 
  "test": "foo baz bar",
  "content": ""
  "extra": ""
}
```

Discussion of directives is beyond this tutorial - see `mumukit-directives`, except for one thing: in order to make them work you need to specify the comment type of your language. Currently the following three comment types are supported: 

   *  `Mumukit::Directives::CommentType::Cpp` (the default)
   *  `Mumukit::Directives::CommentType::Ruby`
   *  `Mumukit::Directives::CommentType::Haskell`
    
Specify it in you `<runner>_runner.rb` file. For example: 

```ruby
...
Mumukit.configure do |config|
  ...
  config.comment_type = Mumukit::Directives::CommentType::Ruby
end
```
