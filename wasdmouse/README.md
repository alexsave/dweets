https://www.dwitter.net/d/34522
how does it work?

a lot of the logic was created by the original https://www.dwitter.net/d/23034 , but a bit simplified and compressed to get WASD buttons working. here is the original, for reference

eval(unescape(escape`󩌽󭼽󪬽󨼮󭽩󩍴󪍼󟝴󟽯󫭭󫽵󬽥󫝯󭭥󟝥󟜾󤬫󟝍󛜨󣜽󩜮󮌯󜼰󚜺󦌽󦬽󤬽󣜽󝜊󩭯󬬨󨜽󤼨󤬩󛍢󟝃󚍒󚜻󪬻󩌫󟜮󜜩󚍘󚼨󠜽󪬯󭼪󩌭󩌯󜬩󚭢󛝤󚭡󯍚󛝁󚭡󛝤󚭢󚜥󞜾󜭼󯍸󛭦󪝬󫍒󩝣󭌨󪬭󛜬󝜴󜌭󜝥󜼯󩌬󜜭󜜯󩌬󜭥󜼯󩌬󩌽󚌱󛜱󛽤󛽤󚜯󜭥󝌬󦌭󟝤󚭡󛍚󛜽󩌪󨬩`.replace(/u../g,'')))

d=w=j=c.width|=t?onmousemove=e=>R+=M-(M=e.x/30):X=Z=R=M=5
for(a=S(R),b=C(R);j;d+=.1)(X+(A=j/w*d-d/2)*b-d*a|Z-A*a-d*b)%9>2||x.fillRect(j--,540-1e3/d,1-1/d,2e3/d,d=(1-1/d/d)/2e4,X-=d*a,Z-=d*b)

or better formatted and slightly rearranged so you can see similarities

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

// now for 34522
eval(unescape(escape`𩡯𬠨𭰽𪠽𨰮𭱩𩁴𪁼👴🱊👛𨠽𠰨𥀩𛀬𨐽𤰨𥀩𛁯𫡫𩑹𩁯𭱮👥🐾𚁘𚰽𢡛𪐽𩐮𭱨𪑣𪀦𝡝𛰹𛁚𚰽𢡛𪐫𜠦𝡝𛰹𚐬𛑢𛀬𛑡𧐺𦀽𦠽𝰻𪠻𤠽𦀭𤠪𤰨𡀽𥀫𪠯𭰭𛠵𚐦𦠫𤠪𠰨𡀩🱒𚰮𜐺𘑸𛡦𪑬𫁒𩑣𭀨𪠭𛐬𝐴𜀭𜑥𜰯𤠬𜠯𤠬𭰯𤠩𚑯𫡭𫱵𬱥𫑯𭡥👥🐾𥀽𩐮𮀯𞐰`.replace(/u../g,'')))
this just barely sneaks in at 140 characters


replace eval with throw to get the page to output:

for(w=j=c.width|=t?J=[b=C(T),,a=S(T),onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9),-b,,-a]:X=Z=7;j;R=X-R*S(D=T+j/w-.5)&Z+R*C(D)?R+.1:!x.fillRect(j--,540-1e3/R,2/R,w/R))onmousemove=e=>T=e.x/90
this is 194 characters, the maximum allowed length before this compression technique
you'll notice that just the key + mouse handlers "onkeydown=e=>" and "onmousemove=e=>" take up an entire 28 characters, leaving us just 166 for the rest of hte logic

lets break this down, first by formatting it a bit better

for(
        w=j=c.width|=t?J=[b=C(T),,a=S(T),onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9),-b,,-a]:X=Z=7
        ;
        j
        ;
        R=X-R*S(D=T+j/w-.5)&Z+R*C(D)?R+.1:!x.fillRect(j--,540-1e3/R,2/R,w/R)){
    onmousemove=e=>T=e.x/90
}

everything between for( and the first ; can be moved to before the for loop. Everything between the second ; of the for loop and the ) can be moved into the for loop, whateever is after the body

w=j=c.width|=t?J=[b=C(T),,a=S(T),onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9),-b,,-a]:X=Z=7
for(;j;){
    onmousemove=e=>T=e.x/90
    R=X-R*S(D=T+j/w-.5)&Z+R*C(D)?R+.1:!x.fillRect(j--,540-1e3/R,2/R,w/R)
}

