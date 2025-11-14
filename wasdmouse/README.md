# WASD Mouse Dweet Breakdown

https://www.dwitter.net/d/34522

But how does it work?

## Inspiration
### Original d/23034 (minified)
A lot of the logic was created by the original https://www.dwitter.net/d/23034 , but a bit simplified and reworked to get WASD buttons working. Here is the original, for reference.

```js
eval(unescape(escape`󩌽󭼽󪬽󨼮󭽩󩍴󪍼󟝴󟽯󫭭󫽵󬽥󫝯󭭥󟝥󟜾󤬫󟝍󛜨󣜽󩜮󮌯󜼰󚜺󦌽󦬽󤬽󣜽󝜊󩭯󬬨󨜽󤼨󤬩󛍢󟝃󚍒󚜻󪬻󩌫󟜮󜜩󚍘󚼨󠜽󪬯󭼪󩌭󩌯󜬩󚭢󛝤󚭡󯍚󛝁󚭡󛝤󚭢󚜥󞜾󜭼󯍸󛭦󪝬󫍒󩝣󭌨󪬭󛜬󝜴󜌭󜝥󜼯󩌬󜜭󜜯󩌬󜭥󜼯󩌬󩌽󚌱󛜱󛽤󛽤󚜯󜭥󝌬󦌭󟝤󚭡󛍚󛜽󩌪󨬩`.replace(/u../g,'')))
```
#### Decompressed
```js
d=w=j=c.width|=t?onmousemove=e=>R+=M-(M=e.x/30):X=Z=R=M=5
for(a=S(R),b=C(R);j;d+=.1)(X+(A=j/w*d-d/2)*b-d*a|Z-A*a-d*b)%9>2||x.fillRect(j--,540-1e3/d,1-1/d,2e3/d,d=(1-1/d/d)/2e4,X-=d*a,Z-=d*b)
```

#### Better formatted and slightly rearranged so you can see similarities

```js
d=w=j=c.width|=t

if(t){
    onmousemove=e=>{
        R+=M-(M=e.x/30)
    }
} else {
    X=Z=R=M=5
}

a=S(R);
b=C(R)

for(;j;d+=.1){
    A=j/w*d-d/2

    if((X+A*b-d*a|Z-A*a-d*b)%9>2){
    }else{
        x.fillRect(j--,540-1e3/d,1-1/d,2e3/d);
        d=(1-1/d/d)/2e4;
        X-=d*a;
        Z-=d*b;
    }
}
```

## dweet 34522
```js
eval(unescape(escape`𩡯𬠨𭰽𪠽𨰮𭱩𩁴𪁼👴🱊👛𨠽𠰨𥀩𛀬𨐽𤰨𥀩𛁯𫡫𩑹𩁯𭱮👥🐾𚁘𚰽𢡛𪐽𩐮𭱨𪑣𪀦𝡝𛰹𛁚𚰽𢡛𪐫𜠦𝡝𛰹𚐬𛑢𛀬𛑡𧐺𦀽𦠽𝰻𪠻𤠽𦀭𤠪𤰨𡀽𥀫𪠯𭰭𛠵𚐦𦠫𤠪𠰨𡀩🱒𚰮𜐺𘑸𛡦𪑬𫁒𩑣𭀨𪠭𛐬𝐴𜀭𜑥𜰯𤠬𜠯𤠬𭰯𤠩𚑯𫡭𫱵𬱥𫑯𭡥👥🐾𥀽𩐮𮀯𞐰`.replace(/u../g,'')))
```
This just barely sneaks in at 140 characters.


### Replace eval with throw to get the page to output:

```js
for(w=j=c.width|=t?J=[b=C(T),,a=S(T),onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9),-b,,-a]:X=Z=7;j;R=X-R*S(D=T+j/w-.5)&Z+R*C(D)?R+.1:!x.fillRect(j--,540-1e3/R,2/R,w/R))onmousemove=e=>T=e.x/90
```
This is 194 characters, the maximum allowed length before this compression technique.
You'll notice that just the key + mouse handlers `onkeydown=e=>` and `onmousemove=e=>` take up an entire 28 characters, leaving us just 166 for the rest of the logic.

