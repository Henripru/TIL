<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ /post.year/post.month/post.day/post.title }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