lets make the J array look a bit better, and make the ternary operator look like an if else

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

still looks like a lot so lets explain a few things to help format it.
First off, the onmousemove in the for loop body. 
while probably not performant, it is perfectly fine to reinitialize the onmousemove handler on each iteration of the loop
i did this to save a few char, as otherwise we'd need have nothing between the for(;; and the ), or a spare ; in the for loop body.
these "empty spaces" are great for declaring variables, as otherwise we'd need a new line or a ; to declare something
remember we need every last char here
so instead of a ; in the for loop body, we just redeclared onmousemove
we could swap the locations of onkeydown and onmousemove, but this seemed a bit more performant

in any case, we can move the onmouse move to the top, to just declare it once

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

for reasons that will become clear soon, we can also move the onkeydown declaration to the top, out of the J array
we are still redefining the event handlers every 1/60 s even though they don't change, but this was the only way to save chars

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

let's look at that w=j=c.width structure first


we'll get to J in a bit, but for now let's just say it's fine to redefine it even if t==0, so we'll move it out and use a 1 as a place holder between ? and : 

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

now we just ahve:
w=j=c.width|=t?1:X=Z=7

i was honestly a bit confused by the order of operations here too
had to experiment a bit to determine that is is in fact equivalent to this:

w=(j=(c.width|=(t?[]:(X=Z=7))))

i used a [] instead of 1 to represent J
the parenthesis make the order of operations a bit more clear
the first thing that happens is the ternary on t
the second is the c.width|=
if it's not clear, we can put pretty much anything right after c.width|= and it will do what it needs to do
in the dweet, we effectively do c.width|=7 if t==0, and c.width|=[...J] if t>0
how does it make sense to do a OR EQUAL operation between a number and an array?
No idea
in the t>0 case, what we do is equivalent to c.width = c.width |= 7
c.width is initially 1920, and doing this sets it to 1927, which isn't really noticable
doing c.width|=7 again keeps it at 1927
in the t==0 case, c.width is set to 1920|[], which is 1920
but this only happens for a single frame and is not noticable (the screen is black as T is Math.tan and not a number anyways)

but because the value after c.width|=  doesnt really matter as long as we keep it small, we can split this out a bit

