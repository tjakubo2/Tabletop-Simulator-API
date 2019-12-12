Vector is a class providing a set bla bla bla

20:00 - 20:53
20:54 - 21:30
14:00 - 15:10
20:00 - 

Example usage: `Wait.frames(functionName, 60)`

!!!tip
    Vector and Color are the first classes to be defined in pure Lua. This means you **have** to use colon operator (e.g. `pos:angle()`) to call member functions, not the dot operator. Failing to do so will fail with cryptic error messages displayed.

##Constructors summary

!!!tip
    Every place that returns a coordinate table, like `obj.getPosition()`, serves a Vector class instance already - you do not have to explicitly construct it.
    When constructing Vector instances, the `.new` part can be omitted, making e.g. `Vector(1, 2, 3)` equivalent to `Vector.new(1, 2, 3)`.

Function Name | Description | Return | &nbsp;
-- | -- | -- | --
new([<span class="tag flo"></span>](/types)&nbsp;x, [<span class="tag flo"></span>](/types)&nbsp;y, [<span class="tag flo"></span>](/types)&nbsp;z) | Constructs a Vector with given initial values. | [<span class="tag vec"></span>](/types#vector)
new([<span class="tag tab"></span>](/types)&nbsp;t) | Constructs a Vector that reads initial values from provided table (at `x`, `y`, `z` or `1`, `2`, `3` keys). | [<span class="tag vec"></span>](/types#vector)
between([<span class="tag vec"></span>](/types)&nbsp;one, [<span class="tag vec"></span>](/types)&nbsp;two) | Constructs a Vector that points from `one` coordinates to `two` coordinates | [<span class="tag vec"></span>](/types#vector)
max([<span class="tag vec"></span>](/types)&nbsp;one, [<span class="tag vec"></span>](/types)&nbsp;two) | Constructs a Vector that has higher values from `one` and `two` under each component. | [<span class="tag vec"></span>](/types#vector)
min([<span class="tag vec"></span>](/types)&nbsp;one, [<span class="tag vec"></span>](/types)&nbsp;two) | Constructs a Vector that has smaller values from `one` and `two` under each component. | [<span class="tag vec"></span>](/types#vector)

###Constructors examples


```lua
function onLoad()
    local vec1 = Vector.new(0.5, 1, 1.5)
    local vec2 = Vector(1, -1, 0) -- same as Vector.new(1, -1, 0)

    print(Vector.between(vec1, vec2)) --> Vector: {0.5, -2. -1.5}
    print(Vector.max(vec1, vec2)) --> Vector: {1, 1. 1.5}
    print(Vector.min(vec1, vec2)) --> Vector: {0.5, -1. -0}
end
```
---

!!!tip
    Numerous methods of Vector will return the same vector instance to allow easy "chaining". That way you can do more complex processing without saving an intermediate result in a variable, like e.g. `vec:setAt('y', 0):scale(0.5):rotateOver('y', 90)`.

##Element access summary

Apart from being able to access elements using x, y, z and 1, 2, 3 keys directly, following methods directly modify vector components.

Function Name | Description | Return | &nbsp;
-- | -- | -- | --
setAt([<span class="tag str"></span>](/types)&nbsp;k, [<span class="tag flo"></span>](/types)&nbsp;value) | Sets a component to value and returns self. | [<span class="tag vec"></span>](/types#vector) 
set([<span class="tag flo"></span>](/types)&nbsp;x, [<span class="tag flo"></span>](/types)&nbsp;y, [<span class="tag flo"></span>](/types)&nbsp;z) | Sets `x`, `y`, `z` components to given values and returns self. Providing a nil value makes it ignore that argument. | [<span class="tag vec"></span>](/types#vector) 
get() | Returns `x`, `y`, `z` components as three separate values. | [<span class="tag flo"></span>](/types), [<span class="tag flo"></span>](/types), [<span class="tag flo"></span>](/types)
copy() | Returns a separate Vector with identical component values. | [<span class="tag vec"></span>](/types#vector) 

!!!tip
    Before `Vector` was introduced, coordinate tables contained separate values under 1, 2, 3 and x, y, z keys, with letter keys taking precedence when they were different. This is no longer the case, and using letter and numerical keys is equivalent. However, when iterating over Vector components you have to use `pairs` and only letter keys will be read there.

###Element access examples


```lua
function onLoad()
    local vec = Vector(1, 2, 3)
    vec.x = 2           -- set the first component
    vec[2] = 4          -- set the second component
    vec:setAt('z', 6)   -- set the third component

    print(vec:get()) --> same as print(vec.x, vec.y, vec.z)

    vec:copy():setAt('x', -11)
    print(vec.x) --> 2, because we only changed 'x' on a copy
end
```

---

##Arithmetics summary

Function Name | Description | Return | &nbsp;
-- | -- | -- | --
add([<span class="tag vec"></span>](/types)&nbsp;other) | Adds components of `other` Vector and returns self. | [<span class="tag vec"></span>](/types#vector) 
sub([<span class="tag vec"></span>](/types)&nbsp;other) | Subtracts components of `other` Vector and returns self. | [<span class="tag vec"></span>](/types#vector) 
scale([<span class="tag flo"></span>](/types)&nbsp;sx, [<span class="tag flo"></span>](/types)&nbsp;sy, [<span class="tag flo"></span>](/types)&nbsp;sz) | Multiplies each component by a factor and returns self. Providing a nil value makes it ignore that argument. | [<span class="tag vec"></span>](/types#vector) 
scale([<span class="tag vec"></span>](/types)&nbsp;other) | Multiplies each component by correspoding component of `other` and returns self. | [<span class="tag vec"></span>](/types#vector) 
scale([<span class="tag flo"></span>](/types)&nbsp;scale) | Multiplies each component by the same provide factor and returns self. | [<span class="tag vec"></span>](/types#vector) 
inverse() | Scales each component by -1 and returns self. | [<span class="tag vec"></span>](/types#vector) 
dot([<span class="tag vec"></span>](/types)&nbsp;other) | Returns a dot product of self and `other`. | [<span class="tag flo"></span>](/types)
cross([<span class="tag vec"></span>](/types)&nbsp;other) | Returns a new Vector that is a cross product of self and `other`. | [<span class="tag vec"></span>](/types#vector) 
equals([<span class="tag vec"></span>](/types)&nbsp;other, [<span class="tag flo"></span>](/types)&nbsp;margin) | Returns a boolean whether `other` is similiar enough to self. The `margin` argument is optional and defaults to tolerating a difference of ~0.03 in both vector magnitude. | [<span class="tag boo"></span>](/types)

Vector also allows you to use arithmetic operators to performs basic operations:

Operator | Description | Return | &nbsp;
-- | -- | -- | --
[<span class="tag vec"></span>](/types)&nbsp;one + [<span class="tag vec"></span>](/types)&nbsp;two | Returns a new Vector that is a sum of `one` and `two` | [<span class="tag vec"></span>](/types#vector) 
[<span class="tag vec"></span>](/types)&nbsp;one - [<span class="tag vec"></span>](/types)&nbsp;two | Returns a new Vector that is a difference of `one` and `two` | [<span class="tag vec"></span>](/types#vector) 
[<span class="tag vec"></span>](/types)&nbsp;one * [<span class="tag flo"></span>](/types)&nbsp;factor | Returns a new Vector that is `one` with each component multiplied by the factor. | [<span class="tag vec"></span>](/types#vector) 
[<span class="tag vec"></span>](/types)&nbsp;one == [<span class="tag vec"></span>](/types)&nbsp;two | Returns a boolean whether `one` and `two` are very similiar to each other (less than ~0.03 difference in magnitude) | [<span class="tag boo"></span>](/types)

###Arithmetics and operators examples


```lua
function onLoad()
    local vec = Vector(1, 2, 3)
    vec:add(Vector(3, 2, 1)) --> vec is now {4, 4, 4}
    vec:sub(Vector(1, 0, 1)) --> vec is now {3, 4, 3}
    
    local another = vec + Vector(-1, -2, -1) --> another is {2, 2, 2}, vec remains unchaged

    print(another:equals(Vector(1, 2, 3))) --> false
    print(another == Vector(2, 2, 2)) --> true
    print(another == Vector(1.99, 2.01, 2)) --> true, small differences are tolerated
end
```

---

##Property methods summary
Function Name | Description | Return | &nbsp;
-- | -- | -- | --
magnitude() | Returns the length of a Vector. | [<span class="tag flo"></span>](/types)
sqrMagnitude() | Returns squared length of a Vector. This is less expensive than `vec:magnitude()` while just as useful for e.g. comparisons. | [<span class="tag flo"></span>](/types)
distance([<span class="tag vec"></span>](/types)&nbsp;other) | Returns the distance between self and `other` Vector. | [<span class="tag flo"></span>](/types)
sqrDistance([<span class="tag vec"></span>](/types)&nbsp;other) | Returns squared distance between self and `other` Vector. | [<span class="tag flo"></span>](/types)
angle([<span class="tag vec"></span>](/types)&nbsp;other) | Returns an angle (in degrees) between self and `other` Vector. This angle is always positive and in a `[0, 180]` range. | [<span class="tag flo"></span>](/types)
heading([<span class="tag str"></span>](/types)&nbsp;axis) | Returns an angle In degrees) of rotation of Vector over a given `axis` (can be `'x'`, `'y'`, `'z'`). | [<span class="tag flo"></span>](/types)
string([<span class="tag str"></span>](/types)&nbsp;prefix) | Returns a formatted string with Vector components. The `prefix` argument is optional, aiding in output readability. This function is automatically called when you do `print(vec)` or `tostring(vec)`. | [<span class="tag str"></span>](/types)

###Property menothds examples


```lua
function onLoad()
    local vec = Vector(1, 0, 1)
    print(vec:magnitude())    --> 1.4142... (sqrt of 2)
    print(vec:sqrMagnitude()) --> 2

    local oneAtX = Vector(1, 0, 0)
    local oneAtXZ = Vector(1, 0, 1)
    print(oneAtX:angle(oneAtXZ))     --> 45 (degress)
    print(oneAtX:distance(oneAtXZ))  --> 1

    print(oneAtX:heading('y'))  -- 90 (degrees)
    print(oneAtXZ:heading('y')) -- 45 (degrees)

    print(oneAtX:string('oneAtX')) --> 'oneAtX: { 1, 0, 0 }'
end
```

---

##Manipulation summary

Function Name | Description | Return | &nbsp;
-- | -- | -- | --
clamp([<span class="tag flo"></span>](/types)&nbsp;maxLen) | Shortens the Vector down to a maximum of `maxLen` if it is longer and returns self. | Vector 
lerp([<span class="tag vec"></span>](/types)&nbsp;target, [<span class="tag flo"></span>](/types)&nbsp;factor) | Linearly interpolates Vector between its current position and `target` depending on `factor` value and returns self. For example `factor` of 0.5 places it halfway between current position and `target`, and `factor` of 1 sets it to `target` exactly. Any value of `factor` is allowed, including negative and over 1. | Vector 
moveTowards([<span class="tag vec"></span>](/types)&nbsp;target, [<span class="tag flo"></span>](/types)&nbsp;maxLen) | Move a vector linearly towards `target`, but only up to a difference of `maxLen`, and return self. | Vector 
normalize() | Scales a Vector so its length is exactly 1 and returns self. Does not do anything with zero Vector. | Vector 
normalized() | Returns a new vector with the same direction and length of 1. | Vector 
project([<span class="tag vec"></span>](/types)&nbsp;other) | Modifies a Vector to be a projection on `other` and returns self. | Vector 
projectOnPlane([<span class="tag vec"></span>](/types)&nbsp;normal) | Modifies a Vector to be a projection on a plane defined by a normal vector `normal` and returns self. | Vector 
reflect([<span class="tag vec"></span>](/types)&nbsp;normal) | Reflect a Vector over a plane defined by a normal Vector `normal` and return self. | Vector 
rotateTowards([<span class="tag vec"></span>](/types)&nbsp;other, [<span class="tag flo"></span>](/types)&nbsp;maxAngle)) | Rotates a Vector towards `other` (over shortest path), but only up to an angle difference of `maxAngle`, and return self. | Vector 
rotateTowardsUnit([<span class="tag vec"></span>](/types)&nbsp;other, [<span class="tag flo"></span>](/types)&nbsp;maxAngle) | Same as rotateTowards, but only works correctly if `other` Vector is normalized. Less expensive than `rotateTowards`. | Vector 
rotateOver([<span class="tag str"></span>](/types)&nbsp;axis, [<span class="tag flo"></span>](/types)&nbsp;angle) | Rotate a Vector `angle` degress over given `axis` (can be `'x'`, `'y'`, `'z'`) and return self. | Vector 
orthoNormalize() | Returns three unit Vectors, each one orthogonal to every other. The first returned Vector points in the same direction as this Vector. | Vector, Vector, Vector
orthoNormalize([<span class="tag vec"></span>](/types)&nbsp;other) | Same as `orthoNormalize`, but the second returned Vector is guaranteed to be on a plane spanned by this Vector and `other`. | Vector, Vector, Vector

###Manipulation examples

Moving an object towards a target position in small steps
```lua
function onLoad()
    local obj = assert(getObjectFromGUID('555555'), 'Object not found!')
    obj.lock()

    local current =  Vector(10, 5, 0) -- obj starting position
    local target =   Vector(-10, 5, 0) -- obj destination
    local movementType = 'asymptotic' -- try with 'spherical' or 'asymptotic' to see how other methods work

    -- We want out movement stretched over time, a Wait will do it periodically
    local waitID
    waitID = Wait.time(
        function()
            -- move the current postion towards destination

            if movementType == 'linear' then
                -- simple linear movement, 1 unit at a time
                current:moveTowards(target, 1)
            elseif movementType == 'spherical' then
                -- rotate towards target, 10 degress at a time
                current:rotateTowards(target, 5)
            elseif movementType == 'asymptotic' then
                -- move quarter of the way towards target (take note that lerp does not modify current directly)
                current = current:lerp(target, 0.25)
            end

            obj.setPositionSmooth(current, true, true)

            -- if we reached the destionation, stop this timer
            if current == target then
                Wait.stop(waitID)
                broadcastToAll('Finished!', {0, 1, 0})
            end
        end,
        0.5,  -- repeats every half second
        -1  -- indefinitely, until stopped because we reached destination
    )
end
```