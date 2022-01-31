# Conditional value return

Ruby conditionals return values. That is, a Ruby conditional such as if or case is an expression that returns a value.

This

```ruby
if some_condition
  q = an_expression_that_returns_a_value
elsif some_other_condition
  q = another_expression_that_returns_a_value
else
  q = yet_another_expression_that_returns_a_value
end
# => 0
q
# => 0
```

Can be written as:

```ruby
q = if some_condition
      an_expression_that_returns_a_value
    elsif some_other_condition
      another_expression_that_returns_a_value
    else
      yet_another_expression_that_returns_a_value
    end
# => 0
q
# => 0
```

Or this

```ruby
case
  when some_condition
    q = an_expression_that_returns_a_value
  when some_other_condition
    q = another_expression_that_returns_a_value
  else
    q = yet_another_expression_that_returns_a_value
  end
# => 0
q
# => 0
```

Can be written as

```ruby
q = case
    when some_condition
      an_expression_that_returns_a_value
    when some_other_condition
      another_expression_that_returns_a_value
    else
      yet_another_expression_that_returns_a_value
    end
# => 0
q
# => 0
```
