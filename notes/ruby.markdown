---
title: Notes about Ruby
layout: page
permalink: /notes/ruby/
---

[Ruby](http://www.ruby-lang.org/) is a dynamic, reflective, general-purpose object-oriented programming language with a focus on simplicity and productivity.

## Mime detection by file content

When you have file which have some like random name such as `1f7c9541e62534830b8723c0514150ce.bin`, how do we know about the `mime type` of this kind of file?

Thanks to [ruby-filemagic](https://github.com/blackwinter/ruby-filemagic) gem. This gem can detect file's mime type by its content. As usual you can install the gem by issuing on your command line:

{% highlight bash %}
$ gem install ruby-filemagic
{% endhighlight %}

And here's how to do the magic :)

{% highlight ruby %}
require 'filemagic'
require 'mime/types'

fp   = "1f7c9541e62534830b8723c0514150ce.bin"
fm   = FileMagic.new(FileMagic::MAGIC_MIME)
mime = fm.file(fp)

# > puts mime
# audio/mpeg; charset=binary
{% endhighlight %}

Now you can see that the file is `audio/mpeg; charset=binary`. To get the file extensions for this mime we can use:

{% highlight ruby %}
mime_details = MIME::Types[mime]

# > puts mime_details
# audio/mpeg
#
# > puts file_extension[0].extensions
# mpga
# mp2
# mp3
{% endhighlight %}

Now you can choose one of the `mpga`, `mp2`, or `mp3` as the file extension for the file.

## Small reminders

Here's listing of small snippets that sometimes forgotten, the listing will be grown over the time.

### Cut string from a position to end of the string

{% highlight ruby %}
> "abcdefgh"[1..-1]
=> "bcdefgh"
> "abcdefgh"[5..-1]
=> "fgh"
{% endhighlight %}

## Generate QR code

Creating [QR code](http://en.wikipedia.org/wiki/QR_code) in Ruby is pretty easy. Put on your `Gemfile`:

{% highlight ruby %}
gem 'rqrcode'
gem 'barby'
{% endhighlight %}

And then you can use on your Ruby script:

{% highlight ruby %}
require 'barby/barcode/qr_code'
require 'barby/outputter/svg_outputter'

barcode     = Barby::QrCode.new("http://subosito.com/")
output      = Barby::SvgOutputter.new(barcode)
output.xdim = 6
output.to_svg
{% endhighlight %}

And the output will looks like:

<figure class="qr-demo">
  {% include qr.html %}
</figure>

I use `SVG` output for the demo, you can find another outputter on [Barby's GitHub](https://github.com/toretore/barby).

## Unique filename on carrierwave

[Carrierwave](https://github.com/carrierwaveuploader/carrierwave) is a file uploads solution for Ruby web frameworks. By default it's using original filename for its uploaded file. To generate unique name for uploaded file, you can use code below, say, your filename `Sujiwo-teDjo.jpg`:

{% highlight ruby %}
class AvatarUploader < CarrierWave::Uploader::Base
  def filename
    if original_filename
      hash = Digest::MD5.hexdigest(File.dirname(current_path))
      "#{model.name.parameterize}-#{hash}.#{file.extension}"
    end
  end
end
{% endhighlight %}

You'll get filename like:

    sujiwo-tedjo-e2f4f80de88190560e3333f6273e40dc.jpg