## Basic structure
Let's break this down, first by formatting it a bit better.

```js
for(
        w=j=c.width|=t?J=[b=C(T),,a=S(T),onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9),-b,,-a]:X=Z=7;
        j;
        R=X-R*S(D=T+j/w-.5)&Z+R*C(D)?R+.1:!x.fillRect(j--,540-1e3/R,2/R,w/R)){
    onmousemove=e=>T=e.x/90
}
```

Everything between `for(` and the first `;` can be moved to before the for loop. Everything between the second `;` of the for loop and the `)` can be moved into the for loop, after the body.

```js
w=j=c.width|=t?J=[b=C(T),,a=S(T),onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9),-b,,-a]:X=Z=7
for(;j;){
    onmousemove=e=>T=e.x/90
    R=X-R*S(D=T+j/w-.5)&Z+R*C(D)?R+.1:!x.fillRect(j--,540-1e3/R,2/R,w/R)
}
```

Let's make the `J` array look a bit better, and make the ternary operator look like an if else.

```js
w=j=c.width|=t?J=[
    b=C(T),
    ,
    a=S(T),
    onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9),
    -b,
    ,
    -a
]:X=Z=7
for(;j;){
    onmousemove=e=>T=e.x/90
    R=X-R*S(D=T+j/w-.5)&Z+R*C(D)
        ?R+.1
        :!x.fillRect(j--,540-1e3/R,2/R,w/R)
}
```

Still looks like a lot so let's explain a few things to help format it.


First off, the `onmousemove` in the for loop body. 
While probably not performant, it is perfectly fine to reinitialize the `onmousemove` handler on each iteration of the loop.
I did this to save a few char, as otherwise we'd need have nothing between the `for(;;` and the `)`, or a spare `;` in the for loop body.
These "empty spaces" are great for declaring variables, as otherwise we'd need a new line or a `;` to declare something.
Remember we need every last char here.
So instead of a `;` in the for loop body, we just redeclared `onmousemove`.
We could swap the locations of `onkeydown` and `onmousemove`, but this seemed a bit more performant.

In any case, we can move the onmouse move to the top, to just declare it once.

```js
onmousemove=e=>T=e.x/90
w=j=c.width|=t?J=[
    b=C(T),
    ,
    a=S(T),
    onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9),
    -b,
    ,
    -a
]:X=Z=7
for(;j;){
    R=X-R*S(D=T+j/w-.5)&Z+R*C(D)
        ?R+.1
        :!x.fillRect(j--,540-1e3/R,2/R,w/R)
}
```

For reasons that will become clear soon, we can also move the `onkeydown` declaration to the top, out of the `J` array.
We are still redefining the event handlers every 1/60 s even though they don't change, but this was the only way to save chars.

```js
onmousemove=e=>T=e.x/90;
onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9);
w=j=c.width|=t?J=[
    b=C(T),
    ,
    a=S(T),
    ,
    -b,
    ,
    -a
]:X=Z=7
for(;j;){
    R=X-R*S(D=T+j/w-.5)&Z+R*C(D)
        ?R+.1
        :!x.fillRect(j--,540-1e3/R,2/R,w/R)
}
```

## Initialization
Let's look at that `w=j=c.width` structure first.


We'll get to `J` in a bit, but for now let's just say it's fine to redefine it even if `t==0`, so we'll move it out and use a `1` as a placeholder between `?` and `:`. 

```js
w=j=c.width|=t?1:X=Z=7
onmousemove=e=>T=e.x/90;
onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9);
J=[
    b=C(T),
    ,
    a=S(T),
    ,
    -b,
    ,
    -a
]
for(;j;){
    R=X-R*S(D=T+j/w-.5)&Z+R*C(D)
        ?R+.1
        :!x.fillRect(j--,540-1e3/R,2/R,w/R)
}
```

