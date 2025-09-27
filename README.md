package main

import (
    "encoding/json"
    "fmt"
    "log"
)

type User struct {
    Username string `json:"username"`
    Email    string `json:"email"`
    Age      int    `json:"age"`
}

// Custom JSON validation
func (u *User) Validate() error {
    if u.Username == "" {
        return fmt.Errorf("username is required")
    }
    if u.Age < 0 || u.Age > 150 {
        return fmt.Errorf("invalid age: %d", u.Age)
    }
    return nil
}

func main() {
    // Valid JSON
    validJSON := `{"username":"john_doe","email":"john@example.com","age":30}`
    
    var user User
    if err := json.Unmarshal([]byte(validJSON), &user); err != nil {
        log.Fatal("JSON parse error:", err)
    }
    
    if err := user.Validate(); err != nil {
        log.Fatal("Validation error:", err)
    }
    
    fmt.Printf("Valid user: %+v\n", user)
    
    // Invalid JSON
    invalidJSON := `{"username":"","email":"invalid","age":200}`
    
    var invalidUser User
    if err := json.Unmarshal([]byte(invalidJSON), &invalidUser); err != nil {
        log.Fatal("JSON parse error:", err)
    }
    
    if err := invalidUser.Validate(); err != nil {
        fmt.Println("Caught validation error:", err)
    }
}
