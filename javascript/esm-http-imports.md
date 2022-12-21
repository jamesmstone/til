# ESM HTTP imports

When using all [modern versions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#browser_compatibility) of JS you can use esm modules.

What I did not realise is you can import modules at runtime over http(s).

To help with this there are a range of CDNs that host the whole of NPM designed for ESM.

Some examples include:
- https://esm.sh/
- https://esm.run/ - from: https://www.jsdelivr.com/


## Example

```js
import { version, verify } from 'https://esm.run/jsonwebtoken-esm';

console.log(version) // "8.5.1" at time of writing.
const token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwic2NvcGVzIjpbImEiLCJiIl0sImlhdCI6MTUxNjIzOTAyMn0.RYe2h0fD7XqWDxSepytntccG5-EfrdGmVmqwfhi36O0"
const decoded = verify(token, 'your-256-bit-secret');
console.log(decoded)
```