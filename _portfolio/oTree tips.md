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
1. __Access HTML elements in JavaScript.__
    - Access by element properties.
        ```js
        getElementByID() // return element
        getElementsByName() // return list
        getElementsByTagName() // return list
        getElementsByClassName() // return list
        ```
    - Access by CSS selector.
        ```js
        querySelector() // return element
        querySelectorAll() // return list
        ```
1. __Useful reference.__
    - [HTML color codes](https://htmlcolorcodes.com/)
    - [CSS selector](https://www.w3schools.com/cssref/css_selectors.php)

## The Backend (Python)

1. __Make sure the HTML syntax is recognised in the string passed to the frontend.__ 
    - Install the package [MarkupSafe](https://pypi.org/project/MarkupSafe/).
    - Wrap the string with `Markup()`:
        ```python
        from markupsafe import Markup

        label = Markup("My string with <strong>HTML syntax.</strong>")
        ```
1. __Understand how live pages work.__
    - The communication between the frontend and the backend always follows the process below:
        - The frontend sends data to the backend (usually in an onclick event):
            ```js
            liveSend(data_send)
            ```
        - The backend receives the data and processes it. Then it response data to the frontend to update the page:
            ```python
            class myLivePage(Page):
                def live_method(player, data):
                    # codes to process the data
                    return {
                        player_id: data_response
                    }
            ```
        - The frontend receives the response data and processes it:
            ```js
            function liveRecv(data) {
                // codes to process the data
            }
            ```
    - The `liveSend()` and `liveRecv()` functions are oTree built-in features, you cannot change their names or use other functions to replace it.
    - You cannot start the communication from the backend (the most common case is that you want to initialise the page by sending configuration data to it). Alternatively, you can send a special message from the frontend on page load, then process this special indicator in the backend and response with initialisation data to the frontend:
        ```js
        liveSend({ "type": "init" })
        ```
        ```python
        class myLivePage(Page):
            def live_method(player, data):
                if (data["type"] == "init"):
                    return {
                        player_id: data_init
                    }
                else:
                    return {
                        player_id: data_response
                    }
        ```