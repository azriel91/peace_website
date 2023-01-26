<div id="{{ id }}" style="
    display: inline-block;
    max-width: 440px;
    margin-left: 10px;
    margin-right: 10px;
    vertical-align: middle;
    {% if fixed_width -%}
    width: 420px;
    overflow-x: hidden;
    {% endif -%}
">

{% if body %}{{ body }}{% endif %}

</div>
