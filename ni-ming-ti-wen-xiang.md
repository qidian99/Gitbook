# 匿名提问箱

Refer to the website [tapechat.net](https://www.tapechat.net/)

## Tech stack

1. Backend: Golang
2. Front-end: Vue
3. iOS: SwiftUI

## Research

1. Charles proxy parse network requests and API calls
   1. GET My Post
   2. GET Refresh Posts
   3. GET Feed \(random posts\)
   4. GET Post Detail
   5. GET Follow list
   6. GET Fan list
   7. GET Channels
   8. GET My Card
   9. GET Box Setting
   10. GET All Messages \(by type\)
   11. GET Random Music
   12. GET Me
   13. GET User Box
   14. GET Picture \(aliyun\)
   15. POST Can Private Chat
       1. 估计是用户自己设定
   16. POST Follow User
   17. POST Unfollow User
   18. POST Blacklist User
   19. POST Random Card
   20. POST Report a Post/User
   21. POST Device Token
   22. POST User Box
   23. POST Change User Profile
   24. POST Create Group Chat
   25. POST Create Channel
   26. POST User Box Customize
2. Postman Collection

   1. [https://www.getpostman.com/collections/6da052bc792f778a8ba8](https://www.getpostman.com/collections/6da052bc792f778a8ba8)

## Golang

1. Course: [https://www.udemy.com/course/go-the-complete-developers-guide/learn/lecture/7797276\#overview](https://www.udemy.com/course/go-the-complete-developers-guide/learn/lecture/7797276#overview)
   1. Package

      ```go
      package main
      ```

   2. Import

      ```go
      import fmt
      ```

   3. Function
      1. Without return value

         ```go
         func main () {
             fmt.Println("Hello World")
         }
         ```

      2. Return multiple values from one function

         ```go
         func deal(d deck, handSize int) (deck, deck) {
             return d[:handSize], d[handSize:]
         }
         ```

         ```go

         ```
   4. Compile

      ```text
      go run/build main.go
      ```

   5. Flow control
      1. For loops

         ```go
         for idx, value := range list {
             fmt.Println(idx, value)
         }
         ```



         Ignore the index in iteration

         ```go
         for _, suit := range cardSuits {
         	for _, value := range cardValues {
         		// Create a new string
         		cards = append(cards, value+" of "+suit)
         	}
         }
         ```

         Loop for certain times

         ```go
         for i:=1; i<10; i++ {
           // do something
         }
         ```
   6. Data Structure
      1. Slice
         1. Initialize

            ```go
            cards := []string{newCard(), newCard()}
            cardSuits := []string{"Spades", "Diamonds", "Hearts", "Clubs"}
            cardValues := []string{"Ace", "Two", "Three", "Four"}
            ```

         2. Append

            ```go
            append(cards, value+" of "+suit)
            ```

         3. Slice

            ```go
            cards[:3]

            card[3:]
            ```
      2. Struct
         1. Type definition

            ```go
            type Person struct {
            	firstName string
            	lastName  string
            }
            ```

         2. Instantiation

            ```go
            person := Person{
                firstName: "a",
                lastName:  "b",
            }

            	person := Person{"a", "b"}
            ```
   7. Interface
      1. Implication
         1. To make function able to able various input types
      2. Problem without interfaces

         ```go
         package main

         import "fmt"

         type englishBot struct {
         	greeting string
         }

         type spanishBot struct {
         	greeting string
         }

         func main() {

         }

         func (e englishBot) getGreeting() string {
         	return e.greeting
         }

         func (s spanishBot) getGreeting() string {
         	return s.greeting
         }

         func printGreeting(eb englishBot) {
         	fmt.Println(eb.getGreeting())
         }

         func printGreeting(sb spanishBot) {
         	fmt.Println(sb.getGreeting())
         }
         ```

         ```go
         package main

         import "fmt"

         type bot interface {
         	getGreeting() string
         }

         type englishBot struct {
         }

         type spanishBot struct {
         }

         func main() {

         	eb := englishBot{}
         	sb := spanishBot{}

         	printGreeting(eb)
         	printGreeting(sb)

         }

         func (englishBot) getGreeting() string {
         	return "Hi There!"
         }

         func (spanishBot) getGreeting() string {
         	return "Hola"
         }

         func printGreeting(b bot) {
         	fmt.Println(b.getGreeting())
         }

         ```

      3. Interfaces are not generic types
      4. Interfaces are implicit
      5. Interfaces are a contract to help us manage types
      6. Interfaces are tough
   8. Types
      1. Basic Go Types: string, integer, float, array, map
         1. Zero values: `string` "", `int` 0, `float` 0, `bool` false
      2. Extend a base type and add some extras functionalities to it
         1. type deck \[\]string

            ```go
            package main

            // Create a new type of 'deck'
            // which is a slice of strings
            type deck []string
            ```

         2. Compile multiple files

            ```text
            go run main.go deck.go
            ```

         3. Receiver

            ```go
            package main

            import "fmt"

            // Create a new type of 'deck'
            // which is a list of strings
            type deck []string

            // Receiver on a function
            func (d deck) print() {
            	for i, card := range d {
            		fmt.Println(i, card)
            	}
            }

            ```

            The `d` is similar to `this` or `self` keywords in other language. It is usually an abbreviation of several characters

         4. All the functions we need to have in the deck of cards
            1. `newDeck()`: create and return a list of playing cards. Essentially an array of strings
               1. No need to have receiver `blabla`.`method_name`
            2. `print()`: Log out the contents of a deck of cards
            3. `shuffle()`: Shuffles all the cards in a deck
               1. rand package
                  1. intn: not randomized correctly
                  2. pseudonumber generator
                     1. 
               2. 
            4. `deal()`: Create a hand of cards
            5. `saveToFile()`: Save a list of cards to a file on the local machine
               1. Should make use of a built in package, `ioutil`: [https://golang.org/pkg/io/ioutil/](https://golang.org/pkg/io/ioutil/)
                  1. `WriteFile(filename string, data []byte, perm os.FileMode) error`
                     1. What is a byte slice 
                     2. How to convert a slice of strings to a slice of bytes
                        1. Type conversion
               2. `strings`
                  1. `func Join(a []string, sep string) string`
                  2. 
            6. `newDeckFromFile()`: Build a deck from a file on the local machine
   9. Testing

      1. To make a test, create a new file ending in \_test.go
      2. To run all tests, run `go test`

      ```go
      package main

      import "testing"

      func TestNewDeck(t *testing.T) {
      	d := newDeck()

      	if len(d) != 2000 {
      		t.Errorf("Expected deck length of 20, but got %v", len(d))
      	}
      }
      ```

   10. Pointers
       1. Go passes by value
       2. Gotchas

          ```go
          package main

          import (
          	"fmt"
          )

          func main() {
          	fmt.Println("Hello, playground")
          	mySlice := []string{"Hi","There", "How", "Are", "You"}
          	updateSlice(mySlice)
          	fmt.Println(mySlice)
	
	
          }


          func updateSlice(s []string) {
          	s[0] = "Bye"
          }

          ```
   11. Misc

       ```go
       fmt.Printf("%+v", person) // every key + value, e.g., {firstName:a lastName:b}
       ```
2. How to develop APIs using Go
3. 
## Golang Q&A

![](.gitbook/assets/image%20%2863%29.png)

![](.gitbook/assets/image%20%2864%29.png)

## VueJS \(Quasar\)

1. How to develop responsive UI using VueJS
2. 
## APIs

1. Login
   1. Third-party
   2. Username/password authentication
2. Post an anonymous quest



