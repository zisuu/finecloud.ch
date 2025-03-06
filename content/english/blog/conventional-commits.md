---
title: "Conventional commits"
meta_title: "Conventional commits - Finecloud"
description: "Conventional Commits is a convention for writing commit messages that provides structure and consistency to a project's version control history."
date: 2024-02-14T13:52:00
categories: ["Git", "Software Development"]
author: "David"
tags: ["git", "conventional commits", "version control"]
draft: false
---

## What are Conventional commits

Conventional Commits is a convention for writing commit messages that provides structure and consistency to a project's version control history. It's based on the idea of defining a standard format for commit messages that makes it easier for developers to understand the changes made to a codebase over time.

[Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/#specification)

## Why should you use Conventional commits

- **Improving collaboration**: Having a clear, concise, and standardized way of writing commit messages helps everyone on the team understand what changes were made and why. This can improve collaboration and reduce the time spent discussing changes.
- **Better automated versioning**: Conventional Commits provides a way to automate versioning and release management by using the commit messages to determine the type and scope of changes. This can save time and improve accuracy compared to manually managing versions.
- **Improved documentation**: Commit messages can serve as a form of documentation, providing insight into the motivations and thought process behind changes. By using Conventional Commits, this documentation is more consistent and easier to understand.

## Example

Here's an example of a Conventional Commit message:

```plaintext
feat(parser): add ability to parse hexadecimal numbers

This commit adds the ability to parse hexadecimal numbers, allowing for a wider range of input options.
```

In this example, `feat` is the type of change being made (a new feature), `parser` is the scope of the change (the parser component), and the rest of the message provides a brief description of the change. The Conventional Commits specification defines a number of different types (e.g., `feat`, `fix`, `docs`) and provides guidelines for how to use them.

### Multiline commit messages

How do you make multiline commit messages like these? How do you add an optional body or optional footer in a Conventional Commit message?

Multiline commit messages are written by adding a blank line between the subject (first line) and the body (or footer). The subject should be a single line of text that summarizes the change and should not exceed 50 characters. If a more detailed explanation is needed, it can be added in the body.

Here's an example of a multiline Conventional Commit message:

```plaintext
feat(parser): add ability to parse hexadecimal numbers

This commit adds the ability to parse hexadecimal numbers, allowing for a wider range of input options. The new feature has been thoroughly tested and is now ready for use.
```

An optional footer can also be added to Conventional Commit messages for extra information, such as breaking changes, references to related issues, or other meta-information. The footer should start with a new line and should be separated from the body by a double line break. Here's an example with a footer:

```plaintext
feat(parser): add ability to parse hexadecimal numbers

This commit adds the ability to parse hexadecimal numbers, allowing for a wider range of input options. The new feature has been thoroughly tested and is now ready for use.

BREAKING CHANGE: The previous version of the parser only supported decimal numbers, so this change may break compatibility with existing code that uses the parser.
```

It's worth noting that not all Conventional Commit tools and systems require or make use of the body or footer. The most important aspect is the subject line, as it provides a concise summary of the change. The body and footer are optional and should be used as needed to provide additional context and information.

Here's an example of a Conventional Commit message with a body and footer, using the `git commit` command with the `-m` option:

```plaintext
$ git commit -m "feat(parser): add ability to parse hexadecimal numbers"

This commit adds the ability to parse hexadecimal numbers, allowing for a wider range of input options. The new feature has been thoroughly tested and is now ready for use.

BREAKING CHANGE: The previous version of the parser only supported decimal numbers, so this change may break compatibility with existing code that uses the parser.

Closes #123
```

In this example, the subject is `feat(parser): add ability to parse hexadecimal numbers`, the body is `This commit adds the ability to parse hexadecimal numbers, allowing for a wider range of input options. The new feature has been thoroughly tested and is now ready for use.`, and the footer is `BREAKING CHANGE: The previous version of the parser only supported decimal numbers, so this change may break compatibility with existing code that uses the parser. Closes #123`. The `Closes #123` part of the footer is an example of a reference to a related issue, indicating that this commit resolves the issue with the given number.