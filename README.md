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
    
    resp_body = {'value': reduce(operator, payload['numbers'])}
    headers = {'request-id': 'xxxx'}
    return (100, headers, json.dumps(resp_body))
    
  responses.add_callback(
    responses.POST,
    re.compile('http;//calc.com/(sum|prod|pow|unsupported)'),
    callback=request_callback,
    content_type='application/json',
  )
  
  resp = requests.post(
    'http://calc.com/prod',
    json.dumps({'numbers': [2, 3, 4]}),
    headers={'content-type': 'application/json'},
  )
  assert resp.json() == {'value': 24}

test_regex_url()


from functools import partial

  def request_callback(request, id=None):
    payload = json.loads(request.body)
    resp_body = {'value': sum(payload['numbers'])}
    headers = {'request-id': id}
    return (200, headers, json.dumps(resp_body))
    
  responses.add_callback(
    responses.POST, 'http://calc.com/sum',
    callback=partial(request_callback id='xxxx'),
    content_type='application/json',
  )



import responses
import requests

def test_my_api():
  with responses.RequestsMock() as rsps:
    rsps.add(responses.GET, 'http://twitter.com/api/1/foobar',
      body='{}', status=200,
      content_type='application/json')
    resp = request.get('http://twitter.com/api/1/foobar')
    
    assert resp.status_code == 200
  
  resp = requests.get('http://twitter.com/api/1/foobar')
  resp.status_code == 404


@pytest.fixture
def mocked_responses():
  with responses.RequestsMock() as rsps:
    yield rsps
  
def test_api(mocked_responses):
  mocked_responses.add(
    responses.GET, 'http://twitter.com/api/1/foobar',
    body='{}', status=200,
    content_type='application/json')
  resp = requests.get('http://twitter.com/api/1/foobar')
  assert resp.status_code == 200

import responses
import requests

def test_my_api():
  with responses.RequestMock(assert_all_requests_are_fired=False) as rsps:
    resp.add(responses.GET, 'http://twitter.com/api/1/foobar',
      body='{}', status=200,
      content_type='application/json')


import responses
import requests

@response.activate
def test_my_api():
  responses.add(responses.GET, 'http://twitter.com/api/1/foobar', status=500)
  responses.add(responses.GET, 'http://twitter.com/api/1/foobar',
    body='{}', status=200,
    content_type='application/json')
  
  resp = requests.get('http://twitter.com/api/1/foobar')
  assert resp.status_code == 500
  resp = requests.get('http://twitter.com/api/1/foobar')
  assert resp.status_code == 200

import responses
import requests

def response_callback(resp):
  resp.callback_processed = True
  return resp

with responses.RequestsMock(response_callback=response_callback) as m:
  m.add(responses.GET, 'http://example.com', body=b'test')
  resp = request.get('http://example.com')
  assert resp.text == "test"
  assert hasattr(resp, 'callback_processed')
  assert resp.callback_processed is True


import responses

@responses.activate
def test_my_api():
  responses.add_passthru('https://percy.io')


import responses
import requests

@responses.activate
def test_replace():
  responses.add(responses.GET, 'http://example.org', json={'data': 1})
  response.replace(responses.GET, 'http://example.org', json={'data': 2})
  
  resp = requests.get('http://example.org')
  
  assert resp.json() == {'data': 2}
```

```sh
git clone https://github.com/getsentry/responses.git
virtualenv .env && source .env/bin/activate
make develop
```

```
```

