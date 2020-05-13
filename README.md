# Firefly
*Kiss subtyping goodbye!*

A simple and safe alternative to Java.

*Firefly combines a small set of simple features -- all of which were invented several decades ago.*

**This is a draft -- claims made below are merely tentative goals.**


# Firefly is safe

## Problem: Ambient Authority

In most programming languages, every piece of code can silently read from your file system, access the internet, and generally misbehave. It's not feasible to vet all the dependencies that a modern application might have. Is the `BCrypt` implementation you depend on secretly sending your passwords somewhere? Have you checked?

## Solution: Object Capabilities

```
function main(system: System): Unit {
  copyFile(system.files, "in.txt", "out.txt")
}

function copyFile(fs: FileSystem, in: String, out: String) {
  let bytes = fs.readBytes(in)
  fs.writeBytes(out, bytes)
}
```

In Firefly, the `main` function gets an instance of `System`, which allows you to do anything, and you can then delegate responsibility to other functions by passing them either `system` or one of its fields, such as `system.files`, which only lets you access the file system. Other examples are `system.network` and `system.environment` etc. Firefly has no global state through which such capabilities can leak.

## Bonus: Reliable Functions

Because there is no global state, all functions are pure unless they are passed or close over impure objects. That means they will reliably return the same value every time they're passed the same arguments. It's a powerful tool for building reliable and predictable software.


# Firefly is simple

## Problem: Contravariance & Deep Hierarchies



## Solution: Replace Subtyping




# Firefly is powerful

## Problem: Overloading

## Solution: Type Classes


# Firefly is portable

## Problem: Poor Language Interoperability

## Solution: Multiple Targets

