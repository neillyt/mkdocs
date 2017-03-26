# Ruby Methods

### Optional Arguments
> For a method that requires multiple parameters, it is possible to assign _nil_ as the parameters. However, it is also possible to assign the parameters values on the fly.


```
def yell(words, sentences, paragraphs)
  # do stuff
end
```

The *yell* method can be called by supplying nil for the parameters.  
`def(words, sentences=nil, paragraphs=nil)`

The *yell* method can be called by assigning values to the parameters inside the method call.  
`def(words="hi there", sentences="This is a sentence.", paragraph="Okay.\n")`


### Options hash
> Ideally, we can use an *option hash* to eliminate the need to remember placeholders and without using super long method calls with a lot of parameters. The order of the keys does not matter and they can be left out if needed.  

```
def yell(message, options={})
  words = options[:words]
  sentences = options[:sentences]
  paragraphs = options[:paragraphs]
end
```

To call this function using the options hash:  
`yell("Message!", :words = "here are some words", :sentences = "This is a complete sentence.", :paragraphs = "This is a short paragraph.\n")`

This can also be called using a JSON like syntax:  
`yell("Message!", words:"here are some words", sentences : "This is a complete sentence.", paragraphs : "This is a short paragraph.\n")`  

Another example of using the options hash:  
```
def new_game(name, options={})
  {
    name: name,
    year: options[:year],
    system: options[:system]
  }
end
```

`game = new_game("Street Figher II", system:"SNES", year:1992)`

### Exceptions
> Exceptions allow us to gracefully fail out of a method.

The following method intentionally raises an exception if the unless condition is not satisfied. *begin* starts by calling the _get\_tweets_ method. *rescue* catches the exception and issues a warning. This is no different from try/catch or try/except in Python.  

```
def get_tweets(list)
  unless list.authorized(@user?)
    raise AuthorizationException.new
  end
  list.tweets
end

begin
  tweets = get_tweets(my_list)
rescue AuthorizationException
  warn "You are not authorized to access this list."
end
```

Another example:  

```
class InvalidGameError < StandardError; end
def new_game(name, options={})
  {
    name: name,
    year: options[:year],
    system: options[:system]
  }
end

begin
  game = new_game(nil)
rescue InvalidGameError => e
  puts "There was a problem creating your new game: #{e.message}"
end
```


### Splat Arguments
> Splay arguments are an expandable array or parameters.

In the following method, notice the *\*names* parameter - this is a splat argument.

```
def mention(status, *names)
  print("#{names.join(' ')} #{status}")
end

mention("A","B","C")
```

Another simple splat example using a for loop:

```
def describe_favorites(*games)
  for game in games
    puts "Favorite Game: #{game}"
  end  
end
describe_favorites('Mario', 'Contra', 'Metroid')
```
