# Today I Learned
This is a place where I put together little summaries of things I learned through the day.

#  Latest Post
{% for post in site.posts limit:1 %}
  <h1>{{ post.title }}</h1>
  <div>{{ post.content }}</div>
{% endfor %}
<h1>Recent Posts</h1>
{% for post in site.posts offset:1 limit:2 %}
  <h1>{{ post.title }}</h1>
  <div>{{ post.content }}</div>
{% endfor %}

# Older Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="/TIL/{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
