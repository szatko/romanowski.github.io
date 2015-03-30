---
layout: post
title: "Values, values everywhere"
date: 2015-03-30 11:00:00
category: toolfriendly
tags: [toolfreidnly, scala]
featured_image: /images/cover.jpg
---

# Values, values everywhere

The basic feature of any language is a cvalue. It allows us to reuse the results of computation, names and many more. So how to use them to make our code more tool-friendly?

Let's look at a "real-world" example. We have to format and pretty print polish mobile numbers. Like this:

```scala
//takes number like 123456789 and format it as +48 123 456 789 
def format(nr: String) = nr.sliding(3).mkString("+48", " ", "")  
 
val numbers = Seq("+48730409203", "890890886", "700 440 675", "700 45 34 34")
```

Ok, it is trivial so 2 minutes of coding and we end up with:

```scala
numbers.map(_.replaceAll(" ", "")).map(_.replace("+48", "")).map(format)
```

And let's assume that we have some problems with one of replace, so let's see how our numbers look after first map.

And we can't do it. JVM store returns values direcly on stack and JDI has no direct access to stack.

Solution? Let's name what we get after each transformation. Debugging or even reading is now a pleasure :)

```scala
val withoutSpaces = numbers.map(_.replaceAll(" ", ""))
val withoutLocal = withoutSpaces.map(_.replace("+48", ""))
withoutLocal.map(format)
```
