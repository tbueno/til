# Sequel model hooks

I love [Sequel gem](http://sequel.jeremyevans.net) and how it is build, but its attempt to keep a good compatility with ActiveRecord users might lead other users to mistakes.

One example is the use of `model hooks`. The [documentation on the subject](http://sequel.jeremyevans.net/rdoc/files/doc/model_hooks_rdoc.html) is clear when it says:

    Model hooks, also known as model callbacks, are used to specify actions that occur at a given point in a model instance's lifecycle, such as before or after the model object is saved, created, updated, destroyed, or validated.

In a project i found a model like the one below:

````ruby
class Article
  def after_update
    super
    do_something_asynchronously_after_update
  end
````

And everything was working fine, until I had to extend a piece of funcionality that would update several `articles` under an specific criteria:

````ruby
class Archiver
  def self.archive_old_articles
    Article.where(is_old: true).update(status: Article::Status::ARCHIVED)
  end
end
````

But the pseudo-code above would not trigger the expected model hook, while the following one would:

````ruby
class Archiver
  def self.archive_old_articles
    Article.where(is_old: true).map { |a| 
      a.update(status: Article::Status::ARCHIVED)
    }
  end
end
````

The reason for that is that the `where` method in a model's class will return a `Dataset` object, that has an [update](http://sequel.jeremyevans.net/rdoc/classes/Sequel/Dataset.html#method-i-update) method with a different behaviour. 

The latest version has a gotcha, though. It will perform a SQL `SELECT` followed by multiple `INSERTs`. In a much less performant way than the first one, where a single `INSERT` is needed. It should only be used if the model hook is really necessary.



## More info

- [Model Hooks Documentation](http://sequel.jeremyevans.net/rdoc/files/doc/model_hooks_rdoc.html)
- [Dataset documentation](http://sequel.jeremyevans.net/rdoc/classes/Sequel/Dataset.html)
- [Model documentation](http://sequel.jeremyevans.net/rdoc/classes/Sequel/Model.html)