Now we just have:
```js
w=j=c.width|=t?1:X=Z=7
```

I was honestly a bit confused by the order of operations here too.
Had to experiment a bit to determine that is is in fact equivalent to this:

```js
w=(j=(c.width|=(t?[]:(X=Z=7))))
```

I used a `[]` instead of `1` to represent `J`.
The parenthesis make the order of operations a bit more clear.
The first thing that happens is the ternary on `t`.
The second is the `c.width|=`.
If it's not clear, we can put pretty much anything right after `c.width|=` and it will do what it needs to do.

In the dweet, we effectively do `c.width|=7` if `t==0`, and `c.width|=J` if `t>0`.
How does it make sense to do a OR EQUAL operation between a number and an array?
No idea.

In the `t==0` case, `c.width` is set to `1920|[]`, which is 1920.
But this only happens for a single frame and is not noticable (the screen is black as `T` is `Math.tan` and not a number anyways).

In the `t>0` case, what we do is equivalent to `c.width = c.width |= 7`.
`c.width` is initially 1920, and doing this sets it to 1927, which is not a noticable difference.
Subsequent `c.width|=7` calls keeps it at 1927.

Because the value after `c.width|=` doesn't really matter as long as we keep it small, we can split this out a bit for clarity.

```js
if (t) {
} else {
    X=Z=7
}
w=(j=(c.width|=7))
onmousemove=e=>T=e.x/90;
onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9);
J=[
    b=C(T),
    ,
    a=S(T),
    ,
    -b,
    ,
    -a
]
for(;j;){
    R=X-R*S(D=T+j/w-.5)&Z+R*C(D)
        ?R+.1
        :!x.fillRect(j--,540-1e3/R,2/R,w/R)
}
```

That if else on `t` is critical.
Because `t` starts at 0 and only goes up, as set up by dwitter, we only have a single frame where `t==0`.
In that frame, we define `X` and `Z` to be `7` in just 5 characters.
`X` and `Z` are probably the best variable names in this entire thing, as they of course correspond to X and Z coordinates in a "3D" space.
We could choose `X` and `Z` to be different values, but this is more concise.

`7` was chosen because it is a single digit, and `[7,7]` works out to a nice "initial position" in the pattern.
Some other low values end up putting us "in a wall" as you'll see later.

For clarity, let's make some nicer variable names for this part.

```js
time = t
if (time === 0){
    zPos = 7;
    xPos = 7;
}
w=(j=(c.width|=7))
onmousemove=e=>T=e.x/90;
onkeydown=e=>(xPos+=J[i=e.which&6]/9,zPos+=J[i+2&6]/9);
J=[
    b=C(T),
    ,
    a=S(T),
    ,
    -b,
    ,
    -a
]
for(;j;){
    R=xPos-R*S(D=T+j/w-.5)&zPos+R*C(D)
        ?R+.1
        :!x.fillRect(j--,540-1e3/R,2/R,w/R)
}
```

Now for the `w=j=...` line.
The first thing that happens is `c.width|=7` (or `J` in the actual dweet).
We can't do `c.width=`, as it would set the width to 7 and mess everything up.
But `c.width = c.width | 7` is fine.
Setting `c.width` is standard practice in dwitter to clear the screen.
It just works.
Ex: https://www.dwitter.net/d/3760

We then set `j` to `c.width`, as well as `w`.
`w` is the width of the canvas, which is useful later.
If you look at the for loop, you'll notice that `j` is an indexing variable.
It is set to `w` initially and sometimes decrements, until `j` evaluates to false, or `j == 0`.
This then breaks the for loop.
So to make it more clear it is an index variable, we can move it into the for loop parenthesis, along with all the other changes described and better variable names.

