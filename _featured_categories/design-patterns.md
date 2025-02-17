---
# Featured tags need to have either the `list` or `grid` layout (PRO only).
layout: list

# The title of the tag's page.
title: Design Patterns

# The name of the tag, used in a post's front matter (e.g. tags: [<slug>]).
slug: design-patterns

# (Optional) Write a short (~150 characters) description of this featured tag.
description: >
  주요 디자인 패턴을 설명하는 글들을 모아놓은 곳입니다.

# (Optional) You can disable grouping posts by date.
# no_groups: true

# Exclude this example category from the sitemap.
# DON'T USE THIS SETTING IN YOUR CATEGORIES!
sitemap: false
---

# Design Patterns

## Category of GoF Patterns
- **Creational**
  - Address problems of creating an onject in a flexible way
  - Separate creation from operation/use
    - Class: [Factory Method](./2023-10-11-factory-method)
    - Object: [Abstract Factory](./2023-10-11-abstract-factory),
              [Builder](./2023-12-24-builder),
              [Prototype](./2023-12-14-prototype),
              [Singleton](./2023-10-18-singleton)
- **Structural**
  - Address problems of using OO constructs like inheritance to organize classes and objects
    - Class: [Adapter](./2023-10-18-adapter)
    - Object: [Adapter](./2023-10-18-adapter),
              [Bridge](./2023-12-13-bridge),
              [Composite](./2023-12-14-composite),
              [Decorator](./2023-10-18-decorator),
              [Façade](./2023-10-18-façade),
              Flyweight,
              Proxy
- **Behavioral**
  - Address problems of assigning responsibilities to classes
  - Suggest both static relationships and patterns of communication
    - Class: Interpreter, [Template Method](./2023-12-25-template-method)
    - Object: [CoR](./2023-12-13-cor),
              [Command](./2023-10-11-command),
              [Iterator](./2023-10-18-iterator),
              [Mediator](./2023-10-20-mediator),
              Memento,
              [Observer](./2023-10-11-observer),
              [State](./2023-12-23-state),
              [Strategy](./2023-10-11-strategy),
              [Visitor](./2023-12-27-visitor)
