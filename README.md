# JSDoc 3.5.5 with NodeJS 8.3.0+ strange bug

This repository is a minimal reproduction for a strange bug.

# What's the bug

1. The name of the generated file is not consistent depending on subtle changes.

   Using this code as is, the file is named `out/Abcdefghijklm_.html`, note the underscore before the extension.
   
2. Same-page anchor links do not use the same file name before the `#`, which leads to 404s (as `out/Abcdefghijklm.html` does not exist).

   This project does not exhibit this, but I believe fixing the above symptom would fix that issue as well. I have a private project to test this.

# How to reproduce

Use NodeJS version 8.3.0 (NPM 5.3.0) or above.

1. Clone this project
2. Run `npm run test`
3. If the `out/` folder contains an `.html` file with an `_` before the extension, it the output ends with `BUG`, otherwise it ends with `Good!`

If it changes anything, I'm running Debian Jessie 8.9, but I used `nvm` to switch between NodeJS versions.

# What changes would make this pass

Any single item of the following list makes the test pass.

1. Use NodeJS version 8.2.1 (NPM 5.3.0) or less
2. Change the file order: `mv src/a.js src/c.js`
3. Change the namespace name length: `sed -i 's/Abcdefghijklm/Abcdefghijkl/g' src/*`
4. Remove the accent in the string value of `var a` in `src/a.js`
5. Remove the `@type` line of `var b` in `src/a.js`
6. Remove the empty doc comment of `Abcdefghijklm.prototype.Enum` in `src/b.js`

It could be interesting to test with older versions of JSDoc too, but we need to backport the following fix: jsdoc3/jsdoc@01192fad9440502f13e2453594f8f9b62fae7639.
