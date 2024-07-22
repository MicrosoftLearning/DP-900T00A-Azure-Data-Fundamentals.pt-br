---
title: Instruções online hospedadas
permalink: index.html
layout: home
---

# Exercícios do Azure Data Fundamentals

Esses exercícios práticos foram projetados para dar suporte ao conteúdo de treinamento no [Microsoft Learn](https://docs.microsoft.com/training/).

Para concluir esses exercícios, você precisará de uma assinatura do Microsoft Azure na qual tenha permissões administrativas. Inscreva-se para uma avaliação gratuita em [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Exercício |
| --- |
{% for activity in labs  %}| [{{ activity.lab.title }}{% if activity.lab.type %} — {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
