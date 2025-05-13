Let's understand why these design choices were made.

1. **Block Struct Fields (Not Pointers)**

```go
type Block struct {  
    Timestamp     int64  // Not a pointer    
    Data          []byte // Not a pointer    
    PrevBlockHash []byte // Not a pointer    
    Hash          []byte // Not a pointer
}
```

These fields are not pointers because:

- `int64` is a basic type that's small (8 bytes) - no need for pointers 
- `[]byte`is already a slice, which internally is a pointer-like structure
    - A slice contains: pointer to array, length, and capacity
    - When you pass a slice, you're already passing a reference-like structure
    - No need for additional pointer indirection

2. **Blockchain Struct (Using Pointer Slice)**

```go
type Blockchain struct {  
    Blocks []*Block // Slice of pointers to Block}
```

The is used because: `[]*Block`

1. **Memory Efficiency**:
    
    - Each Block instance might be relatively large
    - By storing pointers, we avoid copying entire Block structures when:
        - Appending to the slice
        - Reading from the slice
        - Passing blocks around in functions

2. **Shared References**:
    
    - Multiple parts of the code might need to reference the same Block
    - Using pointers ensures all references point to the same Block instance
    - Changes to a Block are visible everywhere that Block is referenced

Here's an illustration:

```go
// Without pointers (less efficient)
type Blockchain struct {  
    Blocks []Block // Each operation would copy entire Block structure}  

// With pointers (more efficient)
type Blockchain struct {  
    Blocks []*Block // Only copies pointer addresses}
```

Consider this example to understand the difference:

```go
// Example showing memory implications
func main() {  
    // Using pointers    
    blockchainWithPointers := &Blockchain{  
        Blocks: []*Block{  
            NewBlock("Transaction 1", []byte{}),  
            NewBlock("Transaction 2", []byte{}),  
        },  
    }  
      
    // Theoretical example without pointers (not actual implementation) 
    blockchainWithoutPointers := Blockchain{  
        Blocks: []Block{  
            *NewBlock("Transaction 1", []byte{}), // Would copy entire Block
            *NewBlock("Transaction 2", []byte{}), // Would copy entire Block
        },  
    }  
      
    // When adding new blocks:    
    // With pointers: only copies a small pointer    
    // Without pointers: would copy entire Block structure}
```

Key points to remember:

1. Use value types (non-pointers) when:
    - The type is small (like `int`, `bool`, small structs)
    - You want value semantics (copies are intentional)
    - The data is immutable

2. Use pointer types when:
    - The type is large (like complex structs)
    - You need shared ownership
    - You need to modify the original data
    - You need `nil` as a valid state

In the blockchain case, using is the right choice because: `[]*Block`

- Blocks are meant to be immutable once created
- Multiple parts of the system need to reference the same blocks
- It's more memory efficient
- It maintains the integrity of the blockchain (all references point to the same block)

This design pattern is common in Go when dealing with collections of larger structures or when you need to maintain shared references to the same instances.