---
layout: post
title:  "How to think about Active Record Associations"
date:   2016-07-23 13:53:17 -0400
---

ActiveRecord associations are pretty intuitive, but one thing that can trip people up is the need for join tables. If you've learned SQL, you know that when there is a many to many assocation between two tables, (meaning for each item in two different tables, there are multiple associated items for every item of both tables), you need a join table to avoid needing an additional additional column. For example, in the bookmark with tags domain, imagine having to do:
<pre>
id               name        bookmark_id_1     bookmark_id_2   bookmark_id_3
1               dev         1                 2             3          
2               math        1                 6                      
</pre>
So you start using ActiveRecord, and it seems to work by magic. You can say that a table `belongs_to` another table in the table in the model and say that a table `has_many` of another table's items with no extra work. So say you have bookmarks and tags. You might think you could just have this:
<pre>
class Bookmark < ActiveRecord::Base
  has_many :tags
 end
...
class Tag < ActiveRecord::Base
  has_many :bookmarks
end
</pre>
But... you go to pick out a bookmark and ask it for its tags and get something like this:
<pre>
>> b = Bookmark.find(57)
=> #<Bookmark id: 57, name: "How to learn Emacs :: The very basics ", link: "http://david.rothlis.net/emacs/tutorial.html", description: nil, secret: nil, user_id: 5, created_at: "2016-07-19 23:33:28", updated_at: "2016-07-19 23:33:28">
>> b.tags
D, [2016-07-23T12:35:17.744758 #26262] DEBUG -- :   Tag Load (0.1ms)  SELECT "tags".* FROM "tags" WHERE "tags"."bookmark_id" = ?  [["bookmark_id", 57]]
ripl: Error while printing result:
ActiveRecord::StatementInvalid: SQLite3::SQLException: no such column: tags.bookmark_id: SELECT "tags".* FROM "tags" WHERE "tags"."bookmark_id" = ?
</pre>
The SQL underneath activerecord rears its head and doesn't know what to do with a many to many relationship unless you use a join table. Luckily, ActiveRecord is smart enough to know that your join table, which in this domain, you should call your join table bookmark_tags, will want to make your many to many relationship happen using two integer foreign keys.

You'll need add this table to your database with a migration like this:
<pre>
create_table :bookmark_tags do |t|
  t.integer :bookmark_id
  t.integer :tag_id
end
</pre>
... and then add this functionality to your models using a has_many, belongs_to relationship between your two primary tables and your join table. Then you'll want a has_many_through relationship between the two primary tables. You'll change these models:
<pre>
class Bookmark < ActiveRecord::Base
  has_many :bookmark_tags
  has_many :tags, through: :bookmark_tags
 end
...
class Tag < ActiveRecord::Base
  has_many :bookmark_tags
  has_many :bookmarks, through: :bookmark_tags 
end
</pre>
... and add a model for the bookmark_tags table:
<pre>
class BookmarkTag < ActiveRecord::Base
  belongs_to :bookmark
  belongs_to :tag
end
</pre>
Now when you go to ask your bookmark about its tag, you'll get a special ActiveRecord array that looks like this: `#<ActiveRecord::Associations::CollectionProxy [#<Tag id: 28, name: "dev", user: nil>]>`. You can now add objects to this list like you would with any other array and then save that association to the database using the save method. 
