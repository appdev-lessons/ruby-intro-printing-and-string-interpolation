# The many ways to print in Ruby

Ruby includes many different methods for printing out-of-the-box. In addition, members of the Ruby community have written even more printing methods and shared them via gems. Let's look at a few.

pp

### print

### Print (`print`)

Try the following:

```ruby
x = "howdy"
y = "world"

puts x
puts y

print x
print y
```
{: .repl #print_1 title="print_1" points=1}

Can you guess the difference between `puts` and `print`?

`print` is the simpler of the two — it prints what you give it, and that's it.

`puts` prints what you give it, but also adds a _new line_. We could achieve the same thing with `print` by adding the _newline character_ ourselves:

```ruby
x = "howdy\n"
y = "world\n"

print x
print y
```
{: .repl #print_1 title="print_1" points=1}

Notice the `\n` at the end of both strings. This is an example of an _escape sequence_, which are used to represent certain special characters that cannot be typed directly.

Escape sequences all start with a backslash (\), which is why they're called "escape" sequences - the backslash "escapes" the usual meaning of the character that follows, and together they represent a different character. Most commonly:

- `\n` represents a newline (since if we type <kbd>return</kbd> it adds a newline within our _code_, not to the `String`).
- `\\` represents a single backslash (since if we type a `\` by itself, it would be interpreted as the start of an escape sequence).
- `\"` represents a double quote (since if we typed a double quote directly, it would end the `String`).

So why would we ever use `print`? Imagine if you wanted to display a random motivational quote, but decorated on each side with `~~~`, like this:

```
~~~ One who chases two rabbits, catches neither. ~~~
```

We can achieve this with `print`:

```
quote = "One who chases two rabbits, catches neither."

print `~~~`
print quote
print `~~~`
```

TODO: this is a bad example, students will ask "Why not just add the 3 strings together before `puts`?"

---

puts

p

ap 
