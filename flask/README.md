## Authentication Middleware Flask
**Step - 1**
basic import basic packages
```python
#! /usr/bin/env python3
from flask import Flask, request, jsonify
from functools import wraps
from datetime import datetime, timedelta
import jwt
```
**Step - 2**
create decorator function in flask functools. check header you can use barear token i like to use `x-access-token`.
```python
def login_required(next):
	'''
		1. first find token in headers 
		2. and then get token in header key is 'x-access-token'
		3. check and verify token
	'''
	@wraps(next)
	def decorator(*args, **kwargs):
		if 'x-access-token' in request.headers:
			accessToken = request.headers.get('x-access-token')

			if accessToken is None:
				return jsonify({"message": "Token is missing"}), 401

			try:
				data = jwt.decode(accessToken, app.config['SECRET_KEY'], algorithms=["HS256"])
				return next(*args, **kwargs)

			except jwt.exceptions.ExpiredSignatureError:
				return jsonify({"message": "Token is expired."}), 403

		return jsonify({"message": "Token is required"})

	return decorator
```
**Step - 3**
use the decorator on your route decorator after
```python
app = Flask(__name__)
app.config['SECRET_KEY'] = "aba30b3b-84d3-4882-8ffa-97901d0a1f71"

@app.route("/login", methods=['POST'])
def login():
	return jwt.encode({"user_id": 1, "exp": datetime.utcnow() + timedelta(minutes=2) }, app.config['SECRET_KEY']) 

@app.route("/secret", methods=['GET'])
@login_required
def secret():
	print(request.args)
	return {"secret": True}

if __name__=='__main__':
	app.run(port=8080, debug=True)
```
