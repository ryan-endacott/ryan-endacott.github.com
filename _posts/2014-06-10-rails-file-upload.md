---
layout: post
title:  "Upload Files to Database in Rails 4 without Paperclip"
date:   2014-06-10 17:29:17

---

While the [Paperclip](https://github.com/thoughtbot/paperclip) gem is awesome for most Rails use cases, it doesn't have support for saving files to a database. In some scenarios, access to the filesystem or an external service like [Amazon S3](http://aws.amazon.com/s3/) isn't feasible. Or maybe you just want to put your files in the database.

As it happens, file upload in vanilla Rails is simple.

First, create a model that will store the file. Give it the following attributes:

- `filename:string`
- `content_type:string`
- `file_contents:binary`

Note: To quickly scaffold the model to save some keystrokes, do `rails g scaffold document filename:string content_type:string file_contents:binary` in your terminal.

Here's the code for the model and migration:

{% highlight ruby %}

# app/models/document.rb
class Document < ActiveRecord::Base
end

# db/migrate/20140602005921_create_documents.rb
class CreateDocuments < ActiveRecord::Migration
  def change
    create_table :documents do |t|
      t.string :filename
      t.string :content_type
      t.binary :file_contents

      t.timestamps
    end
  end
end

{% endhighlight %}

Next, add a file input to the model's form.

{% highlight html %}

<!-- app/views/documents/_form.html.erb -->
<%= form_for(@document) do |f| %>
  <div class="field">
    <%= f.file_field :file %>
  </div>
  <div class="actions">
    <%= f.submit %>
  </div>
<% end %>

{% endhighlight %}

Next, you need to make sure you allow the file as a parameter so you can use it in the model.

{% highlight ruby %}

# app/controllers/documents_controller.rb
def document_params
  params.require(:document).permit(:file)
end

{% endhighlight %}

Finally, update the model to read and save the file. In Rails, a file input is passed in as an [UploadedFile](http://api.rubyonrails.org/classes/ActionDispatch/Http/UploadedFile.html) that can be treated like a normal file. I prefer to save the file in the model initialization.

{% highlight ruby %}

# app/models/document.rb
def initialize(params = {})
  file = params.delete(:file)
  super
  if file
    self.filename = sanitize_filename(file.original_filename)
    self.content_type = file.content_type
    self.file_contents = file.read
  end
end
private
  def sanitize_filename(filename)
    # Get only the filename, not the whole path (for IE)
    # Thanks to this article I just found for the tip: http://mattberther.com/2007/10/19/uploading-files-to-a-database-using-rails
    return File.basename(filename)
  end

{% endhighlight %}

So now you have an application that users can upload files to, but they still can't download them. We can make the `show` action of the `documents_controller` download the file. To do so, simply send the file data saved in the document.

{% highlight ruby %}

def show
  send_data(@document.file_contents,
            type: @document.content_type,
            filename: @document.filename)
end

{% endhighlight %}

You've now seen everything it takes to make file upload and download work in vanilla Rails! If you want to do validations on attributes like file size, you'll need to make a slight change to the initialization in the model so the file object can be accessed in the validations.

{% highlight ruby %}

# app/models/document.rb

validate :file_size_under_one_mb

def initialize(params = {})
  # File is now an instance variable so it can be
  # accessed in the validation.
  @file = params.delete(:file)
  super
  if @file
    self.filename = sanitize_filename(@file.original_filename)
    self.content_type = @file.content_type
    self.file_contents = @file.read
  end
end

NUM_BYTES_IN_MEGABYTE = 1048576
def file_size_under_one_mb
  if (@file.size.to_f / NUM_BYTES_IN_MEGABYTE) > 1
    errors.add(:file, 'File size cannot be over one megabyte.')
  end
end

{% endhighlight %}

I've created an [example file upload application](http://railsfileupload.herokuapp.com/) showing everything in action together. The source is on [GitHub](https://github.com/ryan-endacott/Rails-File-Upload-Example).

If you enjoyed this article, follow me on [Twitter](https://twitter.com/RyanEndacott) for more like it. Happy hacking!

