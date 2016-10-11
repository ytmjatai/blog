<div class="list-group">
    <a href="#" class="list-group-item disabled">最新文章</a>
    {% for post in site.posts limit:10 %}<a href="{{ post.url }}"  class="list-group-item">{{ post.title }}</a>{% endfor %}
</div>