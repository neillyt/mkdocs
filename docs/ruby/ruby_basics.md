RUBY NOTES

### Mathematical operators
`8 * 8`  
`5 + 5`  
`5 - 5`  

### Converting Data Types

`to_s` = Convert to string  
`to_i` = Convert to integer  
`to_a` = Convert to array  

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

### Printing
`print derp` = Print the contents of the variable derp  
`print "hi","tom","guy"` = Print multiple strings  

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


### File
> File is used for working with files - writing and reading files.

`print File.read("/comics.txt")` = Print the contents of the /comics.txt file  
`FileUtils.cp('/comics.txt', '/Home/comics.txt')` = Copy method to copy a file  
`File.open("/Home/comics.txt", "a") do |f|` = Open a file in 'append' mode, "end" to end  
`File.mtime("/Home/comics.txt")` = Get last modified time of file  
`File.mtime("/Home/comics.txt").hour`	= Get last modified hour of file  


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
