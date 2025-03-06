---
title: "Microservices"
meta_title: "Microservices"
description: "A summary of the famous article about Microservices by Martin Fowler."
date: 2023-10-28T19:12:00
categories: ["Microservices"]
author: "David"
tags: ["microservices", "architecture", "software development"]
draft: false
---

This Post is a summary of the famous Article about Microservices: [https://martinfowler.com/articles/microservices.html](https://martinfowler.com/articles/microservices.html)

<!--more-->
The text discusses the concept of "Microservice Architecture," which is an approach to designing software applications as a suite of independently deployable services. It highlights that there is no precise definition but outlines common characteristics such as organization around business capabilities, automated deployment, decentralized control of languages and data, and the use of lightweight communication mechanisms like HTTP. Microservices are contrasted with monolithic architecture, where applications are built as a single unit. The text emphasizes that microservices provide advantages like independent deployment, scalability, and modular structure, making it increasingly appealing for building enterprise applications. The microservice style is not claimed to be innovative but is considered beneficial for software development.

## Componentization via Services

The text discusses the evolution of component-based software development in the software industry. It highlights the distinction between libraries and services as components, with a focus on microservice architectures. The main point is that components, in this context, are units of software that are independently replaceable and upgradeable. The text also explains that services, as out-of-process components, offer advantages in terms of independent deployability and explicit component interfaces. However, it acknowledges that using services can have downsides, such as increased overhead for remote calls and challenges in changing the allocation of responsibilities between components. The text concludes by noting that services can consist of multiple processes that are developed and deployed together.

## Organized around Business Capabilities

The text mentions how companies like comparethemarket.com organize themselves using cross-functional teams responsible for building and operating individual services. The text also touches upon Conway's Law, emphasizing that an organization's system design mirrors its communication structure.

The key point is the contrast between the traditional approach of splitting teams based on technology layers (UI, server-side, database) and the microservices approach, which focuses on dividing services around business capabilities. Microservices encourage cross-functional teams with expertise in user experience, database, and project management. The text suggests that large monolithic applications can also benefit from modularization based on business capabilities but cautions against excessive complexity and recommends maintaining clear team boundaries, which is facilitated by the more explicit separation in service components.

## Products not Projects

The text discusses the difference in development approaches between traditional project-based models and the microservices approach. In the traditional model, software development is seen as a project with a defined end, after which it's handed over to a maintenance organization. Microservice proponents advocate for teams to own a product throughout its entire lifecycle, emphasizing the "you build, you run it" philosophy popularized by Amazon.

This approach encourages developers to be responsible for their software in production, fostering closer interaction with how it behaves and its users. The text highlights that this product-oriented mentality aligns with the focus on business capabilities, emphasizing an ongoing relationship where software continuously enhances business capabilities.

It also notes that while this approach can be applied to monolithic applications, the smaller granularity of services in microservices makes it easier to establish personal relationships between service developers and their users.

## Smart endpoints and dumb pipes

The text starts by mentioning the traditional approach, exemplified by Enterprise Service Bus (ESB), where significant intelligence is embedded in the communication mechanism itself, allowing for sophisticated message routing, choreography, and transformation.

In contrast, the microservices community prefers a different approach: "smart endpoints and dumb pipes." Microservices are designed to be highly decoupled and cohesive, with each service owning its domain logic. These services act as filters, receiving requests, applying logic, and producing responses. They utilize simple RESTish protocols and emphasize two common protocols: HTTP request-response with resource APIs and lightweight messaging. The principles of the World Wide Web and Unix underlie these protocols.

The text also highlights that, in microservices, the infrastructure used for messaging is typically simple and serves as a message router only, with the intelligence residing in the end points. The key challenge in transitioning from a monolithic architecture to microservices is changing the communication pattern. The text advises against a naive conversion to remote procedure calls (RPC) as it can lead to inefficient and "chatty" communications, advocating for a coarser-grained approach instead.

## Decentralized Governance

The text highlights the limitations of centralized governance, such as the tendency to standardize on single technology platforms. In contrast, microservices allow for a more flexible approach, enabling teams to choose the right tools and technologies for specific components.

Microservice teams focus on producing practical tools and sharing them with other developers, often following open-source practices. This approach encourages flexibility in solving similar problems while still valuing service contracts. The text mentions patterns like Tolerant Reader and Consumer-Driven Contracts that help service contracts evolve independently, with tools enabling automated contract verification during the build process.

Furthermore, the text discusses the "build it / run it" ethos popularized by Amazon, where development teams are responsible for operating the software they build, emphasizing the decentralization of responsibility. This approach, exemplified by companies like Netflix, fosters a focus on code quality and contrasts sharply with traditional centralized governance models.

## Decentralized Data Management

The text points out that decentralized data management leads to differences in the conceptual models of systems, particularly when integrating across a large enterprise. This divergence in views can even occur within applications, especially when they are divided into separate components, which can be understood using the concept of Bounded Context from Domain-Driven Design.

Microservices further decentralize data storage decisions by allowing each service to manage its own database, known as Polyglot Persistence. This contrasts with the monolithic approach of a single logical database for persistent data.

In terms of data updates, traditional monolithic applications often use transactions to guarantee consistency when updating multiple resources. However, microservices prioritize transactionless coordination between services due to the challenges of implementing distributed transactions. This approach acknowledges that consistency may be eventual and addresses problems with compensating operations.

The text also highlights that managing inconsistencies aligns with business practices where businesses often tolerate a degree of inconsistency to respond quickly to demand, with the ability to reverse processes to address mistakes. This trade-off is considered worthwhile as long as the cost of fixing errors is lower than the cost of lost business under greater consistency.

## Infrastructure Automation

The text points out that teams building microservices often have experience with Continuous Delivery and Continuous Integration, both of which heavily rely on infrastructure automation.

The text highlights that infrastructure automation plays a crucial role in building confidence in software by running automated tests and automating deployment to different environments. It mentions that once the path to production for a monolithic application is automated, deploying more applications becomes less daunting. The goal of Continuous Delivery is to make deployment a routine and uneventful process.

The text also acknowledges that while the deployment process may not differ significantly between monolithic applications and microservices, the operational landscape for each can be notably distinct, suggesting that infrastructure automation is key to managing microservices effectively in production.

## Design for failure

The text points out that using services as components means applications need to be resilient and capable of handling service failures. Unlike monolithic designs, microservices introduce complexity in managing failures gracefully. To address this, microservice teams place a strong emphasis on monitoring and detecting failures in real-time.

The text mentions Netflix's "Simian Army," which intentionally induces service and datacenter failures during the working day to test the application's resilience and monitoring capabilities. While monolithic architectures can also have sophisticated monitoring, it's less common.

Microservices require the ability to quickly detect and, if possible, automatically restore service. They rely on real-time monitoring, checking both architectural and business-relevant metrics. Semantic monitoring helps spot issues, especially in a microservices architecture where choreography and event collaboration can lead to emergent behavior, which may not always be desirable.

The text concludes that microservice teams expect to have sophisticated monitoring and logging setups for each individual service, including dashboards for status, operational and business metrics, and details on circuit breaker status, throughput, and latency. Transparency and quick detection of failures are critical in a microservices environment.

## Evolutionary Design

Microservice practitioners often come from an evolutionary design background and view service decomposition as a tool to enable application developers to control changes without slowing down the development process.

The key principle behind microservices is the notion of independent replacement and upgradeability. This means looking for points in the application where components can be rewritten without affecting their collaborators. Some microservice groups take this a step further by expecting that many services will be replaced rather than evolved in the long term.

The text provides examples of applications that started as monoliths but evolved in a microservice direction. These microservices are particularly useful for adding temporary features or services that are discarded after a short period, such as specialized pages for sporting events.

It emphasizes the importance of modular design based on the pattern of change, where components that change together should be in the same module. Microservices allow for more granular release planning, as changes only require redeploying the specific service(s) that were modified. However, this introduces the challenge of ensuring that changes to one service do not break its consumers, and the text suggests that versioning should be a last resort, with services designed to be tolerant of changes in their suppliers.

## Are Microservices the Future?

The concept of microservices is a promising architectural style for enterprise applications but the text emphasizes that it's still too early to make definitive judgments about its long-term impact. Several well-known companies, including Amazon, Netflix, The Guardian, and others, have adopted microservices. However, the text acknowledges that the full consequences of architectural decisions may take several years to become evident.

It highlights some challenges and potential concerns associated with microservices, such as the difficulty of defining service boundaries, the increased complexity in coordinating interface changes, and the risk of moving complexity from within a component to the connections between components. Additionally, the success of microservices can be influenced by team skill, and it remains to be seen how less skillful teams would fare with this approach.

The text suggests a reasonable argument of starting with a monolith and splitting it into microservices when necessary, while maintaining modularity from the beginning. It concludes with cautious optimism, acknowledging that the microservices style holds promise, but the ultimate outcomes will depend on how well it addresses these challenges in practice.
