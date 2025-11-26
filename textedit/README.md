# Dwitter Text Editor

Inspired by the original dweet https://www.dwitter.net/d/34552

From the comments we had:

```js
t<1?s='\n':b='Backspace'
e='Enter'
x.reset()
onkeydown=k=>{o=k.key;l=o.length<2|o==b|o==e?k.key.replace(e,'\n'):'';l!=b?s+=l:s=s.slice(0,-1)}
x.font='2cm a'
h=s.split('\n')
for(i=h.length;i--;)x.fillText(h[i],0,60*i)
```

Clocking in at 216 bytes. This looked neat so I decided to see if we could make it shorter.

After a bit of moving variables around, modifying the key check a bit, I got this:

```js
t?e='Enter':s=''
x.reset()
onkeydown=k=>{
k.key=='Backspace'?(s=s.slice(0,-1)):s+=k.key==e?'\n':(k.key[1]?'':k.key)}
x.font='2cm a'
h=s.split('\n')
for(i=h.length;i--;)x.fillText(h[i],0,60+60*i)
```

This is 194 bytes, which is actually enough to compress to 140 bytes using the `eval(unescape(escape...))` stuff.
However, that didn't really feel in the spirit of things, so I decided to see if I could make it shorter.

After a lot more messing around, I got this at 138 bytes:
```js
t?onkeydown=k=>s={s:s.slice(x.reset(o=k.key),-1),r:s+`
`,t:s}[o[4]]||s+o:s=`
`
x.font='2cm a'
s.split`
`.map((t,i)=>x.fillText(t,0,i*60))
```
Available for demo at https://www.dwitter.net/d/34556

Now let's break it down.

## Newlines

To make it look nicer, the first trick you see here is using a backtick, an actual newline in the code, and another back tick instead of `'\n'`.
They are identical, but using backtick saves a byte.
Because we use it three times, we save three bytes here.

This little snippet also should be explained before we format this to be nicer.

```js
s.split`
`
```

This strange JS behavior allows us to skip the parenthesis when we call a method that accepts a string, as long as we use back ticks.
What is really going on here is equivalent to:

```js
s.split(`
`)
```

And an even more clear version:

```js
s.split('\n')
```

However, this is 13 characters vs the 10 characters you see in the final result.

With the newlines replaced, we have:

```js
t?onkeydown=k=>s={s:s.slice(x.reset(o=k.key),-1),r:s+'\n',t:s}[o[4]]||s+o:s='\n'
x.font='2cm a'
s.split('\n').map((t,i)=>x.fillText(t,0,i*60))
```

Now let's format it with indentation and spacing and semicolons:

```js
t
    ? onkeydown = k => 
        s = {
            s : s.slice(x.reset(o = k.key), -1),
            r : s + '\n',
            t : s
        }[o[4]] || s + o
    : s = '\n';

x.font = '2cm a';

s.split('\n').map((t, i) => 
    x.fillText(t,0,i*60)
);
```

## Initialization

Starting with the "else" branch of the first ternary operator.

```js
t
    ? ...
    : s = '\n';
```

`t` is the time in seconds since the Dweet started running.
`s` is the variable we will use to hold and transform our text in this text editor.
By assigning `s = '\n'` only when `t` evaluates to `false`, we will only set `s` to this value when `t` is `0`.
Meaning, we will set it to `\n` exactly once, when the Dweet starts running.

The event handler assignment in `true` branch of the ternary operator can actually be redefined on each frame of the Dweet.
So we can move it out of the ternary, and reformat the else branch like so:

```js
if (time === 0) {
    text = '\n';
}
```

## Keyboard Event Handling

That leaves us with the event handler defined in the `true` branch of the ternary operator.
This is where the most bytes were reduced to fit the Dweet under 140 bytes.

```js
onkeydown = k => 
    s = {
        s : s.slice(x.reset(o = k.key), -1),
        r : s + '\n',
        t : s
    }[o[4]] || s + o
```

Quite a few things going on here at once, and it took a while to find a way to fit all the functionality of the original into this.
I didn't even appreciate how the original was handling Backspaces, Enter, and even Shift keys until I dug into it.

The JS object you notice here is actually created each time the handler is called.
And the value for the `s` key is not a function, it is a value.
So that value is evaluated each time a keydown event happens.
You'll see why this works in our favor when we take a look at that value.

```js
s.slice(x.reset(o = k.key), -1),
```

If we split it out by order of operations, this is equivalent to:

```js
o = k.key;
resetResult = x.reset(o);
sliceResult = s.slice(resetResult, -1);
```

First off, assigning `k.key` to `o` is pretty straightforward.
https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key.
This value can look like `D`, or `Backspace`, or `Shift`, or `3`.
We use it later, and it saves us a few bytes to assign it to a variable here.

Then why do we pass `o` to `x.reset`?
Easy, it takes no parameters.
https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/reset.
So anything we pass will have no effect.
And the previously empty parenthesis of `x.reset()` looked like a good place to put save `k.key` to a variable without needing an extra newline, comma, or semicolon.

The `reset` method itself resets the context, clearing the canvas as you might expect.
I considered doing something like `c.width|=`, as is common on Dwitter, but this ended up needing more bytes.
As we reset the canvas on every keystroke, it means that when we backspace, we will clear the canvas, remove the last character from the string, and then redraw.
So the last character won't be on the canvas.

Then it gets weirder when we pass the result of `reset` to `s.slice`.
The original does `s.slice(0,-1)`.
The return value of `reset` is `undefined`, which luckily for us, `slice` handles as `0`.
So this is the same as `s.slice(0,-1)`.
The `-1` value means that the end index is 1 less than the last char of the string, so it's equal to `s.slice(0, s.length-1)`.
This is exactly what we want when we hit backspace, we want the string to end up being everything in the string except the last character of the string.

```
MDN: If indexEnd < 0, the index is counted from the end of the string
```

Let's rewrite the `onkeydown` function a bit:

```js
onkeydown = k => {
    const key = k.key;
    const resetResult = context.reset(key);
    const sliceResult = text.slice(resetResult, -1);

    text = {
        s : sliceResult
        r : s + '\n',
        t : s
    }[key[4]] || text + key
}
```

Alright now for the JS object itself.
This is simply a mapping of how to transform text if we have a Backspace, Enter, or Shift key.
If you look at the value of `k.key` for each of these, you have what you'd expect.
And if you take the character at index 4, you get unique value for each.

I could've also chosen index 1, 2, or 3 for this, as those positions are all unique in these three keys.
But not 0, you'll see why.

When we hit backspace, we want to delete characters, when we hit enter, we want to add a new line.
But you might also be wondering why we need to handle the Shift key?
Well, the way `onkeydown` works is, when you input a uppercase letter using the Shift key + the letter, you don't just get an event for the uppercase letter.
You also get an event for the shift key press.
The original comment filtered out Shift keys with specifically with the `o.length>2` part.

This might make the JS object more clear now.
The 5th character of `Backspace` is `s`, mapping to `sliceResult`.
If we hit backspace, we want to get everything but the last character.
The 5th character of `Enter` is `r`, mapping to `s + '\n'`.
We append a new line.
The 5th character of `Shift` is `t`, mapping to `s`.
Shift (by itself) won't do anything.


However, most of the key presses will get the value after the `||`.

```js
text = {...}[key[4]] || text + key
```

If you try to get `key[4]` when the `key` is something like `D`, just a normal letter, you will get `undefined`.
`undefined` is not a key in this JS object, which means that accessing the `undefined` key of the object will result in also `undefined`.
It is falsey, so this OR statement will evaluate to whatever is after the `||`.
In this case, we simply append the key to the text.

| Button                   |   Backspace    |   Enter    |   Shift    | D |
|:--------------------:|:------:|:------:|:------:|:------:|
| `k.key`            |   `Backspace`   |   `Enter`   |   `Shift`    |  `D`
| `k.key[4]`            |   `s`   |   `r`   |   `t`   | `undefined`
| Resulting `s`            |   Sliced s   |   s + `\n`   |   s | s + `key`

I chose a JS object to store this mapping as it was shorter than using ternary operators.
And we couldn't do something like `s+=...` as you cannot add anything to `s` to get the desired result of a backspace press.

Reorganizing this a bit better:

