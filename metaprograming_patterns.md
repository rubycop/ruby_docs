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

#### Class Instance Variable
Store class-level state in a instance variable of the Class object.
```ruby
class C
  @inst_var = "some value"
  
  def self.class_attr
    @inst_var
  end
end

C.class_attr #=> "some value"
```

#### Class Macro
Use a class method in a class definition.
```ruby
class C; end

class << C
  def my_macro(arg)
    "my macro with #{arg} called"
  end
end

class C
  my_macro :x #=> "my macro with x called"
end
```

#### Clean Room
Use an object as an environment in which to evaluate a block. It is powerfull for dynamic call.
```ruby
class C
  def my_meth(x); x * 2; end
end

C.new.instance_eval { my_meth(2) } #=> 4
```

#### Code Processor
Process string of code from external source.
```ruby
File.readlines("ruby_file.txt").each do |line|
  "#{line.chomp} => #{eval(line)}"
end

# >> 1+1 => 2
# >> Math.log10(100) => 2.0
```

#### Context Probe
Execute a block to access information in an object's context.
```ruby
class C
  def initialize
    @inst_var = "private instance variable"
  end
end

C.mew.instance_eval { @inst_var } #=> "private instance variable"
```

#### Deferred Evaluation
Store a piece of code and its context in a proc or lambda for evaluation later
```ruby
class C
  def store(&block)
    @capsule_code = block
  end
  
  def execute
    @capsule_code.call
  end
end

c = C.new
c.store { $X = 1 }
$X = 2
c.execute # now $X = 1 
```

#### Dynamic Dispatch
Decide which method to call at runtime.
```ruby
method_to_call = :reverse
str = "abc"
str.send(method_to_call) #=> "cba"
```

#### Dynamic Method
Decide how define a method at runtime.
```ruby
class C; end

C.class_eval do
  define_method :my_meth do |args|
    "dynamic method with #{args}"
  end
end

C.new.my_meth(1) #=> "dynamic method with 1"
```

#### Dynamic Proxy
Forward to another object any message that don't match a method.
```ruby
class C;
  def initialize(target)
    @target = target
  end

  def method_missing(name, *args, &block)
    "result: #{@target.send(name, *args, &block)}"
  end
end

obj = C.new("abc")
obj.reverse #=> "result: cba "
```

#### Flat Scope
Use a closure to share variables between two scopes
```ruby
class C;
  def an_attr
    @attr
  end
end

obj = C.new
var = 100

# flat scope
obj.instance_eval { @attr = var }
obj.an_attr #=> 100
```

#### Ghost Method
Respond a message that doesn't have an assotiated methods.
```ruby
class C;
  def method_missing(name, *args, &block)
    "#{name}"
  end
end

C.new.reverse #=> "reverse"
```

#### Hook Method
Override a method to intercept object moedl events.
```ruby
$INHERITORS = []

class C;
  def self.inherited(subclass)
    $INHERITORS << subclass
  end
end

class D << C; end
class E << D; end
class F << C; end

$INHERITORS #=> "[D, E, F]"
```

#### Kernel Method
Define a method in module *Kernel*, that is available to all objects.
```ruby
module Kernel
  def my_kernel_meth
    "my Kernel method"
  end
end

my_kernel_meth #=> "my Kernel method"
```
