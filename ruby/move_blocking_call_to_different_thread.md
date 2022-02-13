# Move blocking call to new thread

```ruby
def initialize
  @clients = []
  @data_store = {}

  server = TCPServer.new 2000
  puts "Server started at: #{ Time.now }"
  Thread.new do
    loop do
      new_client = server.accept
      @clients << new_client
    end
  end

  loop do
    @clients.each do |client|
      begin
        client_command_with_args = client.gets
        if client_command_with_args.nil?
          @clients.delete(client)
        elsif client_command_with_args.strip.empty?
          puts "Empty request received from #{ client }"
        else
          response = handle_client_command(client_command_with_args)
          client.puts response
        end
      rescue Errno::ECONNRESET
        @clients.delete(client)
      end
    end
  end
end
```
As soon as the server starts, we create a new thread, which does only one thing, accept new clients.

This second thread starts an infinite loop, inside the loop we call accept, and block until it returns a new client.

When we do receive a new client, we add it to the `@clients` instance variable, so that it can be used from the main thread, in the main loop.

By moving the blocking call to accept to a different thread, we’re not blocking the main loop with the accept call anymore.
