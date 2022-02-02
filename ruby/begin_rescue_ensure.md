# Begin, rescue, ensure

```ruby
begin
  # something which might raise an exception
rescue SomeExceptionClass => some_variable
  # code that deals with some exception
rescue SomeOtherException => some_other_variable
  # code that deals with some other exception
else
  # code that runs only if *no* exception was raised
ensure
  # ensure that this code always runs, no matter what
  # does not change the final value of the block
end

begin
  IO.sysopen('/dev/null')
rescue Errno::ENOENT, Errno::EACCES
  puts "There was an error opening the file."
end

begin
  IO.sysopen('/dev/null')
rescue Errno::ENOENT
  puts "File not found."
rescue Errno::EACCES
  puts "Insufficient permissions, not allowed to open file."
end
```

Don't need to use the `begin` keyword if in a method or class or module:

```ruby
def get_null_device
  IO.sysopen('/dev/null')
rescue Errno::ENOENT
  puts "Can't open IO device."
end
```
