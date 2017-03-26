# Classes

The following is a class called Name. The first method in this class, *initialize* requires one argument and has an additional optional argument. The second method, *format* uses the variables initialized from the *initialize* method.  

The *initialize* method is a special method that is executed every time the class is loaded. This is a way to establish default values for our other class methods to use later. The initialize method is called when we create a new instance of the class. This is also when the arguments (first and last) are given their initial values if provided.  

Notice the *format* method below is created without the possibility of passing any arguments to it. Unlike many other languages, it is not necessary to follow it with a set of blank parentheses. Example of another language where no arguments would be supplied might look like this: _def format()_ compared to Ruby's _def format_.  

Compact takes out any nil values in an array. Join combines the strings.

```
class Name
  def initialize(first, last = nil)
    @first = first
    @last = last
  end
  def format
    [@last, @first].compact.join(', ')
    end
end
```

Use the Name class. *<<* is adding objects to the array. The *each* method is similar to for as it loops through each item in the array. The |n| is equivalent to *i* in a for loop (for i in user_names).

```
user_names = []
user_names << Name.new('Tom','Neilly')
user_names << Name.new('Joe', 'Shmo')
user_names << Name.new('Larry', 'Derpo')
user_names << Name.new('Felton')
user_names.each { |n| puts n.format }
```


Try another class. In the following class, *tweet*, there are two new modules: *attr_accessor* and *attr_reader*.  

attr_reader - This allows the *created_at* variable to be only read by the method using it. This prevents it from being changed.  
attr_writer - This allows a variable to be written to but not read by the method using it.  
attr_accessor - This allows the *status* variable to be read and written by the method using it.  

```
class tweet
  attr_accessor :status
  attr_reader :created_at
  def initialize(status)
    @status = status
    @created_at = Time.new
  end
  def to_s
    "#{@status}\n#{@created_at}"
  end
end
```

Use the tweet class.  
```
tweet = Tweet.new("Eating Breakfast.")
puts tweet.to_s
```


Another class!

```
class UserList
  attr_accessor :name
  def initialize(name)
    self.name = name
  end
end
```

```
list = UserList.new('celebrities')
list.name
```
