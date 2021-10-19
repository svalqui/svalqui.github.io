
{% for item in site.data.menulist.toc %}
    <h3>{{ item.title }}</h3>
      <ul>
        {% for entry in item.subfolderitems %}
          <li><a href="{{ entry.url }}">{{ entry.page }}</a></li>
        {% endfor %}
      </ul>
  {% endfor %}
  
<body>
  <main> 
<nav class="menu">
<ul>
<li><a href="#">Notes</a></li>
  <ul>
    <li><a href="/pages/notes_biopython">Biopython Notes</a></li>
    <li><a href="/pages/notes_linux">Linux Notes</a></li>
    <li><a href="/pages/notes_windows">Windows Notes</a></li>
    <li><a href="/pages/notes_python">Python Notes</a></li>
    <li><a href="/pages/notes-github">Github Notes</a></li>
    <li><a href="/pages/Default-Markdown">Default Markdown sample</a></li>
  </ul>

<li><a href="#">Contact</a></li>
<li><a href="#">About</a></li>
</ul>
</nav>
  </main>

</body>

<!---
[Windows Notes](https://svalqui.github.io/pages/notes_windows)
-->
