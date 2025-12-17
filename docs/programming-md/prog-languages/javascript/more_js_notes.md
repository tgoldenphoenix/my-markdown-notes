# More Javascript Notes

## Tagged Template Literals

```javascript
function highlight(strings, ...values) {
  let str = '';
  strings.forEach((string, i) => {
    str += `${string} <span class='hl'>${values[i] || ''}</span>`;
  });
  return str;
}

const name = 'Snickers';
const age = '100';
// Tagged Template Literals
const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;
console.log(sentence);


/*
<style>
  .hl {
    background: #ffc600;
  }
</style>
*/
```