if (t){
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

that if else on t is critical
because t starts at 0 and only goes up, as set up by dwitter, we only have a single frame where t==0
in that frame, we define X and Z to be 7 in jsut 5 characters
X and Z are probably the best variable names in this entire thing, as they of course correspond to X and Z coordinates in a "3D" space
we could choose X and Z to be different values, but this is more concise
7 was chosen because it is a single digit, and [7,7] works out to a nice "initial position" in the pattern
some other low values end up putting us "in a wall" as you'll see later

for clarity, let's make some nicer varible names for this part

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

now for the w=j=... line
the first thing that happens is c.width|=7 (or [] in the actual dweet)
setting c.width is standard practice in dwitter to clear the screen.
it just works
ex: https://www.dwitter.net/d/3760

we can't do c.width=, as it would set the width to 7 and mess everything up
but c.width = c.width | 7 is fine
we then set j to c.width, as well as w
w is the of the canvas, which is useful later for some scaling
if you look at the for loop, you'll notice that j is an indexing varible
it is to w initially and decrements until (j) evaluates to false, or j == 0
this then breaks the for loop
so to make it more clear it is an index variable, we can move it into the for loop parenthesis, along with all the other changes described and better variable names

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

alrigth now lets look at onmousemove first
this is pretty simple, it just takes the x componenet of the mouse event, divides it by 90, and assigns that value to T
T is used specifically becuase it is already defined in dwitter, as T= Math.tan
By using it, we can avoid an extra "T=" or get undefined issues when we try to render. as you can see, T is used in the for loop
while this does mean the screen is all black before you move the mouse, that was a tradeoff I chose to make it fit
another benefit of using T is that once you set the value, it'll be available in the next frame.
so we won't reset to some angle


this works a little bit differently from the original, which does:

R+=M-(M=e.x/30)

R funny enough is also predefined in dwitter, which is why we use it elsewhere
I'm not entirely sure how the original works, as on the surface it would seem to just do R+=0 eacah time
the order of operations is a bit strange here
but it kinda "nudges" R (used in the same manner that I use T) based on the difference between this e.x and the previous e.x

in my dweet this took up too much space so I just decided to device e.x by 90, which doesn't nudge it all that smoothly, but still allows you to pan around 360 deg depending on mouse position

we divide by 90 to slow the panning of the camera
otherwise we would go through a full rotation every 6 horizontal mouse pixels which is very disorienting
I considered doing just e.x/9 to save an extra character, but that is still very jumpy

with all that considered, it would be good to give T and the first e proper names


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

right now this won't actually work as angle is NOT predefined in dwitter, so let's define it in teh time===0 statement
and set it to Math.tan for the hell of it

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


alright now for onkeydown and J
this is the key to WASD functionality
in the dweet, J has "holes" in it that we can put some event handlers, and you'll see why these holes are here
and to be honest, there's some linear algebra here that I don't fully understand
I had to muddle with it, swapping order and changing signs to get it to work 
the mouse handler also used to be e=>T=e.x/-90, and I somehow figured out a way to remove the negative sign

anyways, a lot of credit to this dweet too: https://www.dwitter.net/d/29581
found it udner this tag: https://www.dwitter.net/h/wasd 
I took some inspiration from how they used e.which to map to a direction, and expanded on it to handle WASD

let's start with this: we need to handle keys WASD
this property, while deprecated, is the shortest way to turn these into numbers 
https://developer.mozilla.org/en-US/docs/Web/API/UIEvent/which
great for dwitter code golfing hacks

if you just do onkeydown=e=>console.log(e.which), you'll see that a,s,d,w corresponds to
65, 83, 68, and 87
The best part about these four numbers is that if you do &6, you get...
0, 2, 4, and 6
This &6 was not super obvious, and I had to try a few different operations to get it to a nice orderly sequence
nice quirk of the alphabet I guess
in the 29581,  lewdev used e.which%32%17, which is similar but unfortunately won't fit 
this sequence has the great property that each number is evenly spaced, you'll see why in a bit

now lets talk about the other side of this, the linear algebra
if we want to move in a 2d space, which is what this really is, while also being able to pan around, it's not as simple as X+=1,Z+=0 for up
we also have a direction we're facing


if we want to move "forwards" with w, we don't just move in the X direction
we move X some amount, and Z some amount
same for A, S, D
if we move in the same direction as the angle we're facing (now the variable called angle), we need to move X in some amount * COS(angle), and Z in some amount SIN(angle)
if we want to move backwards, for S, we need to move X some amount * -COS(angle), and Z some amount * -SIN(angle)

I'll just tell you that "some amount" is 1/9, as you might be able to see in the onkeydown function
this is the smallest value I could use , as 1/10 would require additional 2 bytes (one for X and one for Z)

if we want to move LEFT, we need to move X 1/9 * -SIN(angle) and move Z 1/9 * COS(angle)
if we want to move RIGHT, we need to move X 1/9 * SIN(angle) and move Z 1/9 * -COS(angle)


now let's just summarize how we need to update X & Z for keys a,s,d,w (in that order because they map to 0,2,4,6)
ignoring the 1/9 for now

            X           Z
a (right)   +SIN(angle) -COS(angle)
s (down)    -COS(angle) -SIN(angle)
d (left)    -SIN(angle) +COS(angle)
w (up)      +COS(angle)  +SIN(angle)

great, now let's put them into an X array and a Z array
and add a space between each so we can simply do XArray[e.which&6] with no further modifications

XDelta =[+SIN(angle) ,, -COS(angle),, -SIN(angle),, +COS(angle)]
ZDelta = [-COS(angle),, -SIN(angle),, +COS(angle),, +SIN(angle)]

notice anything?
the same values of XDelta appear in ZDelta, but "rotated" -2
so we dont't even need two separate arrays, it can be the same array!
for X, we'll do array[e.which&6], and for Z we'll do array[e.which-2&6]
to save a bit more space, we can assign i=e.which&6 when we get X and reuse it when we get Z, with i-2&6
not only does the order of operations work favorably here with &6, the & operator will also convert negative numbers to positive numbers
this gives it an advantage over using % for array access

I also considered skipping the gaps and doing something like array[(e.which&6)/2], but that ended up longer so kept the gaps
in the final dweet, we could fit the event handler in the gaps with no problem

now you might notice the actual array is a bit different, and we do i+2&6 instead of i-2&6
this is where it gets confusing for me too
hell, Z and X might need to be swapped for all I know
and angle and left and right might also be swapped 
I had to really rearrange a bunch of stuff to ensure that the positives cos and sin appeared before the negatives
otherwise we'd have to do something like [a=-Sin(angle),..., -a]
which is longer than [a=Sin(angle),...,-a]

but we still use the same general idea with "rotating through an array"
the final array (with event handlers removed) looks like this:

J=[b=C(T),,a=S(T),,-b,,-a]

we can give it better variable names now and spacing
dwitter is nice in that C and S are predefined as Math.cos and Math.sin, saving a lot of chars here

let deltaArray = [cosAngle = Math.cos(angle), , sinAngle = Math.sin(angle), , -cosAngle, , -sinAngle];

now let's look at the keyhandler event again

onkeydown=e=>(X+=J[i=e.which&6]/9,Z+=J[i+2&6]/9);

why onkeydown instead of onkeyup?
onkeyup is shorter, but if you hold a key, it only fires once, when you let go
onkeydown fires continuously while you hold it, which is exactly what we want here

we can see the two array accesses with an offset for Z
note that the "zindex" is equal to (e.which&6)+2&6, which works out to be the same as e.which+2&6
with this in mind let's format it a bit better,

onkeydown = (keyEvent) => {
    let keyCode = keyEvent.which;
    let xDeltaIndex = keyCode & 6;
    let zDeltaIndex = keyCode + 2 & 6;
    let deltaScale = 1 / 9;
    
    xPos += deltaArray[xDeltaIndex] * deltaScale;
    zPos += deltaArray[zDeltaIndex] * deltaScale;
};


putting everything we have together:

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

yes this still works in dwitter

now let's move on to the body of the for loop

starting with R, this is very similar to what was "d" in the original 23034 dweet
it's kinda like "ray tracing depth"
it starts at a low value, increments until a certain condition is reached, then draws on the screen
depending on the specific condition, you get different patterns and different "walls" in the final scene

in the original dweet, to create these rays, what it did first was offset the ray from a bit to the left of the position, to a bit to the right of the position
then it marched forward in the exact angle that the "player" was facing
as a result it created a orthographic projection

unfortunatley I couldn't keep this as it required too many chars
instead I also used ray marching, but each ray started at the playes position (xPos, zPos).
then marched in a different direction depending on where on the screen we are
as a result the look is different, but pretty close

now as mentioned, R is predefined by dwitter as an RGB function
this is great, but we don't need color here, so we used R for this "ray depth" 
this allows us to skip "R=" while avoiding errors when using R in fillRect

now let's talk about how we march exactly, for which we'll need to define an angle to march in
this specific angle used here is

D = angle + j/canvasWidth -.5

angle we already know is the angle the player is facing
j/canvasWidth represents what "width fraction" of the screen we are on
we start at canvasWidth, and go down to 0. Thus this goes from 1 to 0
without the -.5, this means D will vary from angle + 1 down to angle
a bit off center, so we subtract -.5 and now D will vary from angle+.5 down to angle-.5
This gives us a nice 1 radian field of view, about 57 degrees.
Anything else would require additional chars

you migth notice there is no up/down or y coordinates.
That is because we are really moving in a 2d space that has a few effects to make it look 3d
so we only iterate on teh canvas x axis

now that we have our D, our modified angle corresponding to a direction to march in, how will we march?

from the position (X,Z), we will want to get to increment X with something times Cos(D), and Z with something times Sin(Z)
this something is R in this case, which allows for us to move some depth away from (X,Z)
in the actual dweet we do X + -R*Sin(D) and Z + R*Cos(D)
it just works, idk why it's -Sin and Cos

Now we can't just march forever
Once we have the X position of the ray and the Z position of the ray, we do an & operation.
In the original dweet, it was something (rayX|rayZ)%9>2, which made for a nicer pattern but I didn't have room
just a simple & operation is good enough here to make some rudimentary patterns

with all this, we can clean up hte for loop body 
we can also call j somethin g like displayX
Dwitters S & C functions really come in handy here too

let rayAngle = angle + displayX / canvasWidth - 0.5;
let rayX = xPos - R * Math.sin(rayAngle);
let rayZ = zPos + R * Math.cos(rayAngle);
let emptySpace = rayX & rayZ

R=emptySpace
?R+.1
:!x.fillRect(displayX--,540-1e3/R,2/R,canvasWidth/R)

by empty space I mean "not a wall"

now for the R= part
because R is previously set to a function, we cannot do R +=
So we do R=
In the case that we hit "emptySpace", we simply do R = R + .1
this will advance the depth by .1 units
this is strange, what if R is the function?
how will we ever increment R?
Well, if R is a function then rayX and rayZ will evaluate to NaN,which will evaluate emptySpace to 0, which will set R to !x.fillRect(...)
that will evaluate to true, allowing us to actually use it on the next iteration pretty much as R=1
this does mean that we ray march starting from 1 "unit" away from the player position, but we don't have any chars to spare 

that brings us to the final part
x.fillRect(displayX--,540-1e3/R,2/R,canvasWidth/R)

x is of course the context, a shorthand from dwitter
fillRect takes 4 parameters, x position of the top left corner, y position of the top left corner, width, and height
let's do one at a time

x position of the top left corner: displayX--
we already know displayX goes from 1927 down to 0
so this directly corresponds to the position on the screen itself, no tricks here

y position of the top left corner: 540-1e3/R
now this is where the "3D" comes in.
while not actually 3d, we draw the rectangles in such a way that closer "walls" appear higher and further walls appear shorter
to see exactly how, look at the height of this rectangle
it is canvasWidth/R
if you add the height to the y position, you get 540-1e3/R+canvasWidth/R, which is 540-1000/R+1927/R, which is 540+927/R
So the rectangle goes from 540-1e3/R to 540-927/R
So it is a rectangle of height 1927/R, roughtly centered on 540
540 is of course 1080/2, and 1080 is the default height
so it is roughly centered on the vertical middle, which is what you see in the scene

width: 2/R
this might be confusing, as we've already discussed that R starts at 1 and increments by +.1 
so this can have a max value of 2, and keep going down, even into values less than 1
while we can't draw a rectangle with a width between 1 and 0, what this will do instead is draw a rectangle of width 1, but with reduced opacity
so 2/R is effectively the opacity of the rectangle
Because the default fillStyle is black and we're drawing on a white background, this results in further walls (large R) being faint white, and closer walls (small R) being dark
i chose 2 instead of 1 just to make it a bit more opaque
3 doesn't look bad either

in the original, this was 1-1/R
this meant that closer walls would appear brighter, and further walls would appear darker.
while a cool lighting effect, unfortunately i could not spare any chars and had to flip this
it is now a fog effect, with further walls more faint and closer walls very dark
I've tried c.style.filter='invert()', and it does look pretty sick, but again we spare no characters

height: canvasWidth/R
this just works well with scaling, adn we already defined w = canvasWidth so it doesn't cost many characters to use w


in the actual dweet it is w/R, which is the shortest way to express this

the !x.fillRect(displayX--,540-1e3/R,2/R,canvasWidth/R) will only trigger once we hit a wall, meaning we only reset R to 1, and only decrement j when we hit a wall
so the for loop can loop many times without decrementing j

cleaning up the for loop body more:

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

and finally putting it all together:

onkeydown = (keyEvent) => {
    let keyCode = keyEvent.which;
    let xDeltaIndex = keyCode & 6;
    let zDeltaIndex = keyCode + 2 & 6;
    let deltaScale = 1 / 9;

    xPos += deltaArray[xDeltaIndex] * deltaScale;
    zPos += deltaArray[zDeltaIndex] * deltaScale;
};


putting everything we have together, and initializing rayDepth to R to match dwitter:

```
let time = t;
if (time === 0){
    zPos = 7;
    xPos = 7;
    angle = Math.tan;
    rayDepth = R;
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

for(let displayX = canvasWidth; displayX; ){
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

}
```

this is over 1100 characters, which is a bit outside dwitter boundaries
