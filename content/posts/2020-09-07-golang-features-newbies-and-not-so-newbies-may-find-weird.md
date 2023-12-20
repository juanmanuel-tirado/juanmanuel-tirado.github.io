---
categories:
- Golang
- Programming
date: "2020-09-07T16:41:05Z"
excerpt: Humans are wired for prejudice. When we start coding in a programming language
  we do not feel comfortable. We always point out those features that annoy us the
  most. Sometimes it is difficult to put these features into the corresponding context
  and understanding the reasons why they were designed in such way. I enumerate some
  of the features I found more weirder when starting my Go experience.
tags:
- golang
- programming
title: Golang features newbies (and not so newbies) may find weird
url: /golang-features-newbies-and-not-so-newbies-may-find-weird/
---
It has been three years now since I started coding in Go. I think it is time for a bit of retrospective and I would like to enumerate some of the aspects I found most "weird" during my Golang learning process.

Before we start and just in case you fell into this post by accident, some context:

## What is Go?
[Go](https://golang.org/ "Go") is a programming language developed by Google. Similar to C, it is compiled, statically typed with garbage collection, memory safety and with a special focus on concurrency and networking. The language is deeply used at Google to develop from services to complete solutions. Actually, [Kubernetes](https://kubernetes.io/ "Kubernetes") is developed using Go.

## Why did I start using Go?
I started designing a smart orchestrator for multi-cloud applications. The target platform was Kubernetes, so it seemed to be the perfect match for the project.

## Go features newbies (and not so newbies)  may find weird
Humans are wired for prejudice. When we start coding in a programming language we do not feel comfortable. We always point out those features that annoy us the most. Sometimes it is difficult to put these features into the corresponding context and understanding the reasons why they were designed in such way. I enumerate some of the features I found weirder when starting my Go experience.

In my case, I have experience in a bunch of languages: C, C++, Java, Scala, Python, R (if we can consider R a language) among others. In general, I did not find my transition difficult. The syntax is straight forward, and the language is not very verbose (in principle). However, I found some frustration with the following features.

**Unnecessary imports and variables**
Go forces the code to be minimalistic. This implies that unnused imports and variables trigger compilation errors. For example:
```golang
import (
	"fmt"
	"os" //not used
)

func main() {
	fmt.Println("Hola")
}
```
The compiler returns
```bash
imported and not used: "os"
```

**Iterating collections**
The range function used to iterate collections returns two values. The first is the position of the entry in the collection and the second value contains the entry value itself.
```go
	x := [4]string{"one","two","three","four"}
	for i, entry := range(x) {
	   fmt.Printf("Element at position %d is %s\n", i, entry)
	}
```
This is very handy because you have in every iteration the two most common values you may use in your loops. However, they are not always required. This means that you will do something like:
```go
	x := [4]string{"one","two","three","four"}
	for i, entry := range(x) {
	   fmt.Printf("Element %s\n", entry)
	}
```
Which returns an error during compilation:
```bash
i declared but not used
```
Or even worse, you will skip the i variable like this:
```golang
    x := [4]string{"one","two","three","four"}
    for entry := range(x) {
       fmt.Printf("Element %s\n", entry)
    }
```
Which can be really confusing because it returns the position value in a variable we expected to be the entry value.
```bash
Element %!s(int=0)
Element %!s(int=1)
Element %!s(int=2)
Element %!s(int=3)
```

We simply have to indicate an unnused variable i.
```go
	x := [4]string{"one","two","three","four"}
	for _, entry := range(x) {
	   fmt.Printf("Element %s\n", entry)
	}
```
**Attributes visibility**
Attributes are visible if they start with an uppercase letter. If no, they are private. It is simple. However, I do regularly forget about this resulting in stupid errors.

```go
type Message struct {
 Text string // This is public
 text string // This is private
}
```

**What happened to overloaded methods?**
If you come from the Java world you are probably used to overload methods. This is, for a method we can have several signatures.  Well... Golang has no method overloading.

**What happened to inheritance?**
There is no inheritance. Just that. You can do some workaround like the one  described [here](https://golangbot.com/inheritance/), but we cannot really say that is inheritance.

**What about interfaces?**
There are interfaces. They can be defined as a collection of method signatures. However, they are "weird" in the sense you use them in other languages. Why? Because you do not programatically indicate that your struct implements an interface (something like class A implements interface I). Your struct fulfils an interface if it has the methods enumerated by the interface. It is easier to understand with an example.

```golang
package main

import (
	"fmt"
)

type Speaker interface {
    SayYourName() string
    SayHello(b Speaker) string
}

type HappySpeaker struct {}

func(hs HappySpeaker) SayYourName() string {
    return "Happy"
}

func(hs HappySpeaker) SayHello(b Speaker) string {
    return fmt.Sprintf("Hello %s!",b.SayYourName())
}

type AngrySpeaker struct {}

func(as AngrySpeaker) SayYourName() string {
    return "Angry"
}

func(as AngrySpeaker) SayHello(b Speaker) string {
    return fmt.Sprintf("I'm not going to say hello to %s!",b.SayYourName())
}

func main() {
	// We have two different structs
	happy := HappySpeaker{}
	angry := AngrySpeaker{}
	// they can say their names
	fmt.Println(happy.SayYourName())
	fmt.Println(angry.SayYourName())
	
	// But they are also speakers
	fmt.Println(happy.SayHello(angry))
	fmt.Println(angry.SayHello(happy))
	
	// This is also valid
	var mrSpeaker Speaker = happy
	fmt.Println(mrSpeaker.SayHello(angry))
}
```
As you can imagine this has larger implications when coding. Interfaces in Go are a much deeper topic for discussion and you can find a lot of examples defining pros and cons for this approach.

**What about constructors?**
There are no constructors like the ones you may find in any object oriented language. Structs definition resembles a lot the one used in C. With one potential issue: you can skip attributes setting when instantiating a new struct. In the following code  `halfMessage1`and `halfMessage2`have unset attributes. 

```go
package main

import (
	"fmt"
)

type Message struct {
	MsgA string
	MsgB string
}

func(m Message) SayIt() {
  fmt.Printf("[%s] - [%s]\n",m.MsgA, m.MsgB)
}


func main() {
	
	fullMessage1 := Message{"hello","bye"}
	fullMessage2 := Message{MsgA: "hello", MsgB: "bye"}
	
	halfMessage1 := Message{"hello",""}
	halfMessage2 := Message{MsgA: "hello"}
	
	emptyMessage := Message{}
	
	fullMessage1.SayIt()
	fullMessage2.SayIt()
	halfMessage1.SayIt()
	halfMessage2.SayIt()	
	emptyMessage.SayIt()		
}
```
The output is:
```bash
[hello] - [bye]
[hello] - [bye]
[hello] - []
[hello] - []
[] - []
```
This is always a potential issue because you can have methods expecting values to be set. A way to mitigate this is to define your own static "constructors".

```golang
package main

import (
	"fmt"
)

type Message struct {
	MsgA string
	MsgB string
}

func(m Message) SayIt() {
  fmt.Printf("[%s] - [%s]\n",m.MsgA, m.MsgB)
}

func NewMessage(msgA string, msgB string) *Message{
  if len(msgA) * len(msgB) == 0 {
     return nil
  } 
  return &Message{MsgA: msgA, MsgB: msgB}
}

func main() {
   // A correct message
   msg1 := NewMessage("hello","bye")	
   if msg1 != nil {
      msg1.SayIt()
   } else {
      fmt.Println("There was an error")
   }
   	
	
   // An incorrect message
   msg2 := NewMessage("","")
   
   if msg2 != nil {
      msg2.SayIt()
   } else {
      fmt.Println("There was an error")
   }
}
```

## Summary
This is a small excerpt of all the potential aspects you have to consider when conding in Go. I would like to hear from your experiences? What are the Go features you find the most weird?
