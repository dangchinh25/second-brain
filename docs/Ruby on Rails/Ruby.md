# Library
- Ruby comes with a ton of functionality that is not available right away, so we need to `require` it as it stored in libraries.
> `require digest`
- Normally require statements should be placed at the very top of the file, so it is easy to see what libraries a particular piece of code (class) uses
# Modules
- Modules are somewhat similar to classes: they are things that hold methods, just like classes do but it can not be instantiated => no method `new`

## Mixin
- With modules you can share methods between classes: Modules can be included into classes, and this makes their methods available on the class, just as if we'd copied and paste these methods over to the class definition
- This is useful if we have methods that we want to reuse in certain classes, but also want to keep them in a central place, so we do not have to repeat them everywhere
![](https://i.imgur.com/TkCf3og.png)
- NOTES: Notice the keyword `include`, that is to make everything under a module available to the current context