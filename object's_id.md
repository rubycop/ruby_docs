#### Interest
```ruby
irb(main):024:0> nil.object_id
=> 8
irb(main):024:0> {"a"=>0}["a"].object_id
=> 1
irb(main):025:0> {"a"=>1}["a"].object_id
=> 3
irb(main):026:0> {"a"=>2}["a"].object_id
=> 5
irb(main):006:0> Class.object_id == Any_Class.class.class.class. ... .object_id
=> true
irb(main):025:0> true.object_id
=> 20
irb(main):025:0> false.object_id
=> 0
```
#### ! do it work at the end
```ruby
irb(main):016:0> false.object_id
=> 0
irb(main):017:0> !Object
=> false
irb(main):017:0> !Object.object_id
=> false
# but
irb(main):017:0> (!Object).object_id
=> 0
```
