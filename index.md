<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ /:year/:month/:day/:title }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
