# apipy
A Python 3 library for interacting with the Stack Exchange API.

## Installation
apipy is on the Python Package Index, so installing it is easy:

    pip install apipy

## Usage

    from apipy import APIRequester

You can initialise a requester with your app's client ID, request key, and access token (all optional). You can also add each of these tokens to the requester later, using `requester.set_request_token`. The full signature of the initialiser method is:

    def __init__(self, request_key=None, client_id=None, access_token=None)

#### 1. Create a requester

    requester = APIRequester('L*dhE6DhheuD53((', 6509)

#### 2. Send a request

    response = requester.make_request('/comments/{id}', {
        'id': [3511992, 3512254],
        'site': 'stackoverflow',
        'filter': '!6JEVJZb8BM9ns'
    })
    
#### 3. Deal with the response

    if not response.is_error():
        for item in response.get_items():
            print("Found comment, ID {0}, text: {1}".format(item['comment_id'], item['text']))
        
        quota_remaining = response.get_wrapper()['quota_remaining']
    else:
        print("API Error! Message: {0}".format(response.get_wrapper()['error_message'])

#### Some points
- If a `request_key` has been supplied, it will be sent with *every* request because it provides increased quota.
- Access tokens need to be included explicitly: use `'access_token': requester.get_request_token('access_token')` in the `data` dict.
- Requests use HTTPS by default.
- `GET` and `POST` requests are supported (these are the only types the API accepts) - GET is the default. To set the request type, use `'request_type': 'post'` in the `data` dict.
- You can use parameters in your API route: `{param}` will be replaced with the value of `data['param']` if it exists. You can set `data['param']` to a single value or a list of values; lists will be joined with `;` as the delimeter.
