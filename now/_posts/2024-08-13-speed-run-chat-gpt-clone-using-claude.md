---
layout: post
type: post
title: "Building a ChatGPT Clone with Ruby on Rails and Claude 3.5 Sonnet 🚀"
date: 2024-08-13
comments: true
author: "Landon Gray"
published: true
tags: [rails, openai, api, development]
description: |
    A comprehensive guide on creating a Rails application that integrates with OpenAI's API, including setup and  troubleshooting.

---




In this video, I build a chat application similar to ChatGPT by relying heavily on Claude 3.5 Sonnet. I wanted to let loose and give yet another example of what LLMs are capble of doing with the help of a thoughtful prompter.

<div style="position: relative; padding-bottom: 56.25%; height: 0;">
<iframe src="https://www.loom.com/embed/1283a5acd11d4593a09d2199b07816bc?sid=0e9d097f-4064-4860-9e71-9a9049dc0369" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
</div>

<br>


## Our initial prompt to kick us off 🙌🏾

> I want to generate a Rails application that is a clone of ChatGPT. 
> Start with the commands I should run to get the initial rails app up and running and then tell me what models, routes, controllers I should create.

## Setting Up the Rails Application

We started by creating a new Rails application with SQLite as the database:

```bash
gem install rails
rails new chatgpt_clone
cd chatgpt_clone
rails db:create
```

## Adding Necessary Gems

We updated the Gemfile to include the required gems:

```ruby
gem 'ruby-openai', '~> 4.0.0'
gem 'dotenv-rails', groups: [:development, :test]
```

After updating the Gemfile, we ran `bundle install` to install the new gems.

## Setting Up Environment Variables

To securely store our OpenAI API key, we created a `.env` file in the root of our project:

```plaintext
OPENAI_API_KEY=sk-your_actual_api_key_here_without_quotes
```

We also ensured that `.env` was added to our `.gitignore` file to keep the API key secret.

## Creating the Chat Model

We generated a Chat model to store our conversations:

```bash
rails generate model Message content:text role:string
rails db:migrate
```

## Implementing the Controller

We created a ChatsController to handle the interaction with OpenAI:

```ruby
class ChatsController < ApplicationController
  def index
    @messages = Message.all
    @message = Message.new
  end

  def create
    @message = Message.new(message_params)
    if @message.save
      # Here you would integrate with an AI service to get a response
      ai_response = "This is a placeholder AI response."
      Message.create(content: ai_response, role: 'assistant')
      redirect_to chats_path
    else
      render :index, status: :unprocessable_entity
    end
  end

  private

  def message_params
    params.require(:message).permit(:content).merge(role: 'user')
  end
end
```

## Setting Up Routes

We updated our `config/routes.rb` file:

```ruby
Rails.application.routes.draw do
  root 'chats#index'
  resources :chats, only: [:index, :create]
end
```

## Creating the View

We created a simple view in `app/views/chats/index.html.erb` to interact with our chat application.

```
<div class="chat-container">
  <div class="messages">
    <% @messages.each do |message| %>
      <div class="message <%= message.role %>">
        <%= message.content %>
      </div>
    <% end %>
  </div>
  
  <%= form_with(model: @message, url: chats_path, local: true) do |f| %>
    <%= f.text_area :content, placeholder: "Type your message..." %>
    <%= f.submit "Send" %>
  <% end %>
</div>

```

## Sprinkle in some css ✨

```
.chat-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.messages {
  height: 400px;
  overflow-y: scroll;
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 20px;
}

.message {
  margin-bottom: 10px;
  padding: 5px;
  border-radius: 5px;
}

.user {
  background-color: #e6f2ff;
  text-align: right;
}

.assistant {
  background-color: #f0f0f0;
  text-align: left;
}

form {
  display: flex;
}

textarea {
  flex-grow: 1;
  margin-right: 10px;
}

```

## Troubleshooting

During the development process, we encountered a few issues:

1. **Uninitialized constant ChatsController::OpenAI**: We resolved this by properly requiring the 'openai' gem in our controller.

2. **Faraday::UnauthorizedError**: This was due to an authentication issue with the OpenAI API. We double-checked our API key and ensured it was being loaded correctly from the `.env` file.

3. **Environment Variable Loading**: We made sure to properly set up the `dotenv-rails` gem to load our environment variables.

## That's it!

Doing this speed run was a fun and useful exercise. I'm throughly impressed by Claude's abiltiy to generate Rails code.
