---
title: Remote code execution, simplified
date: 2023-07-28
---


Hello again to any acmFolks at https://acmcsuf.com/blog and anyone else reading!  Today we’re gonna talk about another category of web-vulnerabilities called Remote code execution. The reason I'm writing about RCE is because after reading about it I found it fascinating how it can manifest itself in so many different, nuanced ways.


# What is RCE?
So first let’s go over what remote code execution is in the first place.

RCE is very broad and seems like it encompasses lots of web vulnerabilities, but like other web-vulnerabilities that exist it usually starts when user input is not sanitized and code/commands gets injected into an application, network or machine remotely leading to unintended code execution. After attackers can run their own code, this may allow them to gain full access or steal data.

# How does RCE occur
RCE can manifest itself through **De-serialization**, **Buffer overflows** and **Type Confusion** and many other methods. Through these, attackers can inject code or commands into a system, and from there escalate access.


### Deserialization
When data is sent to a system it gets serialized (converted to binary) and then deserialized(back to object code), if formatted properly you can maliciously create objects that execute code when deserialized. Take this hypothetical flask app.


```
from flask import Flask, request, jsonify
import json

app = Flask(__name__)

@app.route('/process_data', methods=['POST'])
def process_data():
    # Deserialize
    user_data = json.loads(request.data)

    save_user_data(user_data)
    return jsonify({"message": 'Data processed successfully'})

if __name__ == '__main__':
    app.run()
```

There’s potential vulnerabilities on the use of json.loads() since no validation happens on whatever json payload is sent into the `save_user_data()` function. Assuming the `save_user_data() ` function did not properly validate then an attacker could pass in some data like this as data and get it deserialized by the server so that it would call `os.system`.
 


```
{
  "__class__": "os.system",
  "__args__": ["echo Hacked > /tmp/hacked.txt"]
}
```
To prevent this you could use json web tokens(JWTs), or use libraries that specialize in serialization.
(json.loads is actually pretty safe-ish and this is for examples sake, other libraries like python pickles have had deserialization exploitation see 
https://davidhamann.de/2020/04/05/exploiting-python-pickle/ ) 

### Buffer-overflows
Apps use buffer(temp storage) memory for data storage, this includes user data. In a buffer attack an attacker would write to memory due to lack of checks on allocated memory bounds. When the buffer does overflow it overwrites memory and this can be used to destroy data, make networks unstable, or replace memory with code.

Let’s see a simple example of this below on this simple TCP server.
```
import socket

HOST = '127.0.0.1'
PORT = 3000

def echo_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind((HOST, PORT))
    server_socket.listen(1)
    print(f"[*] Listening on {HOST}:{PORT}")

    client_socket, client_address = server_socket.accept()
    print(f"[*] Accepted connection from {client_address}")

    data = client_socket.recv(1024)
    print(f"[*] Received data: {data}")

    buffer = bytearray(4)  # A buffer that can hold 4 bytes
    buffer[:len(data)] = data

    client_socket.sendall(buffer)
    client_socket.close()

if __name__ == "__main__":
    echo_server()
```

Since the buffer has a defined size of 4 bytes an attacker could send data that is larger then that and cause unexpected behavior.

An attacker could use something like netcat to connect to the server and send something like
` echo -n -e "\x41\x42\x43\x44\x45\x46\x47\x48" | nc 127.0.0.1 3000 `  

After sending these 8 bytes into the input it will cause a buffer overflow. The bytes after the first 4 would overwrite memory. Now assuming if the input isn’t being sanitized in any other way an attacker could craft a payload that includes shellcode to pass code into that target system. 

(It’s important to note buffer overflows are more commonly found in low-level languages like C and C++ rather than Python and that python has more built in safety mechanisms but this code is for examples/understanding's sake)

### Type confusion
Type confusion occurs when an object gets passed in without checking its type. It’s worth noting this tends to happen in applications written in more type heavy languages like in C or C++ but it can happen on web/network apps too.Let’s take a look at this hypothetical flask app.


```
from flask import Flask, request

app = Flask(__name__)

user_data = []

@app.route('/submit', methods=['POST'])
def submit():
    name = request.form['name']
    age = request.form['age']

    # Create a dictionary to store the user data
    data = {'name': name, 'age': age}

    # Add the user data to the list
    user_data.append(data)

    return "Data submitted successfully!"

@app.route('/display', methods=['GET'])
def display():
    return str(user_data)

if __name__ == '__main__':
    app.run()
```
This app has two routes /submit and /display that display data. An attacker could send a post request to /submit with data like

```
POST /submit HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

name=__proto__&age={"isAdmin":true}
```

And set the name to __proto__ and the age to set the admin status as true.

`__proto__` is a js property that allows objects to access their prototypes, which basically means they can inherit properties/methods from other objects.

Anyway, the end result is that they bypassed a check on the isAdmin property due to improper object validation. This is a type confusion vulnerability since an attacker could inject a property into user data, but the application interprets it as an admin privilege. (this example also demonstrates a bit of prototype pollution as we are adding properties object prototypes) 

# Other RCE

Remote Code Execution (RCE) is a serious threat in web applications, and it can arise through various attack vectors. Common vulnerabilities like path traversal, SQL injection (SQLi), cross-site scripting (XSS), OS command injection, and code evaluation can all lead to RCE. However, the list of potential vulnerabilities doesn't end there. Other web vulnerabilities, including file upload flaws, XML injection (XXE), server-side request forgery (SSRF), and command injection on shells, can also be exploited to achieve RCE. Therefore, web developers and security practitioners must be vigilant in addressing all these potential attack vectors to protect against RCE and ensure the security of their applications.

# How to prevent RCE
The first thing would be sanitizing user input properly, so validating/filtering input data, and any api/web service data.
Another thing would be to use prepared statements for any sql to prevent SQLi. Escape sanitization should also be applied to any sites where code can be executed.
Have a zero trust approach to any applications. Another good standard is to not allow any services to run as root, as this is bad practice and should go without saying.

# Peace!
Those are some of the basic ways Remote code execution attacks can happen and hopefully help you better understand RCE. It’s pretty insane how many different ways attackers can get code to run on a targeted machine or system, and there’s so many countless different ways they can do it.  Thanks for reading!