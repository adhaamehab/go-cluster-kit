# 1. Project Structure for Extensibility

Date: 2023-09-12

## Status

Accepted

## Context

As the project grows, it's crucial to organize the codebase in a way that remains clean and maintainable. We need a structure that allows for isolation of concerns, ease of testing, and scalability for future growth.

## Decision

We've chosen to structure our project as follows:

- `cmd`: Application entry points
- `pkg`: Libraries and packages used by our application
- `examples`: Example implementations of our framework
- `docs`: Documentation, including ADRs
- `scripts`: Scripts for build, lint, test, etc.

This structure separates different types of concerns into different directories, making it easy to navigate the project. It also provides clear locations for new code as the project grows.

## Consequences

This structure will require new contributors to familiarize themselves with our project layout. However, we believe the benefits of a clean, organized codebase outweigh this initial learning curve.

The structure also facilitates extensibility. As we add new features or make changes, we have well-defined places to put new code. This will make the project easier to scale and maintain in the long run.

## References

- [Standard Go Project Layout](https://github.com/golang-standards/project-layout)