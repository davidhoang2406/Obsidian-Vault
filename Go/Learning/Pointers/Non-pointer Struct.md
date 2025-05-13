why non-pointer structs get copied in Go and how it works at a lower level.

1. **Value Types vs Reference Types**

```go
type Person struct {  
    Name    string  
    Age     int  
    Address string  
}  
  
// Example of copying
func main() {  
	person1 := Person{  
	    Name:    "Alice",  
	    Age:     30,  
	    Address: "123 Main St",  
	}  
	      
	// This creates a complete copy of all fields    
	person2 := person1  
	      
	// Modifying person2 doesn't affect person1    
	person2.Name = "Bob"  
	      
	fmt.Println(person1.Name) // Still prints "Alice"    
	fmt.Println(person2.Name) // Prints "Bob"
}
```

2. **Memory Layout**

```go
// When using values (copying occurs)
type Example struct {  
    Field1 int64    // 8 bytes    
    Field2 string   // 16 bytes (string header)    
    Field3 []byte   // 24 bytes (slice header)
}  
  
// When passing as value, all these bytes are copied:
func processStruct(e Example) {  
    // A new copy of Example is created with all its bytes
}  
  
// When using pointers (only address is copied)
func processStructPtr(e *Example) {  
    // Only 8 bytes (address) is copied
}
```

Here's why copying occurs:

1. **Memory Model in Go**:
    - Go uses a value-based memory model
    - When you assign a struct to a new variable, Go creates a new memory location
    - All fields are copied byte-by-byte to the new location

2. **Stack vs Heap**:

```go
func example() {  
    // Lives on the stack - fast but gets copied    
    person1 := Person{Name: "Alice"}  
      
    // Lives on the heap - only pointer is copied    
    person2 := &Person{Name: "Bob"}  
}
```

3. **Performance Implications**:

```go
type LargeStruct struct {  
    Data [1024]int // 8KB of data}  
  
func demonstrateCopy() {

    original := LargeStruct{}  
      
    // Expensive: Copies 8KB of data    
    copy1 := original  
      
    // Cheap: Copies only 8 bytes (pointer)    
    copy2 := &original  
}
```

4. **Real-world Example with Benchmarks**:

```go
package main  
  
import "testing"  
  
type BigStruct struct {  
    Data [1000]int64  
}  
  
func BenchmarkStructCopy(b *testing.B) {  
    original := BigStruct{}  
      
    b.Run("Value Copy", func(b *testing.B) {  
        for i := 0; i < b.N; i++ {  
            _ = original // Copies entire struct        }  
    })  
      
    b.Run("Pointer Copy", func(b *testing.B) {  
        for i := 0; i < b.N; i++ {  
            _ = &original // Copies only pointer        }  
    })  
}
```

5. **Special Cases - Slices and Maps**:

```go

type Container struct {  
    Items []int    // Slice header is copied, but underlying array is shared
    Data  map[string]int // Map header is copied, but data is shared
}  
  
func main() {  
    c1 := Container{  
        Items: []int{1, 2, 3},  
        Data:  map[string]int{"a": 1},  
    }  
      
    c2 := c1 // Copies the struct, but slices and maps share underlying data    
    c2.Items[0] = 99    // Affects both c1 and c2    
    c2.Data["a"] = 100  // Affects both c1 and c2
}
```

6. **When to Use Values vs Pointers**:

```go
// Use values for small structs
type Point struct {  
    X, Y int // 16 bytes total - okay to copy
}  
  
// Use pointers for large structs
type Image struct {  
    Pixels [][]uint8  // Could be very large    
    Width  int  
    Height int  
}
  
func processImage(img *Image) { // Use pointer to avoid copying    
	// Work with image
}

```


Understanding these concepts helps in:

1. **Performance Optimization**:
    - Use pointers for large structs to avoid expensive copies
    - Use values for small structs to avoid heap allocations

2. **Memory Management**:
    - Values are automatically cleaned up when they go out of scope
    - Pointers require garbage collection

3. **Data Sharing**:
    - Values provide data isolation
    - Pointers allow shared access and modification

4. **API Design**:
    - Methods that modify state should use pointer receivers
    - Methods that only read can use value receivers for small structs

This is why in your blockchain example:

```go
type Blockchain struct {  
    Blocks []*Block // Uses pointers because:                    
				    // 1. Blocks are relatively large                    
				    // 2. Multiple parts need to reference same block            
				    // 3. Avoid copying entire blocks when manipulating slice
}
```

The choice between value types and pointers significantly impacts both performance and behavior of your program, so it's important to choose appropriately based on your specific needs.