```js
let time = t;
if (time === 0){
    zPos = 7;
    xPos = 7;
}
c.width = c.width | 7;
let canvasWidth = c.width;
onmousemove=e=>T=e.x/90;
onkeydown=e=>(xPos+=J[i=e.which&6]/9,zPos+=J[i+2&6]/9);
J=[
    b=C(T),
    ,
    a=S(T),
    ,
    -b,
    ,
    -a
]
for(let j = canvasWidth;j;){
    R=xPos-R*S(D=T+j/canvasWidth-.5)&zPos+R*C(D)
        ?R+.1
        :!x.fillRect(j--,540-1e3/R,2/R,canvasWidth/R)
}
```

## Mouse input
Alright now let's look at `onmousemove`.
This handler just takes the x component of the mouse event, divides it by 90, and assigns that value to `T`.
`T` is used specifically because it is already defined in dwitter, as `T= Math.tan`.
By using it, we can avoid an extra `T=` and avoid undefined issues when we try to render. As you can see, `T` is used in the for loop.
While this does mean the screen is all black before you move the mouse, that was a tradeoff I chose to make the code fit.


This works a little bit differently from the original dweet, which does:

```js
R+=M-(M=e.x/30)
```

`R`, funny enough, is also predefined in dwitter, which is why we use it elsewhere.
I'm not entirely sure how the original works, but as it would seem to just do `R+=0` each time.
But it kinda "nudges" `R` (used in the same manner that I use `T`) based on the difference between this `e.x` and the previous `e.x`.

In my dweet this took up too much space so I just decided to divide `e.x` by 90, which doesn't nudge it all that smoothly, but still allows you to pan around 360 deg depending on mouse position.

We divide by 90 to slow the panning of the camera.
Otherwise we would go through a full rotation every 6 horizontal px which is very disorienting.
I considered doing just `e.x/9` to save an extra character, but that is still very jumpy.

With all that considered, let's give `T` and the first `e` proper names.

```js
let time = t;
if (time === 0){
    zPos = 7;
    xPos = 7;
}
c.width = c.width | 7;
let canvasWidth = c.width;
onmousemove = (mouseEvent) => {
    angle = mouseEvent.x/90;
}
onkeydown=e=>(xPos+=J[i=e.which&6]/9,zPos+=J[i+2&6]/9);
J=[
    b=C(angle),
    ,
    a=S(angle),
    ,
    -b,
    ,
    -a
]
for(let j = canvasWidth;j;){
    R=xPos-R*S(D=angle+j/canvasWidth-.5)&zPos+R*C(D)
        ?R+.1
        :!x.fillRect(j--,540-1e3/R,2/R,canvasWidth/R)
}
```

Right now this won't actually work as `angle` is NOT predefined in dwitter, so let's define it in the `time===0` block.
And set it to `Math.tan` for the hell of it.

```js
let time = t;
if (time === 0){
    zPos = 7;
    xPos = 7;
    angle = Math.tan;
}

c.width = c.width | 7;
let canvasWidth = c.width;

onmousemove = (mouseEvent) => {
    angle = mouseEvent.x/90;
}

onkeydown=e=>(xPos+=J[i=e.which&6]/9,zPos+=J[i+2&6]/9);
J=[
    b=C(angle),
    ,
    a=S(angle),
    ,
    -b,
    ,
    -a
]
for(let j = canvasWidth;j;){
    R=xPos-R*S(D=angle+j/canvasWidth-.5)&zPos+R*C(D)
        ?R+.1
        :!x.fillRect(j--,540-1e3/R,2/R,canvasWidth/R)
}
```


## Keyboard input
Alright now for `onkeydown` and `J`.
This is the key to WASD functionality.
In the dweet, `J` has "holes" in it that we can put some event handlers, and you'll see why these holes are here.

And to be honest, there's some linear algebra here that I don't fully understand.
I had to muddle with it, swapping order and changing signs to get it to work. 
The mouse handler was also `e=>T=e.x/-90` at some point, and I somehow figured out a way to remove the negative sign.

