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

#### Lazy Instance Variable
Wait until the first access to initialize ana instance variable.
```ruby
module A
  def attr
    @var = @var || "some val"
  end
end

C.new.attr #=> "some val"
```

#### Mimic Method
Disguise a method as another language construct.
```ruby
def A(obj)
  obj == "string" ? String : Object
end

class B < A("string") # a method that looks like a class
  attr_accessor :attr # a method that looks like a keyword
end

C.new.attr = 1 # a method that looks like an attribute
```

#### Monkeypatch
Change thr features of an existing class.
```ruby
"abc".reverse #=> "cba"

class String
  def reverse; "some val"; end
end

"abc".reverse #=> "some val"
```

#### Named Arguments (Ruby 2.0)
```ruby
def meth(val1: "val1") 
  p val1 
end 
  
meth(val1: "new val1") #=> "new val1"
meth("new val1") #=> wrong number of arguments (1 for 0)
```

#### Namespace
```ruby
module M
  class Array
    def to_s; "some val"; end
  end
end

Array.new.to_s    #=> "[]"
M::Array.new.to_s #=> "some val"
```

#### Nil Guard
```ruby
x = nil
y = x || "some val" #=> "some val"
```

#### Object Extension
Define Singleton methods by mixeng a module into an object's eigenclass.
```ruby
obj = Object.new

module M
  def meth; "singleton meth"; end
end

class << obj; include M; end

obj.meth #=> "singleton meth"
```

#### Open Class
Modify an existing class.
```ruby
class String
  def meth; "some val"; end
end

"abc".meth #=> "some val"
```

#### Pattern Dispatch
```ruby
class C
  def meth1; "meth1"; end
  def meth2; "meth2"; end
end

obj = C.new
obj.methods.each do |m|
  obj.send(m) if m.to_s =~ /meth/
end
```

#### Sandbox
Execute intrusted code in a safe environment.
```ruby
def sandbox(&code)
  proc {
    $SAFE = 2
    yield
  }.call
end

begin
  sandbox { File.delete 'file' }
rescue Exception => ex
  ex #=> #<SecurityError: Insecure operation `delete' at level 2>
end
```

#### Scope Gate
```ruby
a = 1
defined? a #=> "local-variable"

module M
  b = 1
  defined? a #=> nil
  defined? b #=> "local-variable"
end

defined? b #=> nil
```

#### Self Yield
```ruby
class A
  attr_accessor :attr1, :attr2
  
  def initialize; yield self; end
end

A.new {|a| a.attr1; a.attr2;}
```

#### Shared Scope
Share variables among multiply contexts in the same *Flat Scope*
```ruby
lambda {
  var = 10
  self.class.class_eval do
    define_method :counter; var; end
    define_method :down; var -= 1; end
  end
}

counter #=> 10
down    #=> 9
```

#### Singleton Method
```ruby
obj = "abc"

class << obj
  def meth; "bca"; end
end

obj.meth #=> "bca"
```

#### String Of Code
Evaluate a string of ruby code
```ruby
code = "1 + 1"
eval(code) #=> 2
```

#### Symbol To Proc
```ruby
[1, 2].map(&:to_s) #=> ["1", "2"]
```
