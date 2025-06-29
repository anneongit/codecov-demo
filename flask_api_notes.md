# Key Concepts for Flask API

Here are the key concepts from our discussion about the `app.py` and `calculator.py` interaction.

### 1. The Flask `request` Object

-   **What it is:** A special global object that Flask provides. It contains all the information about the *single* HTTP request that is currently being processed.
-   **Lifecycle:** A brand new `request` object is created for **every single request** the server receives and is destroyed once the response is sent. This keeps user data separate and safe.
-   **`request.json`:** If a client sends a request with a `Content-Type` header of `application/json`, Flask automatically parses the body of the request into a Python dictionary, which you can access via `request.json`.

### 2. The API "Contract"

-   An API defines a set of rules that clients must follow.
-   In our example, the contract for `/api/add` requires the client to send a `POST` request with a JSON body.
-   That JSON body **must** contain two keys, `x` and `y`, for the server to understand what to do. The server code `request.json.get('x')` is specifically looking for that key.

### 3. Dynamic Method Calling with `getattr()`

-   **What it is:** A built-in Python function `getattr(object, 'attribute_name_as_string')`.
-   **Why it's used:** It allows you to access an object's attribute or method using a variable containing its name.
-   **Example:** `getattr(Calculator, 'add')` is the same as `Calculator.add`. This is powerful because it lets you write flexible functions that can call different methods without needing a big `if/elif/else` block.

### 4. Argument Unpacking with the Splat Operator (`*`)

-   **What it is:** The `*` operator, when placed before a list or tuple in a function call.
-   **Why it's used:** It "unpacks" the items from the list and passes them as individual arguments to the function.
-   **Example:** If `factors = [10.0, 5.0]`, then `my_func(*factors)` is the same as calling `my_func(10.0, 5.0)`. This is a clean way to pass a variable number of arguments.

### 5. Separation of Concerns

-   This is a core software design principle.
-   **`calculator.py`:** Handles the "business logic." Its only concern is performing calculations correctly. It knows nothing about the web.
-   **`app.py`:** Handles the "presentation logic" or "API layer." Its only concern is managing web requests and responses. It doesn't know *how* to add, it just knows to ask the `Calculator` to do it.
-   **Benefit:** This makes the code easier to manage, test, and debug. You can test the calculator without needing a web server, and you can change the web framework without having to rewrite your calculation logic.