A lot of credit to this dweet too: https://www.dwitter.net/d/29581 .
I took some inspiration from how they used `e.which` to map to a direction, and expanded on it a bit.

Let's start with this: we need to handle keys WASD.
The `which` property, while deprecated, is the shortest way to turn these into numbers.
Great for dwitter code golfing hacks.
https://developer.mozilla.org/en-US/docs/Web/API/UIEvent/which

If you do `onkeydown=e=>console.log(e.which)`, you'll see that a,s,d, and w corresponds to
65, 83, 68, and 87.
The best part about these four numbers is that if you do `&6`, you get...
0, 2, 4, and 6.
This `&6` was not super obvious, and I had to try a few different operations to get it to a nice orderly sequence.
Nice quirk of the alphabet I guess.
This sequence has the great property that each number is evenly spaced, you'll see why in a bit.
In the dweet mentioned above, lewdev used `e.which%32%17`, which is similar but unfortunately won't fit.

| Key                   |   a    |   s    |   d    |   w    |
|:--------------------:|:------:|:------:|:------:|:------:|
| `e.which`            |   65   |   83   |   68   |   87   |
| Binary `e.which`   |`1000001`|`1010011`|`1000100`|`1010111`|
|  Binary 6       |      `0000110   `|      `0000110   `|   `   0000110   `|      `0000110   `|
| Binary `e.which & 6` |`0000000`|`0000010`|`0000100`|`0000110`|
| Final Result         |   0    |   2    |   4    |   6    |

Now let's talk about the linear algebra side of this.
If we want to move in a 2d space, which is what this really is, while also being able to pan around, it's not as simple as `X+=1,Z+=0` for up.
We also have a direction we're facing, which we get from the `mouseEvent.x`

If we want to move "forwards" with `w`, we move X some amount, and Z some amount.
Same for A, S, and D.
If we move in the same direction as the angle we're facing (`angle`), we need to move X in `some amount * cos(angle)`, and Z in `some amount * SIN(angle)`.
If we want to move backwards, for S, we need to move X `some amount * -COS(angle)`, and Z `some amount * -SIN(angle)`.

I'll just tell you that "some amount" is `1/9`, as you might be able to see in the `onkeydown` function.
This is the smallest value I could use, as `1/10` would require additional 2 bytes (one for X and one for Z).

If we want to move LEFT, we need to move X `1/9 * -SIN(angle)` and move Z `1/9 * COS(angle)`.
If we want to move RIGHT, we need to move X `1/9 * SIN(angle)` and move Z `1/9 * -COS(angle)`.


Now let's just summarize how we need to update X & Z for keys `a,s,d,w` (in that order because they map to `0,2,4,6`).
Ignoring the `1/9` for now.

|         |   a   |   s   |   d   |   w   |
|---------|-------|-------|-------|-------|
| **X**   | +SIN(angle) | -COS(angle) | -SIN(angle) | +COS(angle) |
| **Z**   | -COS(angle) | -SIN(angle) | +COS(angle) | +SIN(angle) |
| `e.which&6 `         |   0    |   2    |   4    |   6    |

Great, now let's put them into an X array and a Z array.
And add a space between each so we can simply do `XArray[e.which&6]` with no further modifications.

```js
XDelta = [+SIN(angle),, -COS(angle),, -SIN(angle),, +COS(angle)]
ZDelta = [-COS(angle),, -SIN(angle),, +COS(angle),, +SIN(angle)]
```

Notice anything?
The same values of `XDelta` appear in `ZDelta`, but "rotated" -2.
So we don't even need two separate arrays, we can use the same array!
For X, we'll do `array[e.which&6]`, and for Z we'll do `array[e.which-2&6]`.

To save a bit more space, we can assign `i=e.which&6` when we get X and reuse it when we get Z, with `i-2&6`.
Not only does the order of operations work nicely here with `&6`, the `&` operator will also convert negative numbers to positive numbers.
This gives it an advantage over using `%` for array access.

