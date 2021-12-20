### Python Flask: eBay Marketplace Account Deletion/Closure Notifications
Read about it here: [https://developer.ebay.com/marketplace-account-deletion](https://developer.ebay.com/marketplace-account-deletion)

Set it up here: [https://developer.ebay.com/my/push/?env=production&index=0](https://developer.ebay.com/my/push/?env=production&index=0)


### How to set it up:
* pip install flask
* Set the verification_token and endpoint_url variables to the ones you want to use

* Create a random verification token, it needs to be 32-80 characters long

* There will be errors if you just use '/' as the route as it will redirect eBays request
* eBay will send a request to https://dev.example.com?challenge_code=123
* The request will get redirected by Flask to https://dev.example.com/?challenge_code=123 which eBay will not accept

* The Content-Type header will be added automatically by Flask as 'application/json'


### The code:

```
from flask import Flask, request
import hashlib

# Create a random verification token, it needs to be 32-80 characters long 
verification_token = 'a94cbd68e463cb9780e2008b1f61986110a5fd0ff8b99c9cba15f1f802ad65f9'
endpoint_url = 'https://dev.example.com'

app = Flask(__name__)

# There will be errors if you just use '/' as the route as it will redirect eBays request
# eBay will send a request to https://dev.example.com?challenge_code=123
# The request will get redirected by Flask to https://dev.example.com/?challenge_code=123
# eBay will not accept that redirect and will consider it a fail
endpoint = endpoint_url + '/test'

# The Content-Type header will be added automatically by Flask as 'application/json'
@app.route('/test')
def test():
    code = request.args.get('challenge_code')
    print('Requests argument:', code)

    code = code + token + endpoint
    code = code.encode('utf-8')
    code = hashlib.sha256(code)
    code = code.hexdigest()
    print('Hexdigest:', code)

    final = {"challengeResponse": code}
    return final

## To run locally first use this:
# app.run(port=29)
```
