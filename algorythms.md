# **Algorythms**
*with* ***Ruby*** *implementation*
## 1. Dynamic Connectivity
Consist of:

1) **Quick Find**
2) **Quick Union**

> Array that will use for examples `a = [0, 1, 2, 3, 4]`

![enter image description here](https://lh3.googleusercontent.com/0EGiNgJC1O64nWy9aWHa87CfU_0Rtsw8Rg7VaW6pTOs=s350)
### Quick Find
Union implemented by redefined each el to `id[b]` if each el is equal `id[a]`.

*For example:*
```ruby
union(1, 0)  # i = 0; id[0] != id[1];
             # i = 1; id[1] == id[1]; id[1] = id[0];
             #=> a = [0, 0, 2, 3, 4]
```
![enter image description here](https://lh3.googleusercontent.com/qsI5MMW2vV3xq8KGmrOzcXaLIym1DOZqPnG-FH4WutM=s250)
```ruby
union(2, 1)  #=> a = [0, 0, 0, 3, 4]
```
![enter image description here](https://lh3.googleusercontent.com/Rw2nEGbhJ4g3_enjbKxogdqZgrLB3oY9S5_-79gPiFw=s250)
```ruby
# Quick Find
class QF
  attr_accessor :id
  
  def initialize(size)
    @id = []
    size.times {|i| @id << i}
  end
  
  def is_conn?(a, b)
    @id[a] == @id[b]
  end
  
  def union(a, b)
    return if is_conn?(a, b)
    
    @id.each do |i|
      @id[i] = @id[b] if @id[i] == @id[a]
    end
  end
end
```
### Quick Union
Union implemented by finding a root of `a` and `b` (find all el that has differents between its value and its array's position) and then redefined `id[root(a)]` to `root(b)`.

*For example:*
```ruby
union(1, 0)  # root(1) = 1; root(0) = 0;
             # id[1] = 0
             #=> a = [0, 0, 2, 3, 4]
```
![enter image description here](https://lh3.googleusercontent.com/qsI5MMW2vV3xq8KGmrOzcXaLIym1DOZqPnG-FH4WutM=s250)
```ruby
union(2, 1)  # id[2] = 0
             #=> a = [0, 0, 0, 3, 4]
```
![enter image description here](https://lh3.googleusercontent.com/OQeFnYhNK8vYCm6foA-pfrI5sUHLTv6xE-JTemcS1w0=s250)
```ruby
# Quick Union
class QU < QF
  def root(i)
    while(i != @id[i]) do
      i = @id[i]
    end

    i
  end

  def union(a, b)
    root_a = root(a)
    root_b = root(b)
    return @id if is_conn?(root_a, root_b)
  
    @id[root_a] = root_b
    @id
  end
end
```
### Weighted Quick-union
Union implemented by putting smaller tree to bigger tree. If `a` is in small tree - put it tree to `b` otherwise - `b` to `a`.

*Example:*
```ruby
union(1,0) #=> [1, 1, 2, 3, 4, 5, 6]
union(2,0) #=> [1, 2, 2, 3, 4, 5, 6]
union(0,3) #=> [2, 2, 2, 2, 4, 5, 6]
union(4,5) #=> [2, 2, 2, 2, 5, 5, 6]
union(6,5) #=> [2, 2, 2, 2, 5, 5, 5]
```
We can improve our algorythm using ***path compression*** that meens: After finding root - set a point of each nodes to this root.

*For example:*
We have a tree

![enter image description here](https://lh3.googleusercontent.com/8DTvos-XdPwyTAPHj8HSYogJZB6OHxV6kroGv4a0z90=s300)

after `union(4,3)`

![enter image description here](https://lh3.googleusercontent.com/BY70lA5lMJcxg7HJPpVH1_xkX-tNgDmPp6Bwa2OX7ic=s300)

Check the root: `root(4) #=> 3`
after `union(6,4)`changed on

![enter image description here](https://lh3.googleusercontent.com/dmZ2kRXQ3gp3UCLIAPJ1d7MTP7j3MDMbL0IY_BKpb5c=s300)

Check the root: `root(6) #=> 3`.

```ruby
# Weighted Quick Union
class WQU < QU
  attr_accessor :rs # id's root size
  
  def initialize(size)
	  super(size)
	  @rs = []
    size.times {|i| @rs << i}
  end
  
  def root(i)
    while(i != @id[i]) do
      @id[i] = @id[@id[i]]
      i = @id[i]
    end
 
    i
  end

  def union(a, b)
    a = root(a)
    b = root(b)

    return @id if a == b
    
    if @rs[a] < @rs[b]
      @id[a] = b
      @rs[b] += @rs[a]
    else
      @id[b] = a
      @rs[a] += @rs[b]
    end

    @id
  end
end
```