I also considered removing the gaps and doing something like `array[(e.which&6)/2]`, but that ended up longer so kept the gaps.
In the final dweet, we could fit the event handler in the gaps no problem.

Now you might notice in the dweet the actual array is a bit different, and we do `i+2&6` for Z instead of `i-2&6`.
This is where it gets confusing for me too.
Hell, Z and X might need to be swapped for all I know.
And angle and left and right might also be swapped.

The reason for all this rearranging is to ensure that the positives cos and sin appeared before the negatives in this array.
Otherwise we'd have to do something like `[a=-Sin(angle),..., -a]`, which is longer than `[a=Sin(angle),...,-a]`.

we still use the same general idea with "rotating through an array".
The final array (event handlers removed) looks like this:

```js
J=[b=C(T),,a=S(T),,-b,,-a]
```

Dwitter is nice in that `C` and `S` are predefined as `Math.cos` and `Math.sin`, saving a lot of chars here.
But we can give it better variable names now and spacing.

```js
let deltaArray = [cosAngle = Math.cos(angle), , sinAngle = Math.sin(angle), , -cosAngle, , -sinAngle];
```

Now let's look at the keyhandler event again.

```js
onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9);
```

Why `onkeydown` instead of `onkeyup`?
`onkeyup` is shorter, but if you hold a key, it only fires once when you let release the key.
`onkeydown` fires continuously while you hold it, which is exactly what we want here.

We can see the two array accesses, with an offset for Z.
Note that the "zindex" is equal to `(e.which&6)+2&6`, which works out to be the same as `e.which+2&6`.
With this in mind let's format it a bit nicer.

```js
onkeydown = (keyEvent) => {
    let keyCode = keyEvent.which;
    let xDeltaIndex = keyCode & 6;
    let zDeltaIndex = keyCode + 2 & 6;
    let deltaScale = 1 / 9;
    
    xPos += deltaArray[xDeltaIndex] * deltaScale;
    zPos += deltaArray[zDeltaIndex] * deltaScale;
};
```


Putting everything we have together.

```js
let time = t;
if (time === 0){
    zPos = 7;
    xPos = 7;
    angle = Math.tan;
}

c.width = c.width | 7;
let canvasWidth = c.width;

onmousemove = (mouseEvent) => {
    angle = mouseEvent.x/90;
};

let deltaArray = [cosAngle = Math.cos(angle), , sinAngle = Math.sin(angle), , -cosAngle, , -sinAngle];

onkeydown = (keyEvent) => {
    let keyCode = keyEvent.which;
    let xDeltaIndex = keyCode & 6;
    let zDeltaIndex = keyCode + 2 & 6;
    let deltaScale = 1 / 9;
    
    xPos += deltaArray[xDeltaIndex] * deltaScale;
    zPos += deltaArray[zDeltaIndex] * deltaScale;
};

for(let j = canvasWidth;j;){
    R=xPos-R*S(D=angle+j/canvasWidth-.5)&zPos+R*C(D)
        ?R+.1
        :!x.fillRect(j--,540-1e3/R,2/R,canvasWidth/R)
}
```


## Ray Marching loop
Now let's move on to the body of the for loop.

Starting with `R`, this is very similar to what `d` was in the original 23034 dweet.
It's like "ray tracing depth".
It starts at a low value, increments until a certain condition is reached, then draws on the screen.
Depending on the specific condition, you get different patterns and different "walls" in the final scene.

To create these rays, the original dweet, first offset the ray a bit to the left or right of the position, then it marched forward in the exact angle that the "player" was facing.
As a result it created a orthographic projection.

Unfortunately I couldn't keep this as it required too many chars.
Instead I also used ray marching, but each ray started at the player's position `(xPos, zPos)`.
Then marched in a different direction depending on the x position on the screen.
So we have a perspective projection. 
The look is different, but pretty close.

Now let's talk about how we march exactly, for which we'll need to define an angle to march in.
This specific angle used here is:

```js
D = angle + j/canvasWidth -.5
```

