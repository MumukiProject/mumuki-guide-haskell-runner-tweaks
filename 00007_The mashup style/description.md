By default, the `mashup` directives compiles the `POST /test` requests into a file that simply concatenates `extra`, `content`, `test`, in that order. 

However, you can alter the concatenation order by explicitly declaring it, e.g.:

```ruby
mashup :content, :extra, :test
```