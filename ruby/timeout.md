# Timeout

Timeout provides a way to auto-terminate a potentially long-running operation if it hasn't finished in a fixed amount of time.

```ruby
require 'timeout'
status = Timeout::timeout(5) {
  # Something that should be interrupted if it takes more than 5 seconds...
}
```
