---
layout: page
title: Links
tagline: リンク一覧
permalink: /links.html
---


{% for f in site.data.links %}
<div class="link-chip-div"><a href="{{f.url}}" target="_blank" class="link-chip ripple">
 <img alt="{{f.describe}}" src="{{f.image}}" class="link-chip-icon"/>
 {% if f.skin %}<img style="filter:opacity(0.8);float:right;height:64px;margin-right:-8px" src="{{f.skin}}" />{% endif %}
 <span title="{{f.describe}}" class="link-chip-title">{{f.name}}</span>
 <p class="link-chip-dc">{{f.describe}}</p></a></div>
{% endfor %}

[Top Page]({{ site.url }})

<hr/>
