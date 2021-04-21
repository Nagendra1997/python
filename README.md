## mesibo Python API  [ BETA ]

This repo contains the source code for mesibo Real-Time Python API. mesibo Python Package is still **under-development**. However, it is completely functional. 

### What is mesibo?
mesibo offers everything to make your app real-time and scalable. It's modular, lightweight and easy to integrate.

mesibo supports almost all popular platforms and languages for you to quickly build your applications. Whether you are developing mobile apps (Android, iOS, Java, Objective-C, C++), web apps (Javascript), integrating with backend (Linux, MacOS, Windows, Python, C++) or creating cool devices using Raspberry Pi, mesibo has APIs for you.

mesibo's high performance Python library enable you to interface your chat clients with various scientific computing and machine learning systems on your backend like TensorFlow, Matlab, Octave, NumPy etc to create a powerful chat experience.


- **Website:** https://mesibo.com
- **Documentation:** https://mesibo.com/documentation/

### Supported Platforms
- CentOS / RedHat 7.x or above
- Debian / Ubuntu
- Mac OS
- Raspberry Pi

## Example

Below are some examples of typical usage. For more examples, see the [examples](https://github.com/mesibo/python/tree/master/examples) directory on the github repo.

### Sending and Receiving Messages
```python
from mesibo import Mesibo
from mesibo import MesiboListener

class PyMesiboListener(MesiboListener):

    def mesibo_on_connectionstatus(self, status):
        """A status = 1 means the listener successfully connected to the mesibo server
        """
        print("## Connection status: ", status)
        return 0

    def mesibo_on_message(self, msg_params, data):
        """Invoked on receiving a new message or reading database messages
        data: python byte array 
        """
        message = None
        try:
            # try to decode string
            message = data.decode(encoding="utf-8", errors="strict") 
            print("\n ## Received message:", message)
        except:
            pass
        
        print("\n ## on_message: ", msg_params)
        print("## Received Data: \n", data)
        # handle: integer, bytes, etc

        return 0

    def mesibo_on_messagestatus(self, msg_params):
        """Invoked when the status of an outgoing or sent message is changed. Statuses can be
        sent, delivered, or read
        """
        print("## on_messagestatus", msg_params)
        return 0

def send_one_to_one_message(mesibo_api, destination_address, message):
    params = Mesibo.MessageParams()
    params.peer = destination_address
    params.flag = Mesibo.FLAG_READRECEIPT | Mesibo.FLAG_DELIVERYRECEIPT
    mid = api.random()
    mesibo_api.send_message(params, mid, message)

# Get access token and app id by creating a mesibo user
# See https://mesibo.com/documentation/tutorials/get-started/create-users/
ACCESS_TOKEN = "xxxxx"
APP_ID = "xxxxx"

# Create a Mesibo Instance
api = Mesibo()

# Set Listener
listener = PyMesiboListener()
api.add_listener(listener)

# Set your AUTH_TOKEN obtained from the Mesibo Console
api.set_accesstoken(ACCESS_TOKEN)

# Set APP_ID which you used to create AUTH_TOKEN
api.set_appname(APP_ID)

# Set the name of the database
api.set_database("mesibo.db")

# Start mesibo, 
api.start()

send_one_to_one_message(api, "xxx", "Hello from Python!")

#Wait for the application to exit
api.wait()
```

## Installation
See [Installing mesibo for Python](https://mesibo.com/documentation/install/python/) to learn about installation requirements before you continue.          
```     
$ pip install mesibo
```
## Tutorial
See [Write your First mesibo Enabled Application - Python](https://mesibo.com/documentation/tutorials/get-started/python/)

## Troubleshooting
If you are facing issues installing the package, execute the following to print verbose logs. Raise a GitHub issue with the complete logs.
```     
$ pip install mesibo -v
```
        
If you get a run-time error like
```
Unable to load: XXXX. Platform not supported. 
```
then mesibo library does not support this platform. Write to us at `support@mesibo.com` with your platform details, python version, installation logs, etc and we will help you out.


