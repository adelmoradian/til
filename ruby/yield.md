# Yield

Yield takes a block and executes it. 

Yield command pauses executing the code in the method, and instead passes control back to the block of code that called it, executes that code, and then continues executing the rest of the method after that.

```ruby
def Hello
  puts "hello"
  yield
  puts "world"

hello do
  puts "there"
end

# output
  # hello
  # there
  # world
```

Another example

```ruby
class MyClass
  attr_accessor :items

  def initialize(ary=[])
    @items = ary
  end

  def yield_method
    @items.each do |item| 
      yield item
    end
  end
end

my_class = MyClass.new(%w[a b c d])
my_class.yield_method do |y|
  puts y
end
```
yield item in the MyClass.yield_method method passes item to the block attached to my_class.yield_method. The item being yielded is assigned to the local y.
