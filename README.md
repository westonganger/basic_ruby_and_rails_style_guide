# Basic Ruby/Rails Code Style Guide

Helpful tips for clean and consistent code. This list is intended to be brief and non-comprehensive list.

### General

- Use 2-space indentation for everything. Ruby, JS, CSS


### Rails

Always use null: true and rely on model validations. Its hard to deal with null db errors nicely in form validations.

```ruby
# Bad
add_column :my_table, :col, :string, null: true

# Good
add_column :my_table, :col, :string, null: false

validates :col, presence: true ### in model
```


Never use default_scope, later it will become a MAJOR problem

```ruby
### Bad
default_scope ->(){ where(visible: true) }

### Good
scope :visible, ->(){ where(visible: true) }
```


### Ruby

Freeze all constants whenever to avoid accidentally changing there value instead of accessing it.

```ruby
### Bad
MY_CONSTANT = ["str"]

### Good
MY_CONSTANT = ["str"].freeze
```

Prefer `if` over `unless`, it is easier to congnitively understand the code

```ruby
### Bad
unless str.include?("asd")

### Good
if str.exclude?("asd")

if !str.include?("asd")
```

Use string interpolation syntax `#{}` instead of `+` for string concatention

```ruby
### Bad
str = name + ' ' + other

### Good
str = "#{name} #{other}"
```

Utilize `.presence` method whenever possible

```ruby
### Bad
str = name || item.association.try!(:name)

str = name.present? ? name : item.association.try!(:name)

### Good
str = name.presence || item.association.try!(:name)
```

Avoid using duck typing for methods, parenthesis help make the code more clear. The exception here is within view template files where is common place to exclude the parenthesis

```ruby
### Good
do_something(@item)

### Bad
do_something @item
````

Avoid Inline If/Unless Statements to be more readable, native languages usually do not read from Right to Left

```ruby
### Good
if val.present?
  val.to_a
end

### Bad
val.to_a if val.present?
```

Prefer Keyword over Hash Rocket syntax when key is non-quoted

```ruby
### Good
{foo: 'bar'}

### Bad
{'foo' => 'bar'}
```

Prefer Hash Rocket over Keyword syntax when key is quoted

```ruby
### Good
{'foo-bar' => 'bar'}

### Bad
{'foo-bar': 'bar'}
```

Inline rescue - Avoid rescue which is slow and instead use a simple `try!` or `try` call.

```ruby
### Bad
item.fishery.name rescue 'N/A'

### Good
item.fishery.try!(:name) || "N/A"
```

Prefer `try!` over `&.` because its safer in that checks that you actually have the method defined and its not just mistyped

```ruby
### Bad
item.fishery.&name

### Good
item.fishery.try!(:name)
```

Prefer named arguments over positional arguments when there is more than 1 arguments - this helps make the code more explicit

```ruby
### Bad
def do_something(user, account, project, val=nil)

### Good
def do_something(user:, account:, project:, val: nil)
```


### ERB

Single Line ERB - Seperate start and end tags with a space

```
<%= options %> ### Good

<%=options%> ### Bad
```

Multi Line ERB - Seperate the start and end tags from the Ruby code

```erb
### Good
<%=
  Fishery
    .where(...)
    .includes(...)
%>

### Bad
<% Fishery
     .where(...)
     .includes(...) %>
```

### Javascript

Place Function start bracket on same line as function

```javscript
// Good
function doSomething(){
  <code here>

// Bad
function doSomething()
{
  <code here>
```
