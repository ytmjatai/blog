<div class="list-group">
    <a href="#" class="list-group-item disabled">标签</a>
      {% for tag in site.tags reversed %}
        <a class="list-group-item" href="/pages-tags.html#{{ tag[0] }}-ref">
            {{ tag[0] }}
            <span class="tag tag-pill tag-default pull-xs-right">{{ tag[1].size }}</span>
        </a>
        {% endfor %}
</div>