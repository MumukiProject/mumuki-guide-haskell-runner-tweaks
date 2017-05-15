It is quite common that the output of the command you are using to run the tests is not human-friendly. For example: 

* it may contain too detailed information, you will like to hide to the user
* it may not behave like a good unix command that returns 0 when command succeed, non-zero otherwise. This is a problem, since by default `mumukit` maps return codes to test and query status: `0` to `:passed`, non-zero to `:errored`, and some special return codes to `aborted`.

In those cases, you should override in the `test_hook` and/or the `query_hook` the `post_process_file` method: 

```ruby
  def post_process_file(file, result, status)
    [result, status]
  end
```

This method takes the raw output of your command - `result` - the raw return code `status` and the original self-contained test-file contents - `file` - and returns the output that will be displayed to the user, and its status. Status can be any of the following: 

* `passed`: test or query succeeded
* `failed`: test was propertly executed, but there are broken tests. 
* `errored`: test or query finished with and error - for example a compilation error or an unhandled exeception
* `aborted`: test or query was cancelled by the runner because of a memory, time or cpu limits reached. `mumukit` will in fact try to detect those conditions and stop the docker container. 

For example, the following code removes verbose details from a ruby stack trace within a ruby a `query_runner`: 

```ruby
class RubyQueryHook < Mumukit::Templates::FileHook
  ...
  def post_process_file(_file, result, status)
    if status == :failed && error_output?(result)
      [sanitize_error_output(result), status]
    else
      super
    end
  end


  def error_output?(result)
    error_regexp =~ result
  end

  def sanitize_error_output(result)
    result.gsub(error_regexp, '').strip
  end

  def error_regexp
    # Matches lines like:
    # * from /tmp/mumuki.compile20170404-3221-1db8ntk.rb:17:in `<main>'
    # * /tmp/mumuki.compile20170404-3221-1db8ntk.rb:17:in `respond_to?':
    /(from )?(.)+\.rb:(\d)+:in `([\w|<|>|?|!|+|*|-|\/|=]+)'(:)?/
  end
end
```