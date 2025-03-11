---
title: "The Concept of API Contracts"
meta_title: "The Concept of API Contracts"
description: "We define an API contract as a formal agreement between a software provider and a consumer that abstractly communicates how to interact with each other."
date: 2023-09-23T13:52:00
categories: ["Software Development"]
author: "Davod"
tags: ["api", "software development"]
draft: false
---

![The Concept of API Contracts](https://www.finecloud.ch/media/website/download.jpg)

## The Concept of API Contracts

### What is it about?

We define an API contract as a formal agreement between a software provider and a consumer that abstractly communicates how to interact with each other. This contract defines how API providers and consumers interact, what data exchanges look like, and how to communicate success and failure cases.

The provider and consumers do not have to share the same programming language, only the same API contracts. Let's imagine that we need to design an API for a Family Cash Card Web Application. Let’s assume that currently there's one contract between the Cash Card service and all services using it. Below is an example of that first API contract.

```json
Request
  URI: /cashcards/{id}
  HTTP Verb: GET
  Body: None

Response:
  HTTP Status:
    200 OK if the user is authorized and the Cash Card was successfully retrieved
    403 UNAUTHORIZED if the user is unauthenticated or unauthorized
    404 NOT FOUND if the user is authenticated and authorized but the Cash Card cannot be found
  Response Body Type: JSON
  Example Response Body:
    {
      "id": 99,
      "amount": 123.45
    }
```

##  Why Are API Contracts Important?

API contracts are important because they communicate the behavior of a REST API. They provide specific details about the data being serialized (or deserialized) for each command and parameter being exchanged. The API contracts are written in such a way that can be easily translated into API provider and consumer functionality, and corresponding automated tests.

