*   Add `:day_format` option to `date_select`

        date_select("article", "written_on", day_format: ->(day) { day.ordinalize })
        # generates day options like <option value="1">1st</option>\n<option value="2">2nd</option>...

    *Shunichi Ikegami*

*   Allow `link_to` helper to infer link name from `Model#to_s` when it
    is used with a single argument:

        link_to @profile
        #=> <a href="/profiles/1">Eileen</a>

    This assumes the model class implements a `to_s` method like this:

        class Profile < ApplicationRecord
          # ...
          def to_s
            name
          end
        end

    Previously you had to supply a second argument even if the `Profile`
    model implemented a `#to_s` method that called the `name` method.

        link_to @profile, @profile.name
        #=> <a href="/profiles/1">Eileen</a>

    *Olivier Lacan*

*   Support svg unpaired tags for `tag` helper.

        tag.svg { tag.use('href' => "#cool-icon") }
        # => <svg><use href="#cool-icon"></svg>

    *Oleksii Vasyliev*


## Rails 7.0.0.alpha2 (September 15, 2021) ##

*   No changes.


## Rails 7.0.0.alpha1 (September 15, 2021) ##

*   Improves the performance of ActionView::Helpers::NumberHelper formatters by avoiding the use of
    exceptions as flow control.

    *Mike Dalessio*

*   `preload_link_tag` properly inserts `as` attributes for files with `image` MIME types, such as JPG or SVG.

    *Nate Berkopec*

*   Add `weekday_options_for_select` and `weekday_select` helper methods. Also adds `weekday_select` to `FormBuilder`.

    *Drew Bragg*, *Dana Kashubeck*, *Kasper Timm Hansen*

*   Add `caching?` helper that returns whether the current code path is being cached and `uncacheable!` to denote helper methods that can't participate in fragment caching.

    *Ben Toews*, *John Hawthorn*, *Kasper Timm Hansen*, *Joel Hawksley*

*   Add `include_seconds` option for `time_field`.

        <%= form.time_field :foo, include_seconds: false %>
        # => <input value="16:22" type="time" />

    Default includes seconds:

        <%= form.time_field :foo %>
        # => <input value="16:22:01.440" type="time" />

    This allows you to take advantage of [different rendering options](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/time#time_value_format) in some browsers.

    *Alex Ghiculescu*

*   Improve error messages when template file does not exist at absolute filepath.

    *Ted Whang*

*   Add `:country_code` option to `sms_to` for consistency with `phone_to`.

    *Jonathan Hefner*

*   OpenSSL constants are now used for Digest computations.

    *Dirkjan Bussink*

*   The `translate` helper now passes `default` values that aren't
    translation keys through `I18n.translate` for interpolation.

    *Jonathan Hefner*

*   Adds option `extname` to `stylesheet_link_tag` to skip default
    `.css` extension appended to the stylesheet path.

    Before:

    ```ruby
    stylesheet_link_tag "style.less"
    # <link href="/stylesheets/style.less.scss" rel="stylesheet">
    ```

    After:

    ```ruby
    stylesheet_link_tag "style.less", extname: false, skip_pipeline: true, rel: "stylesheet/less"
    # <link href="/stylesheets/style.less" rel="stylesheet/less">
    ```

    *Abhay Nikam*

*   Deprecate `render` locals to be assigned to instance variables.

    *Petrik de Heus*

*   Remove legacy default `media=screen` from `stylesheet_link_tag`.

    *André Luis Leal Cardoso Junior*

*   Change `ActionView::Helpers::FormBuilder#button` to transform `formmethod`
    attributes into `_method="$VERB"` Form Data to enable varied same-form actions:

        <%= form_with model: post, method: :put do %>
          <%= form.button "Update" %>
          <%= form.button "Delete", formmethod: :delete %>
        <% end %>
        <%# => <form action="posts/1">
            =>   <input type="hidden" name="_method" value="put">
            =>   <button type="submit">Update</button>
            =>   <button type="submit" formmethod="post" name="_method" value="delete">Delete</button>
            => </form>
        %>

    *Sean Doyle*

*   Change `ActionView::Helpers::UrlHelper#button_to` to *always* render a
    `<button>` element, regardless of whether or not the content is passed as
    the first argument or as a block.

        <%= button_to "Delete", post_path(@post), method: :delete %>
        # => <form action="/posts/1"><input type="hidden" name="_method" value="delete"><button type="submit">Delete</button></form>

        <%= button_to post_path(@post), method: :delete do %>
          Delete
        <% end %>
        # => <form action="/posts/1"><input type="hidden" name="_method" value="delete"><button type="submit">Delete</button></form>

    *Sean Doyle*, *Dusan Orlovic*

*   Add `config.action_view.preload_links_header` to allow disabling of
    the `Link` header being added by default when using `stylesheet_link_tag`
    and `javascript_include_tag`.

    *Andrew White*

*   The `translate` helper now resolves `default` values when a `nil` key is
    specified, instead of always returning `nil`.

    *Jonathan Hefner*

*   Add `config.action_view.image_loading` to configure the default value of
    the `image_tag` `:loading` option.

    By setting `config.action_view.image_loading = "lazy"`, an application can opt in to
    lazy loading images sitewide, without changing view code.

    *Jonathan Hefner*

*   `ActionView::Helpers::FormBuilder#id` returns the value
    of the `<form>` element's `id` attribute. With a `method` argument, returns
    the `id` attribute for a form field with that name.

        <%= form_for @post do |f| %>
          <%# ... %>

          <% content_for :sticky_footer do %>
            <%= form.button(form: f.id) %>
          <% end %>
        <% end %>

    *Sean Doyle*

*   `ActionView::Helpers::FormBuilder#field_id` returns the value generated by
    the FormBuilder for the given attribute name.

        <%= form_for @post do |f| %>
          <%= f.label :title %>
          <%= f.text_field :title, aria: { describedby: f.field_id(:title, :error) } %>
          <%= tag.span("is blank", id: f.field_id(:title, :error) %>
        <% end %>

    *Sean Doyle*

*   Add `tag.attributes` to transform a Hash into HTML Attributes, ready to be
    interpolated into ERB.

        <input <%= tag.attributes(type: :text, aria: { label: "Search" }) %> >
        # => <input type="text" aria-label="Search">

    *Sean Doyle*


Please check [6-1-stable](https://github.com/rails/rails/blob/6-1-stable/actionview/CHANGELOG.md) for previous changes.
