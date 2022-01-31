# Start program one liner

Temporary workaround when we don't have an easy way of starting a program.

```bash
ruby -r "./server.rb" -e "BasicServer.new"
```

The command means:

- run ruby process
- require `server.rb` file located in the same directory
- execute the command `BasicServer.new`
