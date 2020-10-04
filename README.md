# Micro Reddit

A small exercise dealing with model generations and associations. Created as part of the Odin Project curriculum.

### Thoughts

During this project I experimented with adding generator parameters.

`rails g model post title:string:uniq author:belongs_to` created the following: (`belongs_to` is an alias for `references` and `uniq` creates a unique index). 

```ruby
class CreatePosts < ActiveRecord::Migration[6.0]
  def change
    create_table :posts do |t|
      t.string :title
      t.text :body
      t.belongs_to :author, null: false, foreign_key: { to_table: :users }

      t.timestamps
    end
    add_index :posts, :title, unique: true
  end
end

```

Since I decided to name the `belongs_to` association `author` even though the referenced model is `User`, I needed to add the clarifying table name, since Rails is unable to infer it. That is the one change I made before migrating.

For the `Comment` model I entered `rails g model comment body:text author:references post:references`, using `references` instead of `belongs_to`. That created:

```ruby
class CreateComments < ActiveRecord::Migration[6.0]
  def change
    create_table :comments do |t|
      t.text :body
      t.references :author, null: false, foreign_key: { to_table: :users }
      t.references :post, null: false, foreign_key: true

      t.timestamps
    end
  end
end

```

again, the only change was for the author column — `foreign_key: true` to `foreign_key: { to_table: :users }`.