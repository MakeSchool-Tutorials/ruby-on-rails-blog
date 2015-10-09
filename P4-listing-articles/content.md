---
title: "Listing All Articles"
slug: listing-articles
---     

So we've created a form and saved the articles content in the database, but now we want to see a list of all the articles we have on the blog. 

We need to add another action to our **articles_controller.rb**. Let's open it and add the **index** action:

    class ArticlesController < ApplicationController
      def index
        @articles = Article.all
      end
      
      def show
        @article = Article.find(params[:id])
      end
      
      . . . shortened for brevity . . .
    end

We also need a view of course, so let's create a new file called **index.html.erb** in the views directory */blog/app/views/articles*. Add the following code:

    <h1>Listing Articles</h1>
     <table>
      <tr>
        <th>Title</th>
        <th>Text</th>
      </tr>
      <% @articles.each do |article| %>
        <tr>
          <td><%= article.title %></td>
          <td><%= article.text %></td>
        </tr>
      <% end %>
    </table>

Go to [http://localhost:3000/articles](http://localhost:3000/articles) and see all your articles listed. This is an almost complete web page but the one thing we're missing is fundamental to a web page, **links**! We should link our pages together, so we can go back and forward between all our newly created pages.

Let's do that. Open the *app/views/welcome/__index.html.erb__* file and add the following line:

    <h1>Hello, Rails!</h1>
    <%= link_to 'My Blog', {controller: 'articles'} %>

The **link_to** method is one of Rails' built-in view helpers. It creates a hyperlink based on text to display and where to go - in this case, to the path for articles.

Let's add links to the other views as well, starting with adding a **New Article** link to *app/views/articles/__index.html.erb__*:

    <h1>Listing Articles</h1>
    <%= link_to 'New Article', new_article_path %>
     <table>
      <tr>
        <th>Title</th>
        <th>Text</th>
        <th>Actions</th>
      </tr>
      <% @articles.each do |article| %>
        <tr>
          <td><%= article.title %></td>
          <td><%= article.text %></td>
          <td><%= link_to 'Show', article_path(article) %></td>
        </tr>
      <% end %>
    </table>

Now, add another link in *app/views/articles/__new.html.erb__*, underneath the form, to go back to the index action.

    <h1>New Article</h1>
    <%= form_for :article, url: articles_path do |f| %>
      . . . shortened for brevity . . .
    <% end %>
    <%= link_to 'Back', articles_path %>

Finally, add a link to the *app/views/articles/__show.html.erb__* file to go back to the **index** action as well, so that people who are viewing a single article can go back and view the whole list again:

    <p>
      <strong>Title:</strong>
      <%= @article.title %>
    </p> 
    <p>
      <strong>Text:</strong>
      <%= @article.text %>
    </p>
    <%= link_to 'Back', articles_path %>

And now, you can click back and forth between your web page. Nice work!
