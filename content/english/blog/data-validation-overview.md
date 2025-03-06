---
title: "Data Validation Overview"
meta_title: "Data Validation Overview - Finecloud"
description: "An overview of data validation, covering validation techniques, Java Bean Validation, Jakarta Bean Validation 3.0, Spring Framework validation, and more."
date: 2024-05-02T13:52:00
categories: ["Java", "Spring Framework", "Software Development"]
author: "David"
tags: ["java", "spring", "validation", "bean validation"]
draft: false
---

## What is Validation?

- Validation ensures data integrity by asserting conditions against data.
- Examples:
  - Is a value required?
  - What is the maximum length of a string?
  - Is a given date valid?
- Often referred to as "defensive programming" or "trust but verify."
- An essential but often overlooked step in software development.

<!--more-->

## When to Validate?
- Validate data early and often!
- Validation should occur at every exchange:
  - UI level with user-friendly feedback.
  - RESTful API level before reaching the service layer.
  - Before persisting data into a database.
  - Database constraints also enforce validation.
- Early validation prevents difficult error handling later.

## Java Bean Validation
- Java Bean Validation is a standardized Java API for validation.
- Provides a structured way to validate data and handle errors.
- More elegant than writing custom validation logic.
- Bean Validation is an API, requiring an implementation.
- Fun Fact: Gunnar Morling, founder of MapStruct, leads the Bean Validation API and contributes to Hibernate Validator.

## Jakarta Bean Validation 3.0
- Released in July 2020.
- Renamed from "Bean Validation" to "Jakarta Bean Validation."
- The primary change from 2.0 to 3.0 was package renaming:
  - **2.0:** `javax.validation`
  - **3.0:** `jakarta.validation`
- Supported in **Spring Framework 6.x+**.
- Hibernate Validator 7.x+ is a common implementation.

## Built-In Constraint Definitions
- `@Null` - Checks if value is null.
- `@NotNull` - Ensures value is not null.
- `@AssertTrue` - Value must be true.
- `@AssertFalse` - Value must be false.
- `@Min(value)` - Checks if number is equal to or greater than value.
- `@Max(value)` - Checks if number is equal to or less than value.
- `@Size(min, max)` - Checks if a string or collection size is within limits.
- `@Pattern(regex)` - Validates against a regular expression.
- `@NotEmpty` - Ensures string/collection is neither null nor empty.
- `@Email` - Validates email format.
- And many more...

## Hibernate Validator Constraints
- `@CreditCardNumber` - Validates credit card format.
- `@Currency` - Ensures valid currency amount.
- `@ISBN` - Checks for valid ISBN.
- `@Range(min, max)` - Validates if number falls within range.
- `@URL` - Ensures a valid URL format.
- `@UniqueElements` - Ensures a collection has unique elements.
- Several additional domain-specific constraints exist.

## Validation and Spring Framework
- Spring Framework integrates seamlessly with Bean Validation.
- Can be used in controllers, services, and Spring-managed components.
- Spring MVC automatically returns **400 Bad Request** for validation failures.
- Spring Data JPA enforces validation at persistence level.

## Spring Boot and Validation
- Spring Boot auto-configures validation if an implementation is on the classpath.
- If only the API is present (without an implementation), validation annotations exist but do nothing.
- **Spring Boot 2.3+** requires manually adding the validation starter dependency.
- Example:
  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation</artifactId>
  </dependency>
  ```