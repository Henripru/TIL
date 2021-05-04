# Today I Learned
Here, I document some of the programming topics that I have spent time thinking about and working on.

#  Latest Post
{% for post in site.posts limit:1 %}
  <h1>{{ post.title }}</h1>
  <div>{{ post.content }}</div>
{% endfor %}
<h1>Recent Posts</h1>
{% for post in site.posts offset:1 limit:2 %}
  <h2>{{ post.title }}</h2>
  <div>{{ post.content }}</div>
{% endfor %}

# Older Posts
<ul>
  {% for post in site.posts offset:3 %}
    <li>
      <a href="/TIL{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
