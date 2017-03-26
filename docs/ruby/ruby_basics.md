RUBY NOTES

### Mathematical Operators
`+` = plus  
`-` = minus  
`/` = divide  
`*` = multiply  
`<` = less than  
`>` = greater than  
`%` = remainder

### Variables
> Assigning variables in Ruby is much like other languages.

`a = "This is a string."`
`b = 10`
`c = 20`

### Printing
> To print to the screen, use _puts_ or _print_. _puts_ vs _print_: _put_ adds a newline after executing, print does not.

`puts "Hello, screen!"` = Prints "Hello, screen!" to the screen  
`puts b + c` = Adds variables b and c together and prints on screen.  
`puts "hi" * 10` = Prints "hi" 10 times.  
`print derp` = Print the contents of the variable derp  
`print "hi","tom","guy"` = Print multiple strings  


### String Replacement
> Like the back ticks are used to execute a chunk of code, Ruby uses the hash symbol for this.

`#{}` = Sequence for executing a piece of code separately from the rest of the line.  

```
puts "The answer to 2 plus 2 is #{2 + 2}"
The answer to 2 plus 2 is 4
```

```
puts "A + B = #{b + c}"
A + B = 30
```

```
d = "There are #{b} people in the room."
"There are 10 people in the room."
```

### Formatting and Hash Mapping

```
formatter = "%{first} %{second} %{third} %{fourth}"

puts formatter % {first: 1, second: 2, third: 3, fourth: 4}
puts formatter % {first: "one", second: "two", third: "three", fourth: "fourth"}
puts formatter % {first: true, second: false, third: true, fourth: false}
puts formatter % {first: formatter, second: formatter, third: formatter, fourth: formatter}

puts formatter % {
  first: "I had this thing.",
  second: "That you could type up right.",
  third: "But it didn't sing.",
  fourth: "So I said goodnight."
}
```

### Special Characters

`\n` = newline  
`\` = escape character  
`\\` = backslack  
`\"` = double quote  
`\'` = single quote  
`\a` = bell  
`\b` = backspace  
`\f` = formfeed  
`\r` = carriage return   
`\t` = horizontal tab  
`\v` = vertical tab  
`\ooo` = character with octal value ooo  
`\xhh` = character with hex value hh  
`"""` = block quotes  

Example of a multiline string:  
```
puts ###
oh hi
there
tommyboy
###
```

Example of escaping double quotes:  
```
puts "Hello there \"Tommyboy\""
```

### Reading Input
> To get input from a user, use the _gets.chomp_ method. _gets_ is a function that gets string from the input. _chomp_ will strip newlines and carriage returns from the input.

##### String input

```
print "How old are you?"
age = gets.chomp
print "How tall are you?"
height = gets.chomp
print "How much do you weight?"
weight = gets.chomp

puts "So, you're #{age} old, #{height} tall, and #{weight} heavy."
```

##### Number input
> Getting a number is no different, but we must convert it to an integer to ensure it is a number.

```
print "Give me a number: "
number = gets.chomp.to_i

bigger = number * 100
puts "A bigger number is #{bigger}"

print "Give me another number: "
another = gets.chomp
number = another.to_i

smaller = number / 100
puts "A smaller number is #{smaller}."
```

### Passing Arguments to your Script
> Ruby uses a predefined list called _ARGV_ to store any arguments supplied when executing the script. Note that these are always input as strings and must be converted if necessary.


Passing multiple arguments at once:  
```
first, second, third = ARGV

puts "Your first variable is: #{first}"
puts "Your second variable is: #{second}"
puts "Your third variable is: #{third}"
```

You can also specify a specific argument (first arg, second arg, etc.):  
```
username = ARGV.first
```

### Converting Data Types

`to_s` = Convert to string  
`to_i` = Convert to integer  
`to_a` = Convert to array  
`to_f` = Convert to floating point


### Reading Files
> To read a file, we must use the _open_ and _read_ methods.

