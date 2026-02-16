# Custom operations on slices

## Removing

```go
func remove(slice []string, index int) []string {
	slice = append(slice[:index], slice[index+1:]...)
	return slice
}
```

## Adding

```go
func add(slice []string, index int, value string) []string {
	rightPart := append([]string{value}, slice[index:]...)
	slice = append(slice[:index], rightPart...)
	return slice
}
```

## Filtering
```go
func filter(slice []int) []int {
    filtered := []int{}
    for _, elem := range slice {
        if elem > 0 { // filtering condition
            filtered = append(filtered, elem)
        }
    }
    return filtered
}
```