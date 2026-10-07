{% comment %}
============================================================
CUSTOMER.IO — CHURN PREVENTION ENGINE
============================================================
============================================================
{% endcomment %}

{% assign first_name = customer.first_name | default: "there" %}

{% assign engagement_score =
  customer.engagement_score | default: 0
%}

{% assign order_count =
  customer.order_count | default: 0
%}

{% assign lifetime_value =
  customer.lifetime_value | default: 0
%}

{% assign days_since_activity =
  customer.days_since_activity | default: 0
%}


{% comment %}
------------------------------------------------------------
CALCULATE RISK
------------------------------------------------------------
{% endcomment %}

{% assign risk_score = 0 %}


{% if days_since_activity >= 90 %}

  {% assign risk_score = risk_score | plus: 50 %}

{% elsif days_since_activity >= 60 %}

  {% assign risk_score = risk_score | plus: 35 %}

{% elsif days_since_activity >= 30 %}

  {% assign risk_score = risk_score | plus: 20 %}

{% endif %}


{% if engagement_score < 20 %}

  {% assign risk_score = risk_score | plus: 30 %}

{% elsif engagement_score < 50 %}

  {% assign risk_score = risk_score | plus: 15 %}

{% endif %}


{% if risk_score >= 70 %}

  {% assign risk_level = "critical" %}

{% elsif risk_score >= 40 %}

  {% assign risk_level = "high" %}

{% elsif risk_score >= 20 %}

  {% assign risk_level = "medium" %}

{% else %}

  {% assign risk_level = "low" %}

{% endif %}


{% comment %}
------------------------------------------------------------
MESSAGE STRATEGY
------------------------------------------------------------
{% endcomment %}

{% if risk_level == "critical" %}

  Hi {{ first_name }},

  We miss you.

  Here's an exclusive offer to welcome you back.


{% elsif risk_level == "high" %}

  Hi {{ first_name }},

  It's been a while since we've seen you.

  We thought you might want to see what's new.


{% elsif risk_level == "medium" %}

  Hi {{ first_name }},

  Here's something we think you'll enjoy.


{% else %}

  Hi {{ first_name }},

  Thanks for being an active customer.

{% endif %}