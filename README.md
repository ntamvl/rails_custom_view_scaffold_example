# Rails Custom View Scaffold Example
Customize view scaffolding template using Bootstrap 5

We will do in this example:
- Create new new the Ruby on Rails project using version 6.1.1
- Add Boostrap version 5.0.0-beta1
- Generate scaffold for category and article 
- Run project and see the result

## Create new project
```
cd
rails _6.1.1_ new rails_custom_view_scaffold_example
```

## Locate the existing template

The existing scaffolding templates are under the railties directory, located in:

```bash
[railties directory path]/lib/rails/generators/erb/scaffold/templates
```

Run this command to show where railties is located

```bash
bundle info railties

OR

bundle show railties
```

**Example in my pc:**

```bash
bundle info railties
  * railties (6.1.1)
  Summary: Tools for creating, working with, and running Rails applications.
  Homepage: https://rubyonrails.org
  Documentation: https://api.rubyonrails.org/v6.1.1/
  Source Code: https://github.com/rails/rails/tree/v6.1.1/railties
  Changelog: https://github.com/rails/rails/blob/v6.1.1/railties/CHANGELOG.md
  Bug Tracker: https://github.com/rails/rails/issues
  Mailing List: https://discuss.rubyonrails.org/c/rubyonrails-talk
  Path: /Users/tamnguyen/.rbenv/versions/3.0.0/lib/ruby/gems/3.0.0/gems/railties-6.1.1
```

We found railties path is `/Users/tamnguyen/.rbenv/versions/3.0.0/lib/ruby/gems/3.0.0/gems/railties-6.1.1`

Then we get the scaffolding templates path:

```bash
/Users/tamnguyen/.rbenv/versions/3.0.0/lib/ruby/gems/3.0.0/gems/railties-6.1.1//lib/rails/generators/erb/scaffold/templates
```

**List folder in the existing scaffold template:**
```bash
ls -l /Users/tamnguyen/.rbenv/versions/3.0.0/lib/ruby/gems/3.0.0/gems/railties-6.1.1//lib/rails/generators/erb/scaffold/templates
total 40
drwxr-xr-x  7 tamnguyen  staff   224 Jan  8 10:01 .
drwxr-xr-x  4 tamnguyen  staff   128 Jan  8 10:01 ..
-rw-rw-r--  1 tamnguyen  staff  1195 Jan  8 10:01 _form.html.erb.tt
-rw-rw-r--  1 tamnguyen  staff   237 Jan  8 10:01 edit.html.erb.tt
-rw-rw-r--  1 tamnguyen  staff   974 Jan  8 10:01 index.html.erb.tt
-rw-rw-r--  1 tamnguyen  staff   179 Jan  8 10:01 new.html.erb.tt
-rw-rw-r--  1 tamnguyen  staff   903 Jan  8 10:01 show.html.erb.tt
```

## Copy and customize the scaffold template
We should copy the existing scaffold template to path `lib/templates/erb/scaffold` in the project folder
should we have the lib folder in the project:
```
lib/templates/erb/scaffold/_form.html.erb.tt
lib/templates/erb/scaffold/edit.html.erb.tt
lib/templates/erb/scaffold/index.html.erb.tt
lib/templates/erb/scaffold/new.html.erb.tt
lib/templates/erb/scaffold/show.html.erb.tt
```

**Following these commands to get this:**
```
mkdir -p lib/templates/erb/scaffold
cp /Users/tamnguyen/.rbenv/versions/3.0.0/lib/ruby/gems/3.0.0/gems/railties-6.1.1//lib/rails/generators/erb/scaffold/templates/*.* lib/templates/erb/scaffold
```

*Then we open files `*.erb.tt` to customize base bootstrap*

**File _form.html.erb.tt**
```erb
<%%= form_with(model: <%= model_resource_name %>) do |form| %>
  <%% if <%= singular_table_name %>.errors.any? %>
    <div id="error_explanation">
      <h2><%%= pluralize(<%= singular_table_name %>.errors.count, "error") %> prohibited this <%= singular_table_name %> from being saved:</h2>

      <ul>
        <%% <%= singular_table_name %>.errors.each do |error| %>
          <li><%%= error.full_message %></li>
        <%% end %>
      </ul>
    </div>
  <%% end %>

<% attributes.each do |attribute| -%>
  <div class="form-group mb-3">
<% if attribute.password_digest? -%>
    <%%= form.label :password, class: "form-label" %>
    <%%= form.password_field :password, class:"form-control" %>
  </div>

  <div class="form-group mb-3">
    <%%= form.label :password_confirmation, class: "form-label" %>
    <%%= form.password_field :password_confirmation, class:"form-control" %>
<% elsif attribute.attachments? -%>
    <%%= form.label :<%= attribute.column_name %>, class: "form-label" %>
    <%%= form.<%= attribute.field_type %> :<%= attribute.column_name %>, multiple: true %>
<% elsif attribute.type == :boolean -%>
    <%%= form.label :<%= attribute.column_name %>, class: "form-check-label" %>
    <%%= form.<%= attribute.field_type %> :<%= attribute.column_name %>, class:"form-check-input" %>
<% elsif attribute.reference? -%>
    <%%= form.label :<%= attribute.column_name %> %>
    <%%= form.collection_select :<%= attribute.column_name %>, <%= attribute.name.camelize %>.all, :id, :name, { prompt: true }, { class: "form-control" }  %>
<% else -%>
    <%%= form.label :<%= attribute.column_name %>, class: "form-label" %>
    <%%= form.<%= attribute.field_type %> :<%= attribute.column_name %>, class:"form-control" %>
<% end -%>
  </div>

<% end -%>
  <div class="form-group mb-3">
    <%%= form.submit class: 'btn btn-primary' %>
  </div>
<%% end %>

```

