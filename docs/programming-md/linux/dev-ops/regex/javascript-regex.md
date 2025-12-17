# Javascript Regex

The `s` flag allows `.` to match newline characters. [src](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions#advanced_searching_with_flags)

- `String.match(regex_patter)` nếu có capture group `()` thì sẽ return an array `arr`:
  * `arr[0] = entire match`
  * `arr[1] = first capture group` and so on.

`.match` by default will search with flag global & multi-line => This statement is not true. This is my wrong assumption!!

- `String.prototype.replace()`
  * nếu regex input có `g` flag thì replace all instance

## Template Literal

 Trong javascript, template string, khi gõ `enter` thì nó sẽ insert newline character `\n`.  
 Tuy nhiên nếu gõ `\e` rồi `enter` liền sau đó thì nó sẽ không insert `\n`.
 
```javascript
string = `\
     hogehoge
     `;
```