```js
const mapping = {
    s: sliceResult,
    r: text + '\n',
    t: text
};

const mappingKey = key[4];

const nextTextValue = mapping[mappingKey];
if (nextTextValue !== undefined) {
    text = nextTextValue;
} else {
    text = text + key;
}
```

## Font Size

This one is unchanged from the original dweet comment.

```js
x.font = '2cm a';
```


How it works exactly is kinda interesting.
If you look at the MDN documentation.
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/font.
It shows that font expects an argument like `ultra-condensed small-caps 1.2em "Fira Sans", sans-serif`.
The formal syntax is:

```
font =
  [ [ <'font-style'> || <font-variant-css2> || <'font-weight'> || <font-width-css3> ]? <'font-size'> [ / <'line-height'> ]? <'font-family'># ]  |
  <system-family-name>
```

What the fuck is this?
No idea.
But it seems that at least <'font-size'> and <'font-family'> are required.

So for font size, here is documentation on what you can provide: https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/font-size.
You might be familiar with something like `24px` or `80%` or `2rem`.
For our purposes, `2cm` looks good and is just 3 bytes.

For the font-family, somehow `a` resolves to a font.
I've also seen `x.font="2in'"` as in https://www.dwitter.net/d/34414.
I could've saved a byte by doing `x.font="2cm'"`, but I was testing this on Safari, where that has no effect on the font size, and results in tiny text.
So `2cm a` it is.

This is outside of the `onkeydown` handler because whenever `x.reset()` is called, we need to set the font again.
Otherwise the fillText will appear tiny.
By setting `x.font` to this value every time the dweet function is called, we ensure the text stays the right size.

2cm is a fine size, and the `a` font family rendered fine on my browser, so I stuck with it.
Any modifications to this messed up the font, making the text tiny or nonexistent.
So I left it as is.

CanvasRenderingContext2D has no `fontSize` or `fontFamily` properties, just `font`, so this is the the clearest I can make this line.

```js
context.font = '2cm a';
```

## Text Rendering

Now the last part, render the text.

```js
s.split('\n').map((t, i) => 
    x.fillText(t, 0, i * 60)
);
```

Functionally this is very close to the original text rendering part in the dweet comment.

```js
h=s.split('\n')
for(i=h.length;i--;)x.fillText(h[i],0,60*i)
```

The thing about `fillText` is that it does not handle newlines as you'd expect.
https://stackoverflow.com/questions/5026961/html5-canvas-ctx-filltext-wont-do-line-breaks.
So we need to make a separate `fillText` call for each new line.
Since `s` is a string that just keeps getting appended to, rather than an array, we first need to split it by newline.

```js
s.split('\n')
```

The nice thing about map is that it will execute the function you pass to it, you don't even need to return a value.
And you can even get an index argument.

```js
.map((t, i) => ...)
```

Once we've split on new lines, `t` will be each individual line, and `i` will be the index of that line.
`fillText` can take 3 parameters, the text, the x coordinate, and the y coordinate.
We use the exact same coordinates used in the original, `0` to align it to the left of the screen, and `60 * i` to give a nice spacing that works well with `2cm`.

We can give better variable names and split out this part for clarity, and use an equivalent `for` loop.

```js
const lineSpacing = 60;

const lines = text.split('\n');

for (let i = 0; i < lines.length; i++){
    const line = lines[i];
    const xPosition = 0;
    const yPosition = lineSpacing * i;

    context.fillText(line, xPosition, yPosition);
}
```

## Final Result

Putting it all together, we get:


```js
const time = t;
const context = x;

if (time === 0) {
    text = '\n';
}

onkeydown = (keyEvent) => {
    const key = keyEvent.key;
    const resetResult = context.reset(key);
    const sliceResult = text.slice(resetResult, -1);

    const mapping = {
        s: sliceResult,
        r: text + '\n',
        t: text
    };

    const mappingKey = key[4];

    const nextTextValue = mapping[mappingKey];
    if (nextTextValue !== undefined) {
        text = nextTextValue;
    } else {
        text = text + key;
    }
}


context.font = '2cm a';

const lineSpacing = 60;

const lines = text.split('\n');

for (let i = 0; i < lines.length; i++) {
    const line = lines[i];
    const xPosition = 0;
    const yPosition = lineSpacing * i;

    context.fillText(line, xPosition, yPosition);
}
```

Which will run fine on dwitter.