`angle` is the angle the player is facing.
`j/canvasWidth` represents what "width fraction" of the screen we are on.
We start at `canvasWidth`, and go down to 0. Thus `j/canvasWidth` goes from 1 to 0.
Without the `-.5`, this means `D` will vary from `angle + 1` down to `angle`.
This is off-centered, so we subtract `-.5` and now `D` will vary from `angle+.5` down to `angle-.5`.
This gives us a nice 1 radian field of view, about 57 degrees.
Anything else would require additional chars.

You might notice there is no up/down or y coordinates.
That is because we are really moving in a 2d space that has a few effects to make it look 3d.
So we only iterate on the canvas x axis.

Now that we have `D`, our ray angle corresponding to a direction to march in, how will do march?

From the position `(X,Z)`, we will want to get to increment X with something times `Cos(D)`, and Z with something times `Sin(Z)`.
This something is `R` in this case, which allows for us to move some depth away from `(X,Z)`.
In the actual dweet we do:

```js
X - R*Sin(D)
Z + R*Cos(D)
```
Idk why `-Sin` and `Cos` work, but they do.

Now we can't just march forever.
Once we have the X position of the ray and the Z position of the ray, we do an `&` operation.
In the original dweet, it was something like `(rayX|rayZ)%9>2`, which made for a nicer pattern but I didn't have room.
Just a simple `&` operation is good enough here to render some simple patterns.

With all this, we can clean up the for loop body.
We can also call `j` something like `displayX`.

```js
let rayAngle = angle + displayX / canvasWidth - 0.5;
let rayX = xPos - R * Math.sin(rayAngle);
let rayZ = zPos + R * Math.cos(rayAngle);
let emptySpace = rayX & rayZ

R=emptySpace
?R+.1
:!x.fillRect(displayX--,540-1e3/R,2/R,canvasWidth/R)
```

By empty space here I mean "not a wall".

Now for the `R=` part.
Similar to `T`, `R` is predefined by dwitter as an RGB function.
This is great, but we don't need color here, so we used `R` for this "ray depth".
This allows us to skip `R=` while avoiding errors when using `R` in `fillRect`.

Because `R` is previously set to a function, we cannot do `R +=`.
So we do `R=`.
In the case that we hit "emptySpace", we simply do `R = R + .1`.
This will advance the depth by `.1` units.

But wait, what if `R` is set to a function?
How could we increment `R`?
Well if `R` is a function, then `rayX` and `rayZ` will evaluate to `NaN`
That means `emptySpace` will be to `0`, which will set `R` to `!x.fillRect(...)`.
That evaluates to `true`, allowing us to actually use it on the next iteration as if it were equal to `1` (as `true+.1 = 1.1`).
This does mean that we ray march starting from 1 "unit" away from the player position, but we don't have any chars to spare.

The `!x.fillRect(displayX--,540-1e3/R,2/R,canvasWidth/R)` will only trigger once we hit a wall, meaning we only reset `R` to 1 when we hit a wall.

## Rendering

That brings us to the final part:
`x.fillRect(displayX--,540-1e3/R,2/R,canvasWidth/R)`

`x` is of course the context, a shorthand from dwitter.
`fillRect` takes 4 parameters: x position of the top left corner, y position of the top left corner, width, and height.

### X position of the top left corner: `displayX--`
We already know `displayX` goes from 1927 down to 0.
So this directly corresponds to the position on the screen itself, no tricks here.

We only decrement `j` when we hit a wall.
So the for loop can loop many times without decrementing `j` as it marches the ray.

### Height: `canvasWidth/R`
### Y position of the top left corner: `540-1e3/R` and Height: `canvasWidth/R`
This is where the 3D effect comes from.
While not actually 3d, we draw the rectangles in such a way that closer "walls" appear higher and further walls appear shorter.
To see exactly how, look at the height of this rectangle.
It is `canvasWidth/R`.
If you add the height to the y position, you get `540-1e3/R+canvasWidth/R`, which is `540-1000/R+1927/R`, which is `540+927/R`.
So the rectangle goes from `540-1e3/R` to `540-927/R`.

