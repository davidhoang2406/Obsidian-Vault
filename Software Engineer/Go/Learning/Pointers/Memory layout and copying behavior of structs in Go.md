1. **When using value types (struct copying):**

```go
// Initial struct creation
person1 := Person{Name: "Alice", Age: 30}

Memory Layout:
┌─────────── person1 ──────────┐
│ Memory Location: 0x001       │
├────────────┬─────────────────┤
│ Name       │ "Alice"         │
├────────────┼─────────────────┤
│ Age        │ 30              │
└────────────┴─────────────────┘

// Assigning to new variable
person2 := person1

Updated Memory Layout:
┌─────────── person1 ──────────┐  ┌─────────── person2 ──────────┐
│ Memory Location: 0x001       │  │ Memory Location: 0x002       │
├────────────┬─────────────────┤  ├────────────┬─────────────────┤
│ Name       │ "Alice"         │  │ Name       │ "Alice"         │
├────────────┼─────────────────┤  ├────────────┼─────────────────┤
│ Age        │ 30              │  │ Age        │ 30              │
└────────────┴─────────────────┘  └────────────┴─────────────────┘

// After modifying person2
person2.Name = "Bob"

Final Memory Layout:
┌─────────── person1 ──────────┐  ┌─────────── person2 ──────────┐
│ Memory Location: 0x001       │  │ Memory Location: 0x002       │
├────────────┬─────────────────┤  ├────────────┬─────────────────┤
│ Name       │ "Alice"         │  │ Name       │ "Bob"           │
├────────────┼─────────────────┤  ├────────────┼─────────────────┤
│ Age        │ 30              │  │ Age        │ 30              │
└────────────┴─────────────────┘  └────────────┴─────────────────┘
```

2. **When using pointers:**

```go
// Initial pointer struct creation
person1 := &Person{Name: "Alice", Age: 30}

Memory Layout:
┌─────────── person1 ──────────┐
│ Pointer: 0x001               │──┐
└──────────────────────────────┘  │
                                  ▼
                        ┌──── Person Data ──────┐
                        │ Memory Location: 0x100│
                        ├────────────┬──────────┤
                        │ Name       │ "Alice"  │
                        ├────────────┼──────────┤
                        │ Age        │ 30       │
                        └────────────┴──────────┘

// Assigning to new variable
person2 := person1

Updated Memory Layout:
┌─────────── person1 ──────────┐
│ Pointer: 0x001               │──┐
└──────────────────────────────┘  │
                                  ▼
┌─────────── person2 ──────────┐  ┌──── Person Data ────────┐
│ Pointer: 0x001               │──┘ │ Memory Location: 0x100│
└──────────────────────────────┘    ├─────────────┬─────────┤
                                    │ Name        │ "Alice" │
                                    ├─────────────┼─────────┤
                                    │ Age         │ 30      │
                                    └─────────────┴─────────┘

// After modifying through person2
person2.Name = "Bob"

Final Memory Layout:
┌─────────── person1 ──────────┐
│ Pointer: 0x001               │──┐
└──────────────────────────────┘  │
                                  ▼
┌─────────── person2 ──────────┐  ┌──── Person Data ────────┐
│ Pointer: 0x001               │──┘ │ Memory Location: 0x100│
└──────────────────────────────┘    ├────────────┬──────────┤
                                    │ Name       │ "Bob"    │
                                    ├────────────┼──────────┤
                                    │ Age        │ 30       │
                                    └────────────┴──────────┘
```

Key differences to note:

1. **Value Types (first diagram)**:
    - Complete copy of data
    - Changes to one don't affect the other
    - More memory usage
    - No shared state

2. **Pointer Types (second diagram)**:
    - Only pointer address is copied
    - Changes through either pointer affect the same data
    - Less memory usage
    - Shared state

This is why in the blockchain example, using is more efficient than `[]Block`: `[]*Block`

```go
// With []*Block (efficient):
┌─── Blockchain ────┐
│ Blocks slice      │
├──────┬────────────┤      ┌──── Block1 Data ───┐
│ Ptr1 │ 0x001      │─────►│ Hash, Timestamp,   │
├──────┼────────────┤      │ Data, etc.         │
│ Ptr2 │ 0x002      │─┐    └────────────────────┘
├──────┼────────────┤ │    ┌──── Block2 Data ───┐
│ Ptr3 │ 0x003      │─┼───►│ Hash, Timestamp,   │
└──────┴────────────┘ │    │ Data, etc.         │
                      │    └────────────────────┘
                      │    ┌──── Block3 Data ───┐
                      └───►│ Hash, Timestamp,   │
                           │ Data, etc.         │
                           └────────────────────┘
```

This design is memory-efficient because:
1. Only pointers are stored in the slice
2. Block data is stored once and shared
3. Adding/removing blocks only involves manipulating pointers
4. No need to copy entire block structures when modifying the slice