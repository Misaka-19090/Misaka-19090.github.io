---
title: "oTree Programming Tips"
excerpt: "Technical suggestions that may be helpful when programming your oTree experiment."
collection: portfolio
---

## The frontend (HTML, CSS, JavaScript)

1. __General advice for constructing and organising webpages.__
    - Adopt an existent framework to simplify the development of informative webpages, for example, [Bootstrap](https://getbootstrap.com/).
    - Alternatively, if you do need to modify some webpage elements: 
        - Avoid writing the styles directly inside the element tag like this:
            ```css
            <element style="..."></element>
            ```
        - Instead, assign a class name for each style and manage the styles uniformly in CSS files under the folder `./static/`. When you are going to apply the styles on certain page element, first link the style sheet in the page head:
            ```css
            <link rel="stylesheet" type="text/css" href="{{ static 'myStyle.css' }}" />
            ```
            Then simply specifying the class name in the element tag:
            ```css
            <element class="myClass"></element>
            ```
1. 

## The Backend (Python)

1. __How to make sure the HTML syntax recognised in the string passed to the frontend?__ 
    - Install the package [MarkupSafe](https://pypi.org/project/MarkupSafe/).
    - Modify the string as follows:
        ```python
        from markupsafe import Markup

        label = Markup("My string")
        ```
1. 