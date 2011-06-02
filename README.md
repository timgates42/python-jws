python-jws
=====
A Python implementation of [JSON Web Signatures draft 02](http://self-issued.info/docs/draft-jones-json-web-signature.html)

Installing
----------
    $ git://github.com/brianlovesdata/python-jws.git
    $ python setup.py install

Usage
-----
Let's check out some examples.    
    
    >>> import jws
    >>> header  = { 'alg': 'HS256' }
    >>> payload = { 'claim': 'JSON is the raddest.', 'iss': 'brianb' }
    >>> signature = jws.sign(header, payload, 'secret')
    >>> jws.verify(header, payload, signature, 'secret')
    True
    >>> jws.verify(header, payload, signature, 'badbadbad')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "jws/__init__.py", line 44, in verify
        return verifier(_signing_input(head, payload), signature, key)
      File "jws/algos.py", line 31, in verify
        raise SignatureError("Could not validate signature")
    jws.exceptions.SignatureError: Could not validate signature    

Now with a real key!
    >>> import ecdsa
    >>> sk256 = ecdsa.SigningKey.generate(curve=ecdsa.NIST256p)
    >>> vk = sk256.get_verifying_key()
    >>> header = { 'alg': 'ES256' }
    >>> sig = jws.sign(header, payload, sk256)
    >>> jws.verify(header, payload, sig, vk)
    True

Advanced Usage
--------------
::todo::

Algorithms
----------

The JWS spec supports several algorithms for cryptographic signing. This library currently supports:

* HS256 – HMAC using SHA-256 hash algorithm
* HS384 – HMAC using SHA-384 hash algorithm
* HS512 – HMAC using SHA-512 hash algorithm
* RS256 – RSA using SHA-256 hash algorithm
* ES256 – ECDSA using P-256 curve and SHA-256 hash algorithm
* ES384 – ECDSA using P-384 curve and SHA-384 hash algorithm
* ES512 – ECDSA using P-521 curve and SHA-512 hash algorithm

Tests
-----

use nosetests

License
-------

MIT
