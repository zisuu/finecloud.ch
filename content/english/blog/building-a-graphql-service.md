---
title: "Building a GraphQL Service"
meta_title: "Building a GraphQL Service"
description: "Let's build a GraphQL Spring Application that will accept GraphQL requests at http://localhost:8080/graphql. First, let's navigate to https://start.spring.io. This service pulls in all the dependencies you need for an application and does most of the setup for you. GraphQL is a query language to retrieve data efficiently."
date: 2024-01-22T13:52:00
categories: ["Java", "Spring", "GraphQL"]
author: "David"
tags: ["java", "spring", "graphql"]
draft: false
---

## Preparation

First, let's navigate to [Spring Initializr](https://start.spring.io). This service pulls in all the dependencies you need for an application and does most of the setup for you.

1. Choose Maven and select Java.
2. Click **Dependencies** and select **Spring for GraphQL** and **Spring Web**.
3. Click **Generate**.
4. Download the resulting ZIP file, which contains a GraphQL application configured with your choices.
5. Unzip and open the folder with your favorite IDE (IntelliJ recommended).

## A Very Short Introduction to GraphQL

GraphQL is a query language to retrieve data from a server. It is an alternative to REST, SOAP, or gRPC. In the next part, we will query the details for a specific book from an online store backend.

This is an example request you can send to a GraphQL server to retrieve book details:

```graphql
query bookDetails {
  bookById(id: "book-1") {
    id
    name
    pageCount
    author {
      firstName
      lastName
    }
  }
}
```

This GraphQL request says:
- Perform a query for a book with ID `"book-1"`.
- For the book, return `id`, `name`, `pageCount`, and `author`.
- For the author, return `firstName` and `lastName`.

The response is in JSON. For example:

```json
{
  "bookById": {
    "id": "book-1",
    "name": "Effective Java",
    "pageCount": 416,
    "author": {
      "firstName": "Joshua",
      "lastName": "Bloch"
    }
  }
}
```

An important feature of GraphQL is its schema language, which is statically typed. The server knows exactly what types of objects queries can retrieve and what fields those objects contain. Furthermore, clients can introspect the server to ask for schema details.

The schema for the above query is:

```graphql
type Query {
    bookById(id: ID): Book
}

type Book {
    id: ID
    name: String
    pageCount: Int
    author: Author
}

type Author {
    id: ID
    firstName: String
    lastName: String
}
```

## Our Example API: Getting Book Details

These are the main steps to create a server with Spring for GraphQL:

1. Define a GraphQL schema.
2. Implement the logic to fetch the actual data for a query.

Our example app will be a simple API to get details for a specific book. It is not intended to be a comprehensive API.

### Schema

In your Spring for GraphQL application prepared earlier, add a new file `schema.graphqls` to the `src/main/resources/graphql` folder with the following content:

```graphql
type Query {
    bookById(id: ID): Book
}

type Book {
    id: ID
    name: String
    pageCount: Int
    author: Author
}

type Author {
    id: ID
    firstName: String
    lastName: String
}
```

### Create the Book and Author Data Sources

Create the `Book` and `Author` classes in the main application package:

```java
package com.example.graphqlserver;

import java.util.Arrays;
import java.util.List;

public record Book (String id, String name, int pageCount, String authorId) {
    private static List<Book> books = Arrays.asList(
        new Book("book-1", "Effective Java", 416, "author-1"),
        new Book("book-2", "Hitchhiker's Guide to the Galaxy", 208, "author-2"),
        new Book("book-3", "Down Under", 436, "author-3")
    );

    public static Book getById(String id) {
        return books.stream()
            .filter(book -> book.id().equals(id))
            .findFirst()
            .orElse(null);
    }
}
```

```java
package com.example.graphqlserver;

import java.util.Arrays;
import java.util.List;

public record Author (String id, String firstName, String lastName) {
    private static List<Author> authors = Arrays.asList(
        new Author("author-1", "Joshua", "Bloch"),
        new Author("author-2", "Douglas", "Adams"),
        new Author("author-3", "Bill", "Bryson")
    );

    public static Author getById(String id) {
        return authors.stream()
            .filter(author -> author.id().equals(id))
            .findFirst()
            .orElse(null);
    }
}
```

### Adding Code to Fetch Data

Create a `BookController.java` class to handle GraphQL queries:

```java
package com.example.graphqlserver;

import org.springframework.graphql.data.method.annotation.Argument;
import org.springframework.graphql.data.method.annotation.QueryMapping;
import org.springframework.graphql.data.method.annotation.SchemaMapping;
import org.springframework.stereotype.Controller;

@Controller
public class BookController {
    @QueryMapping
    public Book bookById(@Argument String id) {
        return Book.getById(id);
    }

    @SchemaMapping
    public Author author(Book book) {
        return Author.getById(book.authorId());
    }
}
```

## Running Our First Query

### Enable the GraphiQL Playground

GraphiQL is a useful visual interface for writing and executing queries. Enable GraphiQL by adding this line to `application.properties`:

```properties
spring.graphql.graphiql.enabled=true
```

### Boot the Application

Start your Spring application and navigate to [http://localhost:8080/graphiql](http://localhost:8080/graphiql).

### Run the Query

Enter the following query in GraphiQL and execute it:

```graphql
query bookDetails {
  bookById(id: "book-1") {
    id
    name
    pageCount
    author {
      id
      firstName
      lastName
    }
  }
}
```

You should see a response like this:

```json
{
  "data": {
    "bookById": {
      "id": "book-1",
      "name": "Effective Java",
      "pageCount": 416,
      "author": {
        "id": "author-1",
        "firstName": "Joshua",
        "lastName": "Bloch"
      }
    }
  }
}
```

Congratulations! You have built a GraphQL service and executed your first query using Spring for GraphQL.

