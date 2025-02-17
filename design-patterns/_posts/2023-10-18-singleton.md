---
layout: post
title: Singleton Pattern
description: >
  Singleton Pattern에 대해 설명하는 페이지입니다.
image: 
  path: /assets/img/design-patterns/laptop.jpg
  srcset:
    1060w: /assets/img/design-patterns/laptop.jpg
    530w:  /assets/img/design-patterns/laptop.jpg
    265w:  /assets/img/design-patterns/laptop.jpg
related_posts:
  - None
sitemap: true
comments: false
---
<i>Environment</i> 
- <i>Programming Language: Java</i>

## Index
- [Index](#index)
- [Introduction](#introduction)
- [How to Use (Example)](#how-to-use-example)
- [Comments](#comments)

## Introduction
- **Purpose**
  - Ensures that **only one instance** of a class is allowed within a system.
- **Use When**
  - Exactly one instance of a class is required.
  - Controlled access to a single object is necessary.

## How to Use (Example)
- **For single-thread**
  - Make the instructor be private
    - private Singleton() { }
  - Provide a getInstance() method
    - public static Singleton getInstance()
  - Remember the instance once you have created it
    - private static Singleton instance
    - if (instance == null) instance = new Singleton();
  ```java
  public class Singleton {
      private static Singleton instance;

      // other useful instance variables

      private Singleton() { //... }

      public static Singleton getInstance() {
          if (instance == null) {
              instance = new Singleton();
          }
          return instance;
      }

      // other useful methods
  }
  ```

- **For multi-thread**
  - **Option 1**
    - Use **synchronized** keyword
      - public static synchronized Singleton getInstance()
    - Cons: It causes small impact on run-time performance due to frequent locking.
    ```java
    public class Singleton {
        private static Singleton instance;

        // other useful instance variables

        private Singleton() { //... }

        public static synchronized Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }

        // other useful methods
    }
    ```
  
  - **Option 2**
    - Pros: no runtime overhead
    - Cons: resource overhead when the instance is not used
    ```java
    public class Singleton {
        private static Singleton instance = new Singleton();

        // other useful instance variables

        private Singleton() { //... }

        public static Singleton getInstance() {
            return instance;
        }

        // other useful methods
    }
    ```
  
  - **Option 3**
    - Use volatile keyword and double-checked locking
    - Pros: theoretically perfect solution with respect to performance
    - Cons: dependent on the java version (We have to ensure that we are running at least Java 5)
    ```java
    public class Singleton {
        private volatile static Singleton instance = null;

        // other useful instance variables

        private Singleton() { //... }

        public static Singleton getInstance() {
            if (instance == null) {
                synchronized(Singleton.class) {
                    if (instance == null) {
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }

        // other useful methods
    }
    ```
  
<br />
<br />
<br />

## Comments
<hr />
<script
  src="https://utteranc.es/client.js"
  repo="HyunJinNo/HyunJinNo.github.io"
  issue-term="pathname"
  theme="github-light"
  crossorigin="anonymous"
  async
></script>