**File edit.html.erb.tt**
```erb
<div class="container-fluid">
  <h1>Editing <%= singular_table_name.titleize %></h1>

  <%%= render 'form', <%= singular_table_name %>: @<%= singular_table_name %> %>

  <%%= link_to 'Show', @<%= singular_table_name %>, class: "btn btn-light min-width-btn" %> |
  <%%= link_to 'Back', <%= index_helper %>_path, class: "btn btn-light min-width-btn" %>
</div>

```

**File index.html.erb.tt**
```erb
<div class="container-fluid">
  <p id="notice"><%%= notice %></p>

  <h1><%= plural_table_name.titleize %></h1>

  <table class="table">
    <thead>
      <tr>
  <% attributes.reject(&:password_digest?).each do |attribute| -%>
        <th><%= attribute.human_name %></th>
  <% end -%>
        <th colspan="3"></th>
      </tr>
    </thead>

    <tbody>
      <%% @<%= plural_table_name %>.each do |<%= singular_table_name %>| %>
        <tr>
  <% attributes.reject(&:password_digest?).each do |attribute| -%>
          <td><%%= <%= singular_table_name %>.<%= attribute.column_name %> %></td>
  <% end -%>
          <td><%%= link_to 'Show', <%= model_resource_name %> %></td>
          <td><%%= link_to 'Edit', edit_<%= singular_route_name %>_path(<%= singular_table_name %>) %></td>
          <td><%%= link_to 'Destroy', <%= model_resource_name %>, method: :delete, data: { confirm: 'Are you sure?' } %></td>
        </tr>
      <%% end %>
    </tbody>
  </table>

  <br>

  <%%= link_to 'New <%= singular_table_name.titleize %>', new_<%= singular_route_name %>_path, class: "btn btn-success min-width-btn" %>

</div>

```

**File new.html.erb.tt**
```erb
<div class="container-fluid">
  <h1>New <%= singular_table_name.titleize %></h1>

  <%%= render 'form', <%= singular_table_name %>: @<%= singular_table_name %> %>

  <%%= link_to 'Back', <%= index_helper %>_path, class: "btn btn-light min-width-btn" %>
</div>

```

**File show.html.erb.tt**
```erb
<div class="container-fluid">
  <p id="notice"><%%= notice %></p>

  <% attributes.reject(&:password_digest?).each do |attribute| -%>
  <% if attribute.attachment? -%>
  <div class="form-group mb-3">
    <p><strong><%= attribute.human_name %>:</strong></p>
    <%%= link_to @<%= singular_table_name %>.<%= attribute.column_name %>.filename, @<%= singular_table_name %>.<%= attribute.column_name %> if @<%= singular_table_name %>.<%= attribute.column_name %>.attached? %>
  </div>
  <% elsif attribute.attachments? -%>
  <div class="form-group mb-3">
    <p><strong><%= attribute.human_name %>:</strong></p>
    <%% @<%= singular_table_name %>.<%= attribute.column_name %>.each do |<%= attribute.singular_name %>| %>
      <div><%%= link_to <%= attribute.singular_name %>.filename, <%= attribute.singular_name %> %></div>
    <%% end %>
  </div>
  <% else -%>
  <div class="form-group mb-3">
    <p><strong><%= attribute.human_name %>:</strong></p>
    <div>
      <%%= @<%= singular_table_name %>.<%= attribute.column_name %> %>
    </div>
  </div>
  <% end -%>

  <% end -%>

  <%%= link_to 'Edit', edit_<%= singular_table_name %>_path(@<%= singular_table_name %>), class: "btn btn-primary min-width-btn" %>
  <%%= link_to 'Back', <%= index_helper %>_path, class: "btn btn-light min-width-btn" %>
</div>

```

## Add Bootstrap 5 to the project

Run this command to add bootstrap
```
yarn add bootstrap@5.0.0-beta1 @popperjs/core
```

Edit `app/javascript/packs/application.js`:
```js
import Rails from "@rails/ujs"
import Turbolinks from "turbolinks"
import * as ActiveStorage from "@rails/activestorage"
import "channels"

Rails.start()
Turbolinks.start()
ActiveStorage.start()

// Add these lines
import "bootstrap"
import "bootstrap/scss/bootstrap.scss"
```

## Generate scaffold for Category and Article
Generate new views for the restaurant model overriding the original ones.

```
rails g scaffold Category name description:text
rails g scaffold Article title content:text category:references active:boolean

```

**Then run migration command**

```
bundle exec rails db:create db:migrate
bundle exec rails s
```

and see the result at the address
```
http://localhost:3000/categories
http://localhost:3000/articles
```

## If you want to run this example quickly

```
cd 
git clone https://github.com/ntamvl/rails_custom_view_scaffold_example 
cd rails_custom_view_scaffold_example
bundle install && yarn install 
rails db:create db:migrate 
rails s
```

Enjoy :D :) <3

---
Cheers, and happy coding!

I also published this example at my blog [https://ntam.me/customize-view-scaffolding-template-using-bootstrap-5/](https://ntam.me/customize-view-scaffolding-template-using-bootstrap-5/)

If you have questions or comments about this blog post, you can get in touch with me on Twitter [@nguyentamvn](https://twitter.com/nguyentamvn)
