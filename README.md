# Basic Ruby/Rails Code Style Guide

Helpful tips for clean and consistent code. This list is intended to be brief and non-comprehensive list.

### General

- Use 2-space indentation for everything. Ruby, JS, CSS


### Ruby

Prefer named arguments over positional arguments when there is more than 1 arguments - this helps make the API's for the entire codebase greatly simplified

```ruby
### Bad
def do_something(user, account, project, val=nil)

### Good
def do_something(user:, account:, project:, val: nil)
```



Avoid Inline If/Unless Statements to be more readable, native languages usually do not read from Right to Left

```ruby
### Bad
val.to_a if val.present?

### Good
if val.present?
  val.to_a
end
```

Prefer `if` over `unless`, it is easier to congnitively understand the code

```ruby
### Bad
unless str.include?("asd")

### Good
if str.exclude?("asd")

if !str.include?("asd")
```


Avoid using duck typing for methods, parenthesis help make the code more clear. The exception here is within view template files where is common place to exclude the parenthesis

```ruby
### Bad
do_something @item

### Good
do_something(@item)
````


Prefer Keyword over Hash Rocket syntax when key is non-quoted

```ruby
### Bad
{'foo' => 'bar'}

### Good
{foo: 'bar'}
```

Prefer Hash Rocket over Keyword syntax when key is quoted

```ruby
### Bad
{'foo-bar': 'bar'}

### Good
{'foo-bar' => 'bar'}
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
item.fishery.&namee ### never raises error

### Good
item.fishery.try!(:namee) ### raises error cause its actually wrong
```



Utilize `.presence` method whenever possible

```ruby
### Bad
str = name || item.association.try!(:name)

str = name.present? ? name : item.association.try!(:name)

### Good
str = name.presence || item.association.try!(:name)
```


Freeze all constants whenever to avoid accidentally changing there value instead of accessing it.

```ruby
### Bad
MY_CONSTANT = ["str"]

### Good
MY_CONSTANT = ["str"].freeze

HASH_CONSTANT = {
  foo: {
    ...
  }.freeze
  
  bar: {
    ...
  }.freeze
}.freeze
```


Use string interpolation syntax `#{}` instead of `+` for string concatention

```ruby
### Bad
str = name + ' ' + other

### Good
str = "#{name} #{other}"
```


Add at least 3 # sybols for developer comments and one # for code comments

```ruby
### Bad

# this is a comment
# str = "commented out code"

### Good

### THIS IS A COMMENT
# str = "commented out code"
```


### Rails


Never use default_scope, later it will become a MAJOR problem, the main reason is that it also affects associations.

```ruby
### Bad
default_scope ->(){ where(visible: true) }

### Good
scope :visible, ->(){ where(visible: true) }
```

Always use null: true and rely on model validations. Its hard to deal with null db errors nicely in form validations.

```ruby
# Bad
add_column :my_table, :col, :string, null: true

# Good
add_column :my_table, :col, :string, null: false

validates :col, presence: true ### in model
```


### ERB

Single Line ERB - Seperate start and end tags with a space

```er b
### Bad
<%=options%>

### Good
<%= options %>

```

Multi Line ERB - Seperate the start and end tags from the Ruby code

```erb
### Bad
<% Fishery
     .where(...)
     .includes(...) %>
     
### Good
<%=
  Fishery
    .where(...)
    .includes(...)
%>
```

### Javascript

Place Function start bracket on same line as function

```javscript
// Bad
function doSomething()
{
  <code here>
}
  
// Good
function doSomething(){
  <code here>
}
```
