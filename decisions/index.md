---
nav_order: 2
has_children: true
has_toc: false
---

# Decisions

<h2 class="text-delta">Table of contents</h2>
<table>
<tr>
  <th style="min-width: 60px">ID</th>
  <th>Name</th>
  <th>Status</th>
  <th>Tags</th>
</tr>
{%- assign children_list = site.pages | where: "parent", page.title | where: "grand_parent", page.parent -%}
{% for child in children_list %}
  <tr>
    <td style="min-width: 60px"><a href="{{ child.url | absolute_url }}">ADR-{{child.adr}}</a></td>
    <td>
      {{ child.title }}
    </td>
    <td style="width: 60px">
      {{child.status}} {% if child.summary %} - {{ child.summary }}{% endif %}
    </td>
    <td>{{child.tags | join: ", "}}</td>
  </tr>
{% endfor %}
</table>
