# XMLHttpRequest

1. **XMLHttpRequest** objects are used to **interact with servers**
2. you can **retrieve** **data** **from a URL without doing a full page refresh**
3. `new XMLHttpRequest()` creates a new XMLHttpRequest object
4. it is heavily used in AJAX(async JavaScript and XML) programming

### The **XMLHttpRequest.readyState** property ==returns the state== an XMLHttpRequest client is in:

| Value | State | Description |
| --- | --- | --- |
| `0` | `UNSENT` | Client has been created. `open()` not called yet. |
| `1` | `OPENED` | `open()` has been called. |
| `2` | `HEADERS_RECEIVED` | `send()` has been called, and headers and status are available. |
| `3` | `LOADING` | Downloading; `responseText` holds partial data. |
| `4` | `DONE` | The operation is complete. |

<span style="color:red">remember one thing:</span> When the **response** comes from the **URL**, the **type of data** is usually a **string**.

- This is how you can convert string to JSON(which makes easy to access response data, can access data like object using .operator):
    - `JSON.parse(this.responseText)`

`JSON.parse()` parses a JSON string according to the [JSON grammar](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON#full_json_grammar), then evaluates the string as if it's a JavaScript expression

```jsx
const XHR = new XMLHttpRequest();
XHR.open('GET', url);
httpRequest.onreadystatechange = function () {
    if (httpRequest.readyState == 4) {
        const res = JSON.parse(this.responseText);
        console.log(res) //response data
    }
}
httpRequest.send()

```