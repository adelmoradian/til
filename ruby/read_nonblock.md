# IO.read_nonblock

IO.read reads bytes from a stream and is blocking.
IO.read_nonblock is the non blocking alternative.

A key difference is that it requires an int argument to set the maximum number of bytes that will be read from the socket. For reasons that I can’t explain, it seems to be common practice to set it as a power of two. We could set it to a very low value, like 4, but then we wouldn’t be able to read a whole SET command in one call. SET 1 2 is seven bytes long. We could also set it to a very high value, like 4,294,967,296 (2^32), but then we would expose ourselves to instantiating a String of up to that length if a client decided to send one that large.

The default behavior of read_nonblock is to throw different exceptions when encountering eof and when nothing can be read, the exception: false argument allows us to instead only rely on the return value:

- If the value is nil, we reached EOF. It would have raised EOFError without the exception: false argument
- If the value is the symbol :wait_readable, there is nothing to read at the moment. It would have raised IO::WaitReadable without the exception: false argument.
- Otherwise, it returns up to 256 bytes read from the socket

```ruby
def initialize
  # ...

  loop do
    @clients.each do |client|
      client_command_with_args = client.read_nonblock(256, exception: false)
      if client_command_with_args.nil?
        @clients.delete(client)
      elsif client_command_with_args == :wait_readable
        # There's nothing to read from the client, we don't have to do anything
        next
      elsif client_command_with_args.strip.empty?
        puts "Empty request received from #{ client }"
      else
        response = handle_client_command(client_command_with_args.strip)
        client.puts response
      end
    end
  end
end
```
