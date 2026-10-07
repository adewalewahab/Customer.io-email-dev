{% comment %}
============================================================
CUSTOMER.IO — ABANDONED CART PERSONALIZATION
============================================================
Expected event data:
event.cart_id
event.items
event.total
============================================================
{% endcomment %}

{% assign first_name = customer.first_name | default: "there" %}
{% assign cart_total = event.total | default: 0 %}
{% assign cart_url = event.cart_url | default: "" %}


Hi {{ first_name }},

You left a few items in your cart.


{% if event.items %}

  {% for item in event.items %}

    {{ item.name }}

    {% if item.quantity > 1 %}
      Quantity: {{ item.quantity }}
    {% endif %}

    {% if item.price %}
      Price: {{ item.price }}
    {% endif %}

  {% endfor %}

{% endif %}


{% if cart_total > 100000 %}

  You qualify for free delivery on this order.

{% elsif cart_total > 50000 %}

  You're close to qualifying for free delivery.

{% endif %}


{% if cart_url != "" %}

  Complete your purchase:
  {{ cart_url }}

{% endif %}