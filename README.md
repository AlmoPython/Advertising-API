# Advertising-API
### 1. Generate Token: 
```python
import requests

#refresh token
headers = {
    'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8',
}

data = {
  'grant_type': 'refresh_token',
  'client_id': 'amzn1.application-oa2-client.e4811adc4e26435eb1523d61b8536ac1',
  'refresh_token': 'Atzr|IwEBIJz4IldgedqRC3Lkgr20fDE5dipan61Vq00Wb8A9BQobFPDhJpiro-FJbZPPd_dsQPKn8zVkLL9BCU_094JqVJFScQgtA6r7_NxCRP8Kl6mxGsp5vBpDioiOqDaPg2XvYzWCQf_leX6a2miJTcoCRrDpl0N9hT1daOopJpbSa7GMqRB8Mmdjc7fTQ6EQXr8F2YkGSaWHdgWaRyLaDEE7fBl4-RT8F-YuGdg58J_bKSfWUF7znki3r75WhBHWR-EXd4swMUPrfE-E4AQ4K9-cw6h4GZEq3J9Y4fBZnGlDJEa72oRZT-yn1k-alZ7oEWcUkzgL79WiMBYDkgEmnZgBZ0u4YeW9wNaYKpQgv5GDvX097fE_NPrC02vroTbtCXYwwoAg7k-G6eY0dGukgccEJ8rqD2lUUDJWzy20-xHT2sitTeTF0WYOgS41rWnn3bkS8mugIy6h5P-casd00oZ3xGSdHQExIB3F0TDs_o1wYLlRzyAi9ZxOqdH2WK5LwvOSD3JDbIWqRDpubKiF5IcGgb1cZSm7B_wy1UdRYHVpr9CVew',
  'client_secret': 'bb360c83db2943955ff9270cb887b473264383033ed6102f160ffd4fa1e74882'
}

response = requests.post('https://api.amazon.com/auth/o2/token', headers=headers, data=data)
result = response.json()
token = result['access_token']

```
* The refresh token is always the same, the access token is different every time, works only for around 180minutes. 

### 2. Get profile_id (brand)

```python

headers = {
    'Amazon-Advertising-API-ClientId ': 'amzn1.application-oa2-client.e4811adc4e26435eb1523d61b8536ac1',
    'Authorization' : token
}


response = requests.get('https://advertising-api.amazon.com/v2/profiles', headers=headers)
response.text
result = response.json()
# token = result['access_token']
profile_id = '2066214756946634'

```
* Each profile_id represent one brand
### 3. Generate Report 

```python

import json

#Report
headers = {
    'Amazon-Advertising-API-ClientId ': 'amzn1.application-oa2-client.e4811adc4e26435eb1523d61b8536ac1',
    'Content-Type': 'application/json',
    'Amazon-Advertising-API-Scope': profile_id,
    'Authorization' : token
}


data = {
   'reportDate': '20220131',
   'metrics': 'asin,impressions,clicks,cost'
}

response = requests.post('https://advertising-api.amazon.com/v2/sp/productAds/report', headers=headers, json = data)
response.text

result = response.json()
reportid = result['reportId']




headers = {
    'Amazon-Advertising-API-ClientId ': 'amzn1.application-oa2-client.e4811adc4e26435eb1523d61b8536ac1',
    'Content-Type': 'application/json',
    'Amazon-Advertising-API-Scope':profile_id,
    'Authorization' : token
}

response = requests.get('https://advertising-api.amazon.com/v2/reports/' + reportid, headers=headers)
response.text



headers = {
    'Amazon-Advertising-API-ClientId ': 'amzn1.application-oa2-client.e4811adc4e26435eb1523d61b8536ac1',
    'Content-Type': 'application/json',
    'Amazon-Advertising-API-Scope': profile_id,
    'Authorization' : token
}

response = requests.get('https://advertising-api.amazon.com/v2/reports/' + reportid + '/download', headers=headers,allow_redirects=True)
response.text

```
* This is just an test for generating a report. Hopefully there is enough information for you to get access to the API portal. And I'm using Python, hopefully you don't mind i pasting these code here, since i thought this will be easier to understand.
* I will also put the official doc given by Amazon here. 
[https://advertising.amazon.com/API/docs/en-us/setting-up/step-4-authorization-grant](https://advertising.amazon.com/API/docs/en-us/setting-up/step-4-authorization-grant)
