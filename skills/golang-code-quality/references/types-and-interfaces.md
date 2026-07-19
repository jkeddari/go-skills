# Structs and interfaces

## Interfaces

- Define interfaces where they are consumed, not beside the implementation by default.
- Keep interfaces focused on one capability; compose small interfaces when a caller needs several capabilities.
- Accept an interface only when the caller benefits from substitution. Return a concrete type unless abstraction is part of the contract.
- Add a compile-time assertion when interface satisfaction is important and otherwise non-obvious:

```go
var _ io.Reader = (*Buffer)(nil)
```

- Use the comma-ok form for assertions that can legitimately fail.

```go
v, ok := value.(Validator)
if !ok {
    return errors.New("value does not support validation")
}
```

## Structs

- Prefer named fields over embedding when forwarding the embedded API would expose unwanted behavior.
- Make ownership of slices, maps, channels, and closers explicit.
- Keep serialization structs at boundaries when their tags or optionality do not match the domain model.
- Avoid copying structs containing mutexes, atomics, or other stateful synchronization values after first use.

## Receiver choice

Use a pointer receiver when the method mutates state, the value is large, identity matters, or the type must not be copied. Use a value receiver for small immutable values. Keep receiver choice consistent across the method set unless semantics justify the difference.

## Zero values

Make the zero value useful when that contract is simple and unsurprising. Otherwise expose a constructor and make invalid states difficult to represent.
