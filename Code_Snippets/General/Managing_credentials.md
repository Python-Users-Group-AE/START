# Managing Credentials

Storing passwords and API keys in plain text documents is not recommended.
For windows, an acceptable alternate is to use the `keyring` library

Library can be installed by running `pip install keyring`

## Storing Credentials

To save a password to keyring (Need to be done only once):

```python
import keyring 
keyring.set_password("<Application Name>","<User Name>","<Password>") 
```

## Retrieving Credentials

To retrieve the password anytime from any script, just run

```python
Import keyring 
keyring.get_password("<Application Name>","<User Name>") 
```
