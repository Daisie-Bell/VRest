# VRest Documentation

## Introduction

VRest is a Python library for making HTTP and WebSocket requests to RESTful and WebSocket-based APIs. It provides a flexible and easy-to-use interface for interacting with various APIs, allowing you to send requests and receive responses.

## Problem Statement

APIs are a common way to interact with web services and applications. However, they can be difficult to use, especially when they require complex authentication or use WebSocket connections. VRest simplifies API interaction by providing a clean and flexible interface for making HTTP and WebSocket requests.

## Solution

VRest provides a `RestAPI` class that you can use to interact with various APIs. It allows you to easily make HTTP and WebSocket requests and handle responses and errors. You can use it to communicate with RESTful and WebSocket-based APIs, including those that require complex authentication.

## Installation

You can install VRest using poetry:

```bash
poetry add git+https://github.com/Vortex5Root/VRest.git
```

## Dependencies

VRest has the following dependencies:

- [python3.11.2](https://www.python.org/downloads/release/python-3112/)
- [requests](https://pypi.org/project/requests/)
- [websockets](https://pypi.org/project/websockets/)

## Skeleton

The API configuration includes a skeleton, which defines the available API functions. Each function has a name, suffix, and method. The name is used to call the function, the suffix is appended to the API endpoint, and the method is the HTTP method used to make the request. For example, the following skeleton defines a function named `function_name` that makes a GET request to the `endpoint_suffix` endpoint:

```python
"skeleton": {
    "function_name": {
        "suffix": "endpoint_suffix",
        "method": "GET"
    }
}
```

You can add as many functions as needed to the skeleton. For example, the following skeleton defines two functions:

```python
"skeleton": {
    "function_name_1": {
        "suffix": "endpoint_suffix_1",
        "method": "GET"
    },
    "function_name_2": {
        "suffix": "endpoint_suffix_2",
        "method": "POST"
    }
}
```
## RestAPI Structure

The `RestAPI` config has the following structure:

```python
{
    "end_point": "https://api.example.com/v1/",
    "headers":{
        "*Token_Var": "Token {}", # Use * as prefix to indicate the Token Var
    },
    "skeleton": {
        "function_name": {
            "suffix": "endpoint_suffix",
            "method": "GET"
        }
    }
}
```

## Examples

Here's a basic example using the Deepgram API:

```python
from vrest import RestAPI

# Define the API configuration
deepgram_config = {
    "end_point": "https://api.deepgram.com/v1/",
    "header": {
        "accept": "application/json",
        "content-type": "application/json",
        "*Authorization": "Token {}" # Use * as prefix to indicate the Token Var
    },
    "skeleton": {
        "pre_recode": {
            "suffix": "listen",
            "method": "POST"
        },
        "stream": {
            "suffix": "listen",
            "method": "ws"
        }
    }
}

# Create a RestAPI instance
deepgram_api = RestAPI(deepgram_config, api_key="YOUR_API_KEY")

# Example API request
response = deepgram_api.pre_recode(params={"filler_words": "false", "summarize": "v2"}, json={"url": "https://static.deepgram.com/examples/interview_speech-analytics.wav"})

# Print the response status code and data
print(f"Status Code: {response.status_code}")
if response.status_code == 200:
    print(response.json()["results"]["channels"][0]["alternatives"][0]["transcript"])
else:
    print(response.text)
```

### Making API Requests

Once you have created a `RestAPI` instance, you can make API requests using the defined functions. For example, to make a GET request to the `function_name` endpoint:

```python
response = api.function_name(params={"param_name": "param_value"})
```

Replace `function_name` with the name of the function you want to call, and provide any necessary parameters as a dictionary in the `params` argument.

### WebSocket Support

VRest also supports WebSocket connections. If a function is configured to use WebSocket (method="ws"), you can start a WebSocket connection and send and receive data. Here's an example of starting a WebSocket connection:

```python
async def start_websocket():
    # Start the WebSocket connection
    await api.function_name.ws_start(params={"param_name": "param_value"})

# Create an asyncio event loop and run the WebSocket function
import asyncio
loop = asyncio.get_event_loop()
loop.run_until_complete(start_websocket())
```

## Error Handling

VRest includes custom exceptions for handling common errors:

- `InvalidEndPoint`: Raised when the API endpoint is invalid.
- `MissingSkeleton`: Raised when the API configuration lacks a skeleton (function definitions).
- `TokenRequired`: Raised when an API request requires an authentication token.

You can handle these exceptions in your code to gracefully manage errors.

## Conclusion

VRest is a Python library for making HTTP and WebSocket requests to RESTful and WebSocket-based APIs. It provides a flexible and easy-to-use interface for interacting with various APIs, allowing you to send requests and receive responses.
