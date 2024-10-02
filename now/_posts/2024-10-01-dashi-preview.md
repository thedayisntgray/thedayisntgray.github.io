---
layout: post
type: post
title: "🍜 Dashi - A Streamlit like Framework for Rubyists [Preview]"
date: 2024-10-01
comments: true
author: "Landon Gray"
published: true
tags: [ruby, openai, framework]
description: |
    A comprehensive guide on creating a Rails application that integrates with OpenAI's API, including setup and  troubleshooting.

---
# Introducing Dashi🍜: A Lightweight Framework for Building Data & AI Applications in Ruby

<div style="position: relative; padding-bottom: 62.5%; height: 0;"><iframe src="https://www.loom.com/embed/e27152d9705044cf81820181d8104e26?sid=67dceb14-e808-4533-b94d-ba44d0d12998" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe></div>

<br>


## What is Dashi?

Dashi is a minimalist framework inspired by [Streamlit](https://streamlit.io/), designed specifically for Ruby developers. It streamlines the process of creating data-driven and AI applications by providing an intuitive syntax and pre-built components. With Dashi, you can focus on the core functionality of your app without worrying about the boilerplate code usually associated with web development.

## The Full Program

Below is a complete Dashi application that implements a simple AI chatbot:

```ruby
require_relative 'dashi'

Dashi.run do
  title 'Dashi AI Chatbot'
  text 'This is a basic OpenAI application for chat similar to ChatGPT'
  chatbot
end
```

## Breaking Down the Code

Let's delve into what each part of this program does.

### 1. Requiring Dashi

```ruby
require_relative 'dashi'
```

This line includes the Dashi framework into your Ruby script. By using `require_relative`, it tells Ruby to load the `dashi.rb` file located in the same directory as your script. This file contains all the necessary code for Dashi to function.

### 2. Running the Dashi Application

```ruby
Dashi.run do
  # Application code goes here
end
```

The `Dashi.run` method initializes the Dashi application. The block passed to this method contains the components and logic of your app. It's within this block that you'll define the user interface and functionality.

### 3. Adding a Title

```ruby
title 'Dashi AI Chatbot'
```

The `title` method sets the main heading of your application. In this case, it displays "Dashi AI Chatbot" at the top of the web page.

### 4. Adding Descriptive Text

```ruby
text 'This is a basic OpenAI application for chat similar to ChatGPT'
```

The `text` method adds a paragraph of text to your application. It's useful for providing descriptions or instructions to the user.

### 5. Integrating the Chatbot

```ruby
chatbot
```

The `chatbot` method is a pre-configured component in Dashi that integrates a chat interface. It handles user input and displays AI-generated responses, mimicking the functionality of ChatGPT.

## What the Program Does

When you run this program, Dashi starts a web server and generates a web page with the following features:

- **Title**: "Dashi AI Chatbot" is displayed prominently at the top.
- **Description**: A brief explanation of the application's purpose.
- **Chat Interface**: A text input area where users can type messages and receive AI-generated responses.

The application allows users to engage in a conversation with the AI, asking questions or seeking information just like they would with ChatGPT.

## Why Use Dashi?

- **Simplicity**: Dashi's straightforward syntax allows you to build applications quickly without getting bogged down in complex configurations.
- **Efficiency**: With minimal code, you can create functional apps, reducing development time.
- **Flexibility**: Designed for data and AI applications, Dashi provides components that cater specifically to these domains.

## Join the Beta and Stay Updated

I'm excited to share Dashi with the Ruby community and would love for you to try it out once it's released!

- **Stay Updated**: Dashi isn't released yet, but if you're curious about its development, you can:

  - **Subscribe to my newsletter**: [https://buttondown.com/landon.gray](https://buttondown.com/landon.gray)
  - **Follow me on LinkedIn**: [Your LinkedIn Profile](link_to_linkedin_profile)
  - **Follow me on X (formerly Twitter)**: [Your X Profile](link_to_x_profile)

You'll receive updates on Dashi's progress, release dates, and more.

## Conclusion

Dashi aims to make building data and AI applications in Ruby as seamless as possible. By abstracting away the repetitive parts of web development, it allows you to focus on what matters most: creating impactful applications.
