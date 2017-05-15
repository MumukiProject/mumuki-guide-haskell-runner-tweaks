Sometimes yo ur technology may expose specific keywords or constructs that you would like to disable completly. A simply approach to this is to implement a `validation_hook` in `lib/validation_hook.rb`. For example, this prevents Haskell runner to execute code that referencies the module `System.IO.Unsafe`: 

```ruby
class HaskellValidationHook < Mumukit::Hook
  def validate!(request)
    raise Mumukit::RequestValidationError, 'you can not use unsafe io' if unsafe?(request)
  end

  def unsafe?(request)
    [
        request.content,
        request.test,
        request.extra,
        request.query
    ].compact.any? { |it| it.include? 'System.IO.Unsafe' }
  end
end
```

A `validation_hook` takes a `request` object that represents the raw `POST /test` or `/POST query` request. It expo ses the following methods:

* `content`: the student's solution. 
  * `nil` on `/query` requests. 
* `test`: the teacher's test. 
  * `nil` on `/query` requests. 
* `query`: the student's query. 
  * `nil` on `/test` requests. 
* `extra`: extra code provided by the teacher, that may or may not be visible to the student
  * `nil` if teacher has not provided extra code
  
Then require this file in your `<runner>_runner.rb` file. E.g: 

```ruby
require 'mumukit'

...

require_relative './validation_hook'
```
