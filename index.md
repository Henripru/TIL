<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.path }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
