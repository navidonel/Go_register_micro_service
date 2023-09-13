package main

import (
    "database/sql"
    "fmt"
    "net/http"
    "strings"

    _ "github.com/go-sql-driver/mysql"
)

func main() {
    // Open a connection to the database
    db, err := sql.Open("mysql", "root:password@tcp(localhost:3306)/register")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer db.Close()

    // Create a handler for the `/register` endpoint
    http.HandleFunc("/register", func(w http.ResponseWriter, r *http.Request) {
        // Get the user information from the request
        username := r.FormValue("username")
        password := r.FormValue("password")
        email := r.FormValue("email")
        phone := r.FormValue("phone")

        // Validate the user information
        if username == "" || password == "" || email == "" || phone == "" {
            w.WriteHeader(http.StatusBadRequest)
            fmt.Fprintf(w, "Please provide all required information.")
            return
        }

        // Insert the user information into the database
        stmt, err := db.Prepare("INSERT INTO users (username, password, email, phone) VALUES (?, ?, ?, ?)")
        if err != nil {
            fmt.Println(err)
            return
        }
        defer stmt.Close()

        _, err = stmt.Exec(username, password, email, phone)
        if err != nil {
            fmt.Println(err)
            return
        }

        // Redirect the user to the home page
        w.WriteHeader(http.StatusCreated)
        fmt.Fprintf(w, "User created successfully.")
    })

    // Listen for requests on port 8080
    http.ListenAndServe(":8080", nil)
}