`540` is of course `1080/2`, and `1080` is the default height.
So it is a rectangle of height `1927/R`, roughly centered on `540`, the vertical middle.
If ray depth is large (far), the rectangle will be shorter.
If the ray depth is small (near), the rectangle will be taller.

In the actual dweet, height is `w/R`.
We already defined `w = canvasWidth` so it's cheaper to use `w` instead of something like `2e3`.

### Width: `2/R`
This might look strange, as `R` starts at 1 and increments by `+.1`.
So this can have a max value of 2, and keep going down, even into values less than 1.
We can't draw a rectangle with a width between 1 and 0, but what this will do instead is draw a rectangle of width 1, but with reduced opacity.
So `2/R` is effectively the opacity of the rectangle.

Because the default `fillStyle` is black and we're drawing on a white background, this results in further walls (large `R`) being faint white, and closer walls (small `R`) being dark.
I chose 2 instead of 1 just to make it a bit more opaque.
3 doesn't look bad either.

In the original, this was `1-1/R`.
This meant that closer walls would appear brighter, and further walls would appear darker.
While a cool effect, unfortunately I could not spare any chars and had to flip this.
We now have a fog effect, with further walls fading in the distance and closer walls very dark.
I've tried `c.style.filter='invert()'`, and it does look pretty sick, but again we can spare no characters.


Cleaning up the for loop body more:

```js
let rayAngle = angle + displayX / canvasWidth - 0.5;
let rayX = xPos - rayDepth * Math.sin(rayAngle);
let rayZ = zPos + rayDepth * Math.cos(rayAngle);
let emptySpace = rayX & rayZ

if (emptySpace){
    rayDepth = rayDepth + .1
} else {
    let rectX = displayX--;
    let rectY = 540 - 1000 / rayDepth;
    let rectOpacity = 2 / rayDepth;
    let rectHeight = canvasWidth / rayDepth;
    x.fillRect(rectX, rectY, rectOpacity, rectHeight);
    rayDepth = true;
}
```


## Final assembly
And finally putting everything we have together, renaming dwitter shims, and initializing `rayDepth` to `R` to match dwitter:

```js
let canvas = c;
let context = x;
let time = t;

if (time === 0){
    zPos = 7;
    xPos = 7;
    angle = Math.tan;
    rayDepth = R;
}

canvas.width = canvas.width | 7;
let canvasWidth = canvas.width;

onmousemove = (mouseEvent) => {
    angle = mouseEvent.x/90;
};

let deltaArray = [cosAngle = Math.cos(angle), , sinAngle = Math.sin(angle), , -cosAngle, , -sinAngle];

onkeydown = (keyEvent) => {
    let keyCode = keyEvent.which;
    let xDeltaIndex = keyCode & 6;
    let zDeltaIndex = keyCode + 2 & 6;
    let deltaScale = 1 / 9;

    xPos += deltaArray[xDeltaIndex] * deltaScale;
    zPos += deltaArray[zDeltaIndex] * deltaScale;
};

for (let displayX = canvasWidth; displayX; ) {
    let rayAngle = angle + displayX / canvasWidth - 0.5;
    let rayX = xPos - rayDepth * Math.sin(rayAngle);
    let rayZ = zPos + rayDepth * Math.cos(rayAngle);
    let emptySpace = rayX & rayZ

    if (emptySpace) {
        rayDepth = rayDepth + .1
    } else {
        let rectX = displayX--;
        let rectY = 540 - 1000 / rayDepth;
        let rectOpacity = 2 / rayDepth;
        let rectHeight = canvasWidth / rayDepth;
        context.fillRect(rectX, rectY, rectOpacity, rectHeight);
        rayDepth = true;
    }
}
```

This is over 1100 characters, which is a bit outside dwitter boundaries.
