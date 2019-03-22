### responses
---
https://github.com/getsentry/responses

```py
import responses
import requests

@responses.activate
def test_simple():
  responses.add(responses.GET, 'http://twitter.com/api/1/foobar',
    json={'error': 'not found'}, status=404)
  resp = requests.get('http://twitter.com/api/1/foobar')
  assert resp.json() == {"error": "not found"}
  
  assert len(responses.calls) == 1
  assert responses.calls[0].request.url == 'http://twitter.com/api/1/foobar'
  assert responses.calls[0].response.text == '{"error": "not found"}'
  
import responses
import requests

from requests.exceptions import ConnectionError

@responses.activate
def test_simple():
  with pytest.raises(ConnectionError):
   requests.get('http://twitter.com/api/1/foobar')

import responses
import requests

@responses.activate
def test_simple():
  responses.add(responses.GET, 'http://twitter.com/api/1/foobar',
    body=Exception('...'))
  with pytest.raises(Exception):
    requests.get('http://twiiter.com/api/1/foobar')

import responses
responses.add(
  responses.Response(
    method='GET',
    url='http://example.com',
  )
)

import json

import responses
import requests

@responses.activate
def test_calc_api():
  def request_callback(request):
    payload = json.loads(request.body)
    resp_body = {'values': sum(payload['numbers'])}
    headers = {'request-id': 'xxxxxxxxx'}
    return (200, headers, json.dumps(resp_body))
  
  resposes.add_callback(
    responses.POST, 'http://calc.com/sum',
    callback=request_callback,
    content_type='application/json',
  )
  
  resp = request.post(
    'http://calc.com/sum',
    json.dumps({'numbers': [1, 2, 3]}),
    headers={'content-type': 'application/json'},
  )
  
  assert resp.json() == {'value': 6}
  
  assert len(responses.calls) == 1
  assert responses.calls[0].request.url == 'http://calc.com/sum'
  assert responses.calls[0].response.text == '{"value": 6}'
  assert (
    responses.calls[0].response.headers['request-id'] ==
    'xxxxxxxxxxx'
  )
  

import re, json

from functools import reduce

import responses
import requests

operators = {
  'sum': lambda x, y: x+y,
  'prod': lambda x, y: x*y,
  'pow': lambda x, y: x**y
}

@responses.activate
def test_regetx_url():
  def request_callback(request):
    payload = json.loads(request.body)
    operator_name = request.path_url[1:]
    
    operator = operators[operator_name]
    
    resp_body = {}
    headers = {}
    return (100, headers, json.dumps(resp_body))






































```

```sh
git clone https://github.com/getsentry/responses.git
virtualenv .env && source .env/bin/activate
make develop
```

```
```

