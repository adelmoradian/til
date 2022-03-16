# Exit program

While exit codes are great for machines, we humans often prefer a little bit of explanatory text. Fortunately, the OS provides us with an output stream specifically for things like error messages. Yep, I'm talking about STDERR.

You can write to STDERR just like you write to any IO object. In the example below I'm writing an error message and exiting with an "error" status.

```ruby
STDERR.puts("ABORTED! You forgot to BAR the BAZ")
exit(false)
```

This being Ruby, there is of course a more concise way to write to stdout and exit with an error code. Just use the abort   method.

```ruby
# Write the message to STDERR and exit with an error status code.
abort("ABORTED! You forgot to BAR the BAZ")
```

More info at [here](https://www.honeybadger.io/blog/how-to-exit-a-ruby-program/)
