#### Assing hash for instance variable thought the method w/o params
```ruby
class A
  def hash_store
    @hash ||= {}
  end
end

> obj = A.new                    #=> <A:0x00>
> obj.hash_store                 #=> {}
> obj.hash_store[:key1] = "val1" #=> "val1"
> obj                            #=> #<A:0x00 @hash={:key1=>"val1"}>
> obj.hash_store                 #=> {:key1=>"val1"}
```
#### Assign with params
```ruby
class A
  def hash_store(hash = {})
    @hash ||= {}
    @hash.merge!(hash)
  end
end

> obj = A.new                     #=> <A:0x00>
> obj.hash_store                  #=> {}
> obj.hash_store[:key1] = "val1"  #=> "val1"
> obj.hash_store({:key2=>"val2"}) #=> {:key1=>"val1", :key2=>"val2"}
> obj                             #=> #<A:0x00 @hash={:key1=>"val1", :key2=>"val2"}>
```
