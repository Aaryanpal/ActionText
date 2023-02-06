# Instruction to enable the Action Text in Rails

Create a new app

```
rails new actiontext_demo

```
Generate a Scaffold

```
rails generate scaffold Post title:string

```

At this point, let’s set up our database and migrate our Post table.

```
rails db:migrate
```

Configure your routes.rb

```
Rails.application.routes.draw do
  root to: "posts#index"
  resources :posts
end
```

Now let's head into installing ActionText

```
bin/rails action_text:install

```

upon installing this 
```
 append  app/javascript/application.js
      append  config/importmap.rb
      create  app/assets/stylesheets/actiontext.css
To use the Trix editor, you must require 'app/assets/stylesheets/actiontext.css' in your base stylesheet.
      create  app/views/active_storage/blobs/_blob.html.erb
      create  app/views/layouts/action_text/contents/_content.html.erb
Ensure image_processing gem has been enabled so image uploads will work (remember to bundle!)
        gsub  Gemfile
       rails  railties:install:migrations FROM=active_storage,action_text
Copied migration 20230206190402_create_active_storage_tables.active_storage.rb from active_storage
Copied migration 20230206190403_create_action_text_tables.action_text.rb from action_text

```

these files be added

Add the CSS File

```
*= require_tree .
*= require_self
*/
@import "./actiontext.scss";
```

Before moving on, let’s migrate our database again to add the ActiveStorage and ActionText tables to our DB schema.

```
rails db:migrate
```
In your Model File

```
app/models/post.rb
class Post < ApplicationRecord
  has_rich_text :content
end
```
Adding the rich text form

```
<div class="field">
  <%= form.label :content %>
  <%= form.rich_text_area :content %>
</div>
```

Add for showing the content
```
<p>
  <strong>Title:</strong>
  <%= @post.title %>
</p>
<p>
  <strong>Content:</strong>
  <%= @post.content %>
</p>
```
Passing the Content the Strong Params

```
 def post_params
   params.require(:post).permit(:title, :content)
 end
```

And for Performing the N+1 Database queries

```
def index
  @posts = Post.all.with_rich_text_content
end
```