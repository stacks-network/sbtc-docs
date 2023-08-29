# Best Practices

## General Principles

The main principle is simplification. Please keep it simple, and don't add new unnecessaries. In particular, see [Occam's razor].

### Convention Over Configuration

If you can do something without configuring, e.g., using default settings, please do. Try to avoid introducing new "better" conventions. For more details, see [Convention over configuration](https://en.wikipedia.org/wiki/Convention_over_configuration). Using fewer configurations reduces information noise.

### Design Patterns And Anti-Patterns

Don't worry much about [design patterns](https://en.wikipedia.org/wiki/Software_design_pattern), but avoid [anti-patterns](https://en.wikipedia.org/wiki/Anti-pattern). If you don't use some well-known design patterns, like the [factory method](https://en.wikipedia.org/wiki/Factory_method_pattern), it's okay. However, if you introduce anti-patterns, such as a [mutable singleton](https://en.wikipedia.org/wiki/Singleton_pattern#Criticism), it damages the quality of our software.

### No Direct Usage Of I/O

Please don't use direct I/O calls in libraries at all costs. You can bind direct I/O in the final application. Our libraries should never use direct I/O in the ideal world. I/O destroys deterministic behavior. Determinism is essential for testing and debugging because it allows us to reproduce behavior. If we can avoid I/O in our libraries, we can achieve 100% test coverage for them. 

## Rust Conventions

To use the convention over configuration principle in Rust, see [https://doc.rust-lang.org/cargo/guide/project-layout.html](https://doc.rust-lang.org/cargo/guide/project-layout.html).
