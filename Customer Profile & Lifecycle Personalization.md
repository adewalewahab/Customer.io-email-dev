{% comment %}
============================================================
CUSTOMER.IO — PROFILE & LIFECYCLE PERSONALIZATION
============================================================
{% endcomment %}

{% assign first_name = customer.first_name | default: "there" %}
{% assign customer_type = customer.customer_type | default: "standard" %}
{% assign lifecycle_stage = customer.lifecycle_stage | default: "new" %}
{% assign country = customer.country | default: "" %}
{% assign loyalty_tier = customer.loyalty_tier | default: "standard" %}


{% if lifecycle_stage == "new" %}

  {% assign message_type = "welcome" %}

{% elsif lifecycle_stage == "active" %}

  {% assign message_type = "engagement" %}

{% elsif lifecycle_stage == "at_risk" %}

  {% assign message_type = "retention" %}

{% elsif lifecycle_stage == "churned" %}

  {% assign message_type = "winback" %}

{% else %}

  {% assign message_type = "general" %}

{% endif %}


{% if message_type == "welcome" %}

  Hi {{ first_name }},

  Welcome! We're excited to have you with us.

{% elsif message_type == "engagement" %}

  Hi {{ first_name }},

  Here's what's new and exciting for you.

{% elsif message_type == "retention" %}

  Hi {{ first_name }},

  We noticed you haven't been around recently.

  We'd love to have you back.

{% elsif message_type == "winback" %}

  Hi {{ first_name }},

  It's been a while.

  Here's something special to welcome you back.

{% else %}

  Hi {{ first_name }},

  We have something new for you.

{% endif %}


{% if loyalty_tier == "gold" %}

  As a Gold member, you have access to exclusive benefits.

{% elsif loyalty_tier == "platinum" %}

  As a Platinum member, you have access to our highest-tier benefits.

{% endif %}