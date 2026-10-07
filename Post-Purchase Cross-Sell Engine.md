{% comment %}
============================================================
CUSTOMER.IO — POST-PURCHASE CROSS-SELL ENGINE
============================================================
============================================================
{% endcomment %}

{% assign first_name = customer.first_name | default: "there" %}
{% assign product_category = event.product_category | default: "" %}
{% assign product_name = event.product_name | default: "your recent purchase" %}
{% assign order_value = event.order_value | default: 0 %}


Hi {{ first_name }},

Thanks for purchasing {{ product_name }}.


{% if product_category == "electronics" %}

  {% assign recommendation = "compatible accessories and protection plans" %}

{% elsif product_category == "fashion" %}

  {% assign recommendation = "accessories that complement your purchase" %}

{% elsif product_category == "beauty" %}

  {% assign recommendation = "products that complete your routine" %}

{% elsif product_category == "fitness" %}

  {% assign recommendation = "fitness essentials to support your goals" %}

{% else %}

  {% assign recommendation = "products you may also enjoy" %}

{% endif %}


Based on your purchase, we think you'll love
{{ recommendation }}.


{% if order_value >= 100000 %}

  As a high-value customer, you'll also receive
  priority access to selected offers.

{% endif %}