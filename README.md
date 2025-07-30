# Wordleeeeeeee

## How to play
You have 10 attempts to guess a 3 (thats e rounded to the nearest whole number) character sequence of variations of the letter e. You are not without help. If a letter is in the sequence, it will be highlighted in yellow. If a letter is in the correct place in the sequence. If a letter is not in the sequence, it will be highlighted grey.

## Why this game is different from most (kind of)
This game is basically completly based on state. That means the only actions (onclick) I had to write were setting state.

For example, here is the code for the `Key` component, that shows up when you type:
```jsx
<div
  class={clsx(
      'flex size-15 items-center justify-center rounded border text-2xl',
      attempts[attempt] &&
        attempts[attempt][letter] === word[letter] &&
        'bg-green-400/25',
      attempts[attempt] &&
        attempts[attempt][letter] !== word[letter] &&
        word.includes(attempts[attempt][letter]) &&
        'bg-yellow-400/25',

      attempts[attempt] && attempts[attempt][letter] !== word[letter] && 'bg-gray-400/25'
  )}
>
  {attempt == current ? currentText[letter] : attempts[attempt] && attempts[attempt][letter]}
</div>
```
Sure, its not good code but it works, and the one rule of programming is if it works, DONT TOUCH IT.
