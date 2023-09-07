# A bit more Ruby: String interpolation and printing

Before we leave our Intro to Ruby, there's a couple of other items related to `String`s and printing to be aware of.

## String interpolation, alternative to .to_s

Try this:

```ruby
number = 6 * 7
message = "Your lucky number for today is " + number + "."
pp message
```
{: .repl #string_addition_error title="String addition error" points="1"}

Ruby gets confused (RTEM!), because we are trying to add an `Integer` to a `String` and it doesn't feel comfortable with that.

The solution is to tell the `Integer` to convert itself to a `String` first using `.to_s`:

```ruby
number = 6 * 7
message = "Your lucky number for today is " + number.to_s + "."
pp message
```
{: .repl #string_addition_fixed title="String addition fixed" points="1"}

The above technique for composing strings, adding them together with `+`, is called string addition.

There's another technique for composing strings that I personally find a bit easier; it's called **string interpolation**. Try this instead:

```ruby
number = 6 * 7
message = "Your lucky number for today is #{number}."
pp message
```
{: .repl #string_interpolation title="String interpolation" points="1"}

Basically, inside the string, you place `#{}` where you eventually want your value to go. Inside the curly braces, you can write any Ruby expression without worrying about whether it is a string or not. The expression will be evaluated, converted to a string, and added to the string right in that spot. You can interpolate as many expressions as you want into a single string. Pretty neat!

```ruby
number = 6
another_number = 6.6
a_string = "my numbers are"
message = "today #{a_string}: #{number} and #{another_number}."
pp message
```
{: .repl #string_interpolation_again title="String interpolation, again" points="1"}

### Date format with string interpolation

[Revisit the `Date` lesson](https://learn.firstdraft.com/lessons/72-ruby-intro-date), and find the "Format the Date" graded code block. In the graded code block below, solve that test again, but this time using string interpolation. The tests will check that you did use interpolation, so don't include any `+` operations in your code this time.

As a reminder, you should write a program to identify different parts of today's date and print a `String` result. For any given date, the output should be:

"The year is: XXXX, the calendar day is: X, and the month is: X."

Use the _exact same copy_ ("The year is..."), but replace "X" with your calculated date. I included the `require "date"` this time.

When you think you've got it, there are two tests to check the result.

```ruby
require "date"
# write your program here
```
{: .repl #date_formatted_interp title="Format the Date with interpolation"}

```ruby
describe "Format the Date with interpolation" do
  it "outputs 'The year is: 2020, the calendar day is: 1, and the month is: 7.' when today is July 1, 2020" do
    # Set today's date to 2020-07-01
    allow(Date).to receive(:today).and_return Date.new(2020,07,01)
    path = "/tmp/code.rb"

    plus_counter = 0
    File.foreach(path) do |line|
      if line.include?("+") 
        plus_counter += 1
      end
    end
    expect(plus_counter).to be < 1,
      "Expected graded code block NOT to use the '+' method, but found at least one '+'."

    expect { require_relative(path) }.to output(/The year is: 2020, the calendar day is: 1, and the month is: 7./).to_stdout
  end
end
```
{: .repl-test #date_formatted_interp_test_1 for="date_formatted_interp" title="Format the Date with interpolation outputs 'The year is: 2020, the calendar day is: 1, and the month is: 7.' when today is July 1, 2020" points="1"}

```ruby
describe "Format the Date with interpolation" do
  it "outputs 'The year is: 2020, the calendar day is: 29, and the month is: 6.' when today is June 29, 2020" do
    # Set today's date to 2020-06-29
    allow(Date).to receive(:today).and_return Date.new(2020,06,29)
    path = "/tmp/code.rb"

    plus_counter = 0
    File.foreach(path) do |line|
      if line.include?("+") 
        plus_counter += 1
      end
    end
    expect(plus_counter).to be < 1,
      "Expected graded code block NOT to use the '+' method, but found at least one '+'."

    expect { require_relative(path) }.to output(/The year is: 2020, the calendar day is: 29, and the month is: 6./).to_stdout
  end
end
```
{: .repl-test #date_formatted_interp_test_2 for="date_formatted_interp" title="Format the Date with interpolation outputs 'The year is: 2020, the calendar day is: 29, and the month is: 6.' when today is June 29, 2020" points="1"}

## The many ways of printing

So far, we've been using Ruby's built-in `pp` (pretty print) method for all of our printing. But there are a few other printing methods that you will encounter, so let's chat about them:

### Put string (`puts`)

Try the following:

```ruby
x = "Howdy!"

pp x

puts x
```
{: .repl #puts_1 title="puts, 1" points="1"}

Can you spot the difference?

Try this one:

```ruby
x = [4, 8, 15, 16, 23, 42]

pp x

puts x
```
{: .repl #puts_2 title="puts, 2" points="1"}

This time it's easier to spot the difference.

The `pp` methods shows an object in all of its gory, technical detail; which is what we usually want, because we're usually printing in order to debug our code.

Behind the scenes, `pp` calls a method `.inspect` on whatever object you give it. `.inspect` can be called on any object, and returns a `String` with additional information about the object. Then, `pp` displays that string.

But sometimes you are writing a program for other people's consumption, and you don't want to show the double-quotes around strings, etc. You want the output to look nice and clean for the end user.

For those times, the `puts` method (pronounced "put string" or "put S") is your friend.

Behind the scenes, `puts` calls a method `.to_s` on whatever object you give it. `.to_s` can be called on any object, and returns a _user-friendly, formatted_ `String`  representing the object. Then, `puts` displays that string.

Either way, both `puts` and `pp` are doing some extra work on the object — `.to_s` and `.inspect`, respectively — to convert the object into a `String` that's ready for printing. `puts` uses `to_s` in order to get a string that's nicely formatted for an end user, and `pp` uses `.inspect` in order to get a string that contains the gory technical details for a developer.

Notice in this example:

```ruby
x = [4, 8, 15, 16, 23, 42]

puts x
```
{: .repl #puts_3 title="puts, 3" points="1"}

`puts` is doing a lot of work for us! It converts each element of the array into a `String`, then it prints each of those on its own line. But, just by looking the output of `puts`, a developer couldn't immediately tell where those five strings came from. Maybe they were each individual variables? Who knows?

With `pp`, it's much easier to understand what `x` actually _is_:

```ruby
x = [4, 8, 15, 16, 23, 42]

pp x
```
{: .repl #puts_4 title="puts, 4" points="1"}

Like I said, since I'm almost always printing in order to **make the invisible visible** to help myself debug, I almost always use `pp`. But sometimes `puts` comes in handy.

## Escape characters

When you come across `String`s in this class and online, you may begin to see frequent backslash `\` characters within them. A very common one is the so called "newline" character: `"\n"`.

Every character needs to be represented to properly render input and output to the computer. The combination `\n` represents a new paragraph or a line break. 

In other words, backslashes are **escape characters** that invoke _an alternative interpretation on the following character_ in a sequence.

So, in the case of our `\n` combination, we are saying: "this is not the letter n, this is a newline". 

We just learned about the `puts` method, which does some extra work for us. In fact, in the case of printing a string with a newline in it, `puts` actually formats the string and adds the newline:

```ruby
my_string = "RaghuBetina\nthe instructor"
puts my_string
```
{: .repl #puts_newline title="puts newline" points="1"}

Now try that with `pp`:

```ruby
my_string = "RaghuBetina\nthe instructor"
pp my_string
```
{: .repl #pp_newline title="pp newline" points="1"}

Interesting, `pp` is telling us (in all of the gory, technical detail) that the string is made up of two strings added together with a line break after the first string. Much more information than `puts` was giving us by formatting the string.

### Quotes within quotes

Escape characters can be used anywhere to invoke an alternative interpretation of a character that follows it. Sometimes, that leads to new functionality, like the `\n` newline sequence, or `\s` which adds a space:

```ruby
my_string = "Raghu\sBetina\nthe instructor"
puts my_string
```
{: .repl #puts_space title="puts space" points="1"}

Other times, the escape character indicates that the character following it should behave _as it normally would in a context that might otherwise prevent it_. 

The easiest way to demonstrate what we mean is with quotes within quotes.

Try this:

```ruby
my_string = ""RaghuBetina""
pp my_string
```
{: .repl #no_backslash_quote title="no backslash on quotes" points="1"}

Error message! What happened here?

We tried to create a string in `my_string` that was two double quotes (i.e. a self-contained empty string), followed by an undefined variable (`RaghuBetina` is outside of the string), followed by another self-contained empty string. Not gonna work!

We could resolve this by using single quotes, which is one way to include a string within a string:

```ruby
my_string = "'RaghuBetina'"
pp my_string
```
{: .repl #no_backslash_single_quote title="single quotes in double quotes" points="1"}

But, if we want the double quotes, we will need to use the escape character backslash:

```ruby
my_string = "\"RaghuBetina\""
pp my_string
```
{: .repl #backslash_quote title="backslash on quotes" points="1"}

Nice! The `\` before the `"` says: treat this next quotation as a character, not as the `String` literal syntactic sugar.

In our example `pp` still prints the string in all of its gory, technical detail, but we can also use `puts` to see how the result would look formatted:

```ruby
# the below won't work, but feel free to try
# my_string = ""RaghuBetina""
# puts my_string

my_string = "\"RaghuBetina\""
puts my_string
```
{: .repl #puts_backslash_quote title="puts backslash quotes" points="1"}

### Multi-line strings

One more thing that you should see now, since it will come up again soon: multi-line strings with escape characters. 

Consider this: 

```ruby
ms = "
     Some string "with a quote in it"
     with some more text continued below
     "
puts ms
```
{: .repl #multiline_string_error title="Multi-line string error" points="1"}

You should have seen the now familiar error message.

We are trying to build a **multi-line string**, which is perfectly valid as long as there is one opening quote and one closing quote. 

How can we resolve this? How about:

```ruby
ms = "
     Some string \"with a quote in it\"
     with some more text continued below
     "
puts ms
```
{: .repl #multiline_string_fixed title="Multi-line string fixed" points="1"}

Nice! The `\` escape character is particularly important in situations like this to place strings within strings on multiple lines.
