# Subprocess

- Run something but don't need it's output

```ruby
dir = './some/location'
system('ls', '-ali', dir) or raise "Failed to ls #{dir}"
```

If you want to run command without args do `system(["ls", "ls"])`
Otherwise `system` will take the single string as shell string.

- Run something, capture stdout as string and inherit stderr

```ruby
require 'open3'
stdout, status = Open3.capture2('ls', './somedir')
raise "Failed" unless status.success?
```

- Capture stdout as a stream

```ruby
Open3.popen2('unzip', '-l', zipfile) do |stdin, stdout, status_thread|
 stdout.each_line do |line|
   puts "LINE: #{line}"
 end
 raise "Unzip failed"  unless status_thread.value.success?
end
```

Read more [here](https://medium.com/zendesk-engineering/running-a-child-process-in-ruby-properly-febd0a2b6ec8)
