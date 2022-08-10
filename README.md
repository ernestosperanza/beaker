Beaker
------
<img align="left" src="beaker.png" margin="10px" >

Beaker is a smart contract development framework for [PyTeal](https://github.com/algorand/pyteal) inspired by Flask


With Beaker, we build a class that represents our entire application including state and routing.

*Mostly Untested - Expect Breaking Changes* 


## Hello, Beaker


```py
from pyteal import *
from beaker import *

# Create a class, subclassing Application from beaker
class HelloBeaker(Application):
    # Add an external method with ABI method signature `hello(string)string`
    @external
    def hello(self, name: abi.String, *, output: abi.String):
        # Set output to the result of `Hello, `+name
        return output.set(Concat(Bytes("Hello, "), name.get()))

# Create an Application client
app_client = client.ApplicationClient(
    # Get sandbox algod client
    client=sandbox.get_algod_client(),
    # Instantiate app, pass it to client
    app=HelloBeaker(),
    # Get acct from sandbox and pass the signer
    signer=sandbox.get_accounts().pop().signer,
)

# Deploy the app on-chain
app_id, app_addr, txid = app_client.create()
print(f"Deployed app with id {app_id} and address {app_addr} in txid {txid}")

# Call the `hello` method
result = app_client.call(HelloBeaker.hello, name="Beaker")
print(result.return_value) # "Hello, Beaker"
```

## Install

You can install from pip:

`pip install beaker-pyteal`

Or from github directly (no promises on stability): 

`pip install git+https://github.com/algorand-devrel/beaker`

## Use

[Examples](/examples/)

[Docs](https://beaker.algo.xyz)

[TODO](TODO.md)

*Please feel free to file issues/prs*