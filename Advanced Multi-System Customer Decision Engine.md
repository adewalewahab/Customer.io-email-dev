{% comment %}
============================================================
CUSTOMER.IO — MULTI-SYSTEM CUSTOMER DECISION ENGINE
============================================================

SYSTEMS:
1. Customer Profile
2. E-commerce
3. Subscription
4. Loyalty
5. Support
6. Behavioral Events
============================================================
{% endcomment %}


{% comment %}
------------------------------------------------------------
1. CUSTOMER PROFILE
------------------------------------------------------------
{% endcomment %}

{% assign first_name =
  customer.first_name | default: "there"
%}

{% assign customer_status =
  customer.status | default: "active"
%}

{% assign customer_type =
  customer.customer_type | default: "standard"
%}

{% assign lifetime_value =
  customer.lifetime_value | default: 0
%}


{% comment %}
------------------------------------------------------------
2. E-COMMERCE DATA
------------------------------------------------------------
{% endcomment %}

{% assign order_count =
  customer.order_count | default: 0
%}

{% assign cart_value =
  customer.cart_value | default: 0
%}

{% assign last_order_status =
  customer.last_order_status | default: ""
%}


{% comment %}
------------------------------------------------------------
3. SUBSCRIPTION DATA
------------------------------------------------------------
{% endcomment %}

{% assign subscription_status =
  customer.subscription_status | default: "none"
%}

{% assign subscription_plan =
  customer.subscription_plan | default: ""
%}


{% comment %}
------------------------------------------------------------
4. LOYALTY DATA
------------------------------------------------------------
{% endcomment %}

{% assign loyalty_tier =
  customer.loyalty_tier | default: "standard"
%}

{% assign loyalty_points =
  customer.loyalty_points | default: 0
%}


{% comment %}
------------------------------------------------------------
5. SUPPORT DATA
------------------------------------------------------------
{% endcomment %}

{% assign support_status =
  customer.support_status | default: "none"
%>


{% comment %}
------------------------------------------------------------
6. DETERMINE CUSTOMER VALUE
------------------------------------------------------------
{% endcomment %}

{% if lifetime_value >= 1000000 %}

  {% assign value_segment = "VIP" %}

{% elsif lifetime_value >= 500000 %}

  {% assign value_segment = "HIGH_VALUE" %}

{% elsif lifetime_value >= 100000 %}

  {% assign value_segment = "ACTIVE" %}

{% else %}

  {% assign value_segment = "STANDARD" %}

{% endif %}


{% comment %}
------------------------------------------------------------
7. COMMUNICATION PRIORITY
------------------------------------------------------------
Highest-priority situation wins.
------------------------------------------------------------
{% endcomment %}

{% assign communication = "GENERAL" %}


{% if customer_status != "active" %}

  {% assign communication = "SUPPRESS" %}

{% elsif support_status == "open" %}

  {% assign communication = "SUPPORT" %}

{% elsif last_order_status == "failed" %}

  {% assign communication = "PAYMENT_RECOVERY" %}

{% elsif subscription_status == "cancelled" %}

  {% assign communication = "WINBACK" %}

{% elsif cart_value > 0 %}

  {% assign communication = "CART_RECOVERY" %}

{% elsif value_segment == "VIP" %}

  {% assign communication = "VIP" %}

{% elsif loyalty_tier == "Gold" or loyalty_tier == "Platinum" %}

  {% assign communication = "LOYALTY" %}

{% elsif order_count == 0 %}

  {% assign communication = "ONBOARDING" %}

{% endif %}


{% comment %}
------------------------------------------------------------
8. FINAL MESSAGE
------------------------------------------------------------
{% endcomment %}

{% if communication == "SUPPRESS" %}

  {# No communication should be sent #}


{% elsif communication == "SUPPORT" %}

  Hi {{ first_name }},

  We have an update regarding your support request.

  Our team is currently working on it.


{% elsif communication == "PAYMENT_RECOVERY" %}

  Hi {{ first_name }},

  We couldn't complete your recent transaction.

  Please review your payment details and try again.


{% elsif communication == "WINBACK" %}

  Hi {{ first_name }},

  We'd love to have you back.

  Here's something special to help you get started again.


{% elsif communication == "CART_RECOVERY" %}

  Hi {{ first_name }},

  You still have items waiting in your cart.

  Complete your purchase whenever you're ready.


{% elsif communication == "VIP" %}

  Hi {{ first_name }},

  As one of our VIP customers, you've unlocked
  exclusive benefits.


{% elsif communication == "LOYALTY" %}

  Hi {{ first_name }},

  Your {{ loyalty_tier }} membership comes with
  exclusive benefits.

  You currently have {{ loyalty_points }} points.


{% elsif communication == "ONBOARDING" %}

  Hi {{ first_name }},

  Welcome!

  Let's get you started.


{% else %}

  Hi {{ first_name }},

  We have something new we think you'll love.

{% endif %}