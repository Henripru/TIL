<ul>
  {% for post in site.posts %}
    <li>
      <a href="/_posts{{ post.path }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
