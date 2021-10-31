[TOC]

# Configuration

- Create a config class and annotate it with @Configuration
- applicationContext.xml replaced by @Configuration
- @Configuration at class level
- Spring Beans defined by @Bean
- @Bean at method level

## Setter Injection

- Simple as a method call
- "Mystery" of injection goes away
- Simply calling a setter

## Constructor Injection

- Just like setter injection
- Guaranteed contract
- Constructor defined
- Used together with setter injection
- Index-based

# Scopes

5 scopes

Valid in any configuration

- Singleton
  - One instantiation
  - Default bean scope
  - Single instance per Spring container
- Prototype
  - Unique per request

Valid only in web-aware Spring projects

- Request
  - Lifecycle of Bean request
- Session
  - Single Bean per HTTP session
- GlobalSession
  - Single Bean per application

# Autowired

- Add a ComponentScan annotation
- Mark Bean as autowired
- Spring automatically wires beans
  - byType
  - byName
  - constructor
  - no

# Stereotypes

- @Component
- @Repository
- @Service

# applicationContext.xml

- Name doesn't matter
- Spring Context sort of a HashMap
- Can simply be a registry
- XML configuration begins with this file
- Namespaces aid in configuration/validation

# Advanced Bean Configuration

- Bean Lifecycle
  - Instantiation
  - Populate Properties
  - BeanNameAware
  - BeanFactoryAware
  - Pre Initialization - BeanPostProcessors
  - InitializeBean
  - initMethod
  - Post Initialization - BeanPostProcessors
- FactoryBean
  - Builds on initMethod concept
  - Factory Method Pattern
  - Legacy Code
  - Contract without Constructor
  - Static Methods
- SpEL
  - Spring Expression Language
  - Manipulate Object Graph
  - Evaluate at Runtime
  - Configuration
- Proxies
  - @Transactional
- Profiles
  - Adapt Environments
  - Runtime Configuration