File methods for controlling a file:  
* `open` = opens a file so that it can be read, written, etc.  
  * `open("filename.txt","w")` = write to the file  
  * `open("filename.txt","r")` = read from the file  
  * `open("filename.txt","a")` = append to the file  
  * `open("filename.txt","r+")` = open and write to or read from the file  
  * `open("filename.txt","w+")` = create and write to or read from the file  
  * `open("filename.txt","a+")` = open or create to write to or read from the file  
* `close` = closes the file  
* `read` = reads the contents of a file (can assign the result to a var)  
* `readline` = read just one line from a text file  
* `truncate` = accepts an argument of an integer - the argument supplied is how many characters will remain after truncation (0 to remove all characters)  
* `write` = writes text to the file  

Here is an example of passing a file name as an argument, opening that file, and printing the contents of the file on the screen:  
```
filename = ARGV.first

txt = open(filename)

puts "Here's your file #{filename}:"
print txt.read

print "\nType the filename again:"
file_again = $stdin.gets.chomp

txt_again = open(file_again)

print txt_again.read
```

The _open_ method requires an argument - in this example we use the _w_ argument which opens the file in a manner that it can be written to.  The _truncate_ method also requires an argument. In this example we use the number 0
```
filename = ARGV.first

puts "We're going to erase #{filename}"
puts "If you don't want that, hit CTRL-C."
puts "If you do want that, hit RETURN."

$stdin.gets

puts "Opening the file..."
target = open(filename, 'w')
puts "Truncating the file. Goodbye!"
target.truncate(0)

puts "Now I'm going to ask you for three lines."
print "line 1:"
line1 = $stdin.gets.chomp
print "line 2:"
line2 = $stdin.gets.chomp
print "line 3:"
line3 = $stdin.gets.chomp

puts "I'm going to write these to the file."

target.write(line1)
target.write("\n")
target.write(line2)
target.write("\n")
target.write(line3)
target.write("\n")

puts "And finally, we close it."
target.close
```

##### The File class is used for working with files - writing and reading files.

`print File.read("/comics.txt")` = Print the contents of the /comics.txt file  
`FileUtils.cp('/comics.txt', '/Home/comics.txt')` = Copy method to copy a file  
`File.open("/Home/comics.txt", "a") do |f|` = Open a file in 'append' mode, "end" to end  
`File.mtime("/Home/comics.txt")` = Get last modified time of file  
`File.mtime("/Home/comics.txt").hour`	= Get last modified hour of file  
`File.exist?("/Home/comics.txt")` = Checks if the file exists, returns True or False

The following is an example of checking if a file exists, getting the length (size) or a file, and overwriting a file. Note that `$stdin.get` simply pauses the script until some input is provided from the user:  
```
from_file,to_file = ARGV

puts "Copying from #{from_file} to #{to_file}"
in_file = open(from_file)
indata = in_file.read

puts "The input file is #{indata.length} bytes long"
puts "Does the output file exist? #{File.exist?(to_file)}"
puts "Ready hit RETURN to continue, CTRL-C to abort."
$stdin.gets
out_file = open(to_file,'w')
out_file.write(indata)

puts "Alright, all done."
out_file.close
in_file.close
```


### Common Methods

`.reverse` = Reverse a string  
`.length` = Get the length of a string  
`.max` = Get the highest number in an array  
`.sort` = Sort an array  
`.sort!` = Sort an array and keep the contents sorted  
`.lines` = Splits a large string into lines  
`.join` = Combines multiple strings into a single string  
`.downcase` = Makes all letters in a string lowercase  
`.include?` = True or False check to see if a substring is in a string  
`.times` = Do something x amount of times  
`Dir.entries` = List everything in a directory  

### Arrays
`[]` = Create an empty array  
`ticket = [12, 34, 57]` = Create an array called ticket  
`ticket` = Print the array  



### Square brackets
`poem["word"] = "new word"` = Replaces "word" with "new word"  

### Dictionary (hash)
`books = {}` = Defining a dictionary/hash  
`books["Gravity's Rainbow"] = :splendid` = Add an entry to the dictionary  
`books.keys` = Show all keys in the dictionary  
`books.values` = Show all values in the dictionary  
`Ratings = Hash.new(0)` = Build an empty hash  
`books.values.each { |rate| ratings[rate] += 1 }` = Turn all unique values in books into keys with the new Rating has.  

### Blocks of Ruby code are always attached to methods
`5.times { print "Odelay!" }`

### Dir
> Dir can search for files with wildcards.

`Dir.entries "/"` = List everything in the root directory  
`Dir[]` = Can search for files with wildcards  
`Dir["/*.txt"]` = Find files that end with ".txt"  




### Methods
> Use 'def' to create a new method.

The following code is an example of defining a method, code that can be reused later. Comics is an empty array. We call the Files class and the foreach loop on the path variable. For every file or directory in that path, add it to the comics array.  

```
path = "/home/comics.txt"

def load_­comics(path)
  comics = {}
  Files.foreach(path) do |line|
    name,url=line.split(': ')
    comics[name] = url.strip
  end
  comics
end
```

### Including other classes

`require 'popup'` = Imports the "popup" library  
`Popup.goto "http://bing.com"` = Launches a popup to bing.com  

Create your own popup  
```
Popup.make {
  h1 "My Links"
  link "Go to bing","http://bing.com"
}
```

### Conditional Statements
> Ruby has an alternative to "if not" statements called _unless_.

The following code is a typical if/else statement that checks if the tweets variable is empty, then instructs to print (puts) things accordingly.  

```
if tweets.empty?
  puts "No tweets found"
else
  puts "Timeline:"
  puts tweets
end
```

Using *unless*, we can simplify the last statement further. The following code says, "print on screen (puts) the word 'Timeline' and everything stored in the tweets variable if the tweets variable is not empty."  

```
unless tweets.empty?
  puts "Timeline"
  puts tweets
```

If the attachment has a file_path it will eval to true and post the attachment.  

```
if attachment.file_path
  attachment.post
end
```

### Inline if / unless

Here are some common ways to use inline *if* and *unless* statements. The following code says:  
* "Throw an error if password.length is less than 8."
* Throw an error if username is empty (unless will resolve to true or false)

```
fail "Password too short" if password.length < 8
fail "No user name set" unless username
```

Here we define an array called 'things' and use an inline 'unless' statement to reduce our code to a single line. This code says, "print on the screen (puts) the number of things in your attic (things.count) if things.empty is not empty."  

```
things = ["dust", "boxes", "insulation"]
puts "Things in your attic: {#things.count}" unless things.empty?
```

### and
> Use *&&* when you want to satisfy multiple conditions.

The following code checks that the user variable exists and user is signed in.  

```
if user && user.signed_in?
    print "signed in"
end
```

### or
> Ruby offers an interesting way to use "or"

If timeline.tweets is nil then tweets will assign itself to an empty array.  
`tweets = timeline.tweets || []`  

If current session evals to true, *end*.  
If current session evals to false, execute *sign_user_in*.

```
def sign_in
  current_session || sign_user_in
end
```

Set var to 1.  
`i_was_set = 1`

If *i_was_set* is set, do nothing.  
If *i_was_set* is not set, set to 2.   
`i_was_set ||= 2`

Save the variable.  
`puts i_was_set`


Two ways to check if options[:country] returns nil; if it is, assign it the value 'us'.  

```
options[:country] = 'us' if options[:country].nil?
options[:country] ||= 'us'				
```

### Conditional return values

```
if list_name
  options[:path] = "/#{user_name}/#{list_name}"
else
  options[:path] = "/#{user_name}"
end
```

```
options[:path] = if list_name
  "/#{user_name}/#{list_name}"
else
  "/#{user_name}"
end
```
