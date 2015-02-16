#### Argument Array
Collapse a list of arguments into an Array.
```ruby
def my_meth(*args)
  args
end

my_meth('abc', {a: 1}, 0, nil) #=> ["abc", {:a=>1}, 0, nil]
```
#### Arround Alias
Call the previous, aliased version of a method from a redefined method.
```ruby
class String
  alias :old_reverse, :reverse
  
  def reverse
    "old-#{old_reverse}" # in this case avoids looping
  end
end

"abc".reverse #=> "old-abc"
```
#### Blank State
Remove methods from an object to turn them into *Ghost Methods*.

When method undefined for current object and we call it - the ```mehod_missing()```
method сatch it. All inside in ```mehod_missing()``` is a **Ghost Method**.
```ruby
class C
  undef_method :to_s
  
  def method_missing(name, *args, &block)
    "A Ghost Method"
  end
end

c = C.new #=> #<C:0x00000000aac260> 
c.to_s    #=> "A Ghost Method"
```
#### Class Extension
Define class methods by mixing a module into a class's eigenclass
to special case of *Object Extention*.
```ruby
class C; end

module M
  def my_meth
    "A Class Method"
  end
end

class << C; include M; end

C.my_meth    #=> "A Class Method"
```
In the other side we can call ```extend``` method and define simple class.
```ruby
class D; extend M; end

D.my_meth    #=> "A Class Method"
```
NOTE:
* *extend* - is a class method
* *include* - is a instance method

#### Class Extention Mixin
Enable a module to exten its included through a *Hook Method*.
```ruby
module M
  def self.included(base)
    base.extend(ClassMethods)
  end
  
  module ClassMethods
    def my_meth
      "a class method"
    end
  end
end
  
class C; include M; end

C.my_meth #=> "a class method"
```
