---
title: "POSTing forms"
date: 2022-10-13T22:07:35+02:00
draft: false
---

I was doing some TryHackMe recently. And, while I like learning high-level tools such as BurpSuite, I *really* love hacking stuff with just the basic tool. The basic tool being `curl` in that situation.

But there was one stuff that I would *always* get wrong, no matter what. It's writing the `curl` command for forms. How do you encode form data ? What are these encodings ? And… why oh why ?

So this article came out of the necessity for me to understand it all. I hope it will be useful for you too.

# HTML

That's where it starts, eh ?

Well there is the `<form>` HTML element. The [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Form) probably does a better job than me, but I'll sum it up.

Here is a simple example:

```html
<form action="submit.php" >
    <label for="name">Enter your name:</label>
    <input type="text" name="name" id="name" required>

    <input type="submit" value="Subscribe !">
</form>
```

The most important attributes of the `form` element are:

- `action`: the URL that process the form submission
- `enctype`: the MIME type of the form submission if the method is POST. You have mostly two choices for this:
    - `application/x-www-form-urlencoded` which is the default if the `enctype` attributes is not there
    - `multipart/form-data` that seems to be useful to send file.
- `method`: the HTTP method to send the form with. We will focus on GET and POST here.

For the `input`, it can have several types (again, check the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)): `button`, `checkbox`, `file`, `hidden`, `password`, `submit`, `text`, …

The attributes can be `name` or `required` to force the browser to validate them.

# GET

I have yet to see a form using the GET method somewhere in production (but I'm not paying attention !). My best guess is if the user wants to specify a filter in requesting a resource.

In any case, the form data are appended into the `action` url, starting with `?` and separated with. `&`.

So for example, the `curl` command would become: 

```bash
# actually "-X GET" is not required here
curl -X GET http://host.com/submit.php?name=Alphonse&surname=Fonfon
```

And here is the HTTP request that will be sent.

```http
GET /submit.php?name=Alphonse&surname=Fonfon HTTP/1.1
Host: host.com
User-Agent: curl/7.85.0
Accept: */*
```

Pretty simple, eh ?

# POST

Now. Say you want to POST data to a website. First of all, of COURSE you can check the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST).

Then, there is the good old JSON-way. Or XML, or any format like that really. Just stick `Content-Type: application/json` (for JSON) in the request header and BAM. You've done it.

```bash
curl -X POST -H "Content-Type: application/json" \
    -d '{"name": "Alphonse", "surname": "Fonfon"}' \
    http://host.com/submit.php
```

The requests may look like this.

```http
POST /submit.php HTTP/1.1
Host: host.com
User-Agent: curl/7.85.0
Content-Type: application/json
Content-Length: 38
Accept: application/json, */*

{"name":"Alphonse","surname":"FonFon"}
```

But now… as we learned previously, there are two other MIME types that can come from form submissions: `application/x-www-form-urlencoded` and `multipart/form-data`. So, here we go.

# `application/x-www-form-urlencoded`

This is probably the most common case, and it also happens to be the one `curl` picks by default. If no `enctype` is specified, then it's the expected format on the server's side.

Basically, you just put all you data in a `input=value` format and split them with `&`. Don't forget to set the header `Content-Type: application/x-www-form-urlencoded`.

The request might look something like:

```http
POST /submit.php HTTP/1.1
Host: host.com
User-Agent: curl/7.85.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 28
Accept: application/json, */*

name=Alphonse&surname=FonFon
```

And to invoke with `curl`:

```bash
curl -X POST \
    -d name=Alphonse -d surname=Fonfon \
    http://host.com/submit.php
```

# `multipart/form-data`

So. This one is largely describe in [RFC 2388](https://www.ietf.org/rfc/rfc2388.txt). You can go check it out.

As the name suggests, the message is sent in multiple parts. Parts are separated by a string boundary. These boundaries specify a type and a disposition. With this, you can easily encode binary data, as it can be just a part of the overall form submission.

The tricky part here is that the binary encoding you choose should never looks the same as the boundary of the MIME type. Otherwise, the server cannot distinguish between if this is part of the file or just a boundary to another part.

Also, the binary encoding is the reason why there exists two MIME type for form submission. You see, in `application/x-www-form-urlencoded`, if you want to submit a non-alphanumeric character, you have to encode it as `%HH` where `HH` is the hexadecimal value of the ASCII character (don't ask me about Unicode now). So, with this, one byte might be encoded as three bytes. This is why the `multipart/form-data` exists, to be able to pass binary data… such as files ! And this is why the MDN documentation stated that it's its main usage.

Here is an example of such a request.

```http
POST /submit.php HTTP/1.1
Host: host.com
User-Agent: curl/7.85.0
Accept: application/json, */*
Content-Type: multipart/form-data; boundary=---------------------------9051914041544843365972754266
Content-Length: 554

-----------------------------9051914041544843365972754266
Content-Disposition: form-data; name="text"

text default
-----------------------------9051914041544843365972754266
Content-Disposition: form-data; name="file1"; filename="a.txt"
Content-Type: text/plain

Content of a.txt.

-----------------------------9051914041544843365972754266
Content-Disposition: form-data; name="file2"; filename="a.html"
Content-Type: text/html

<!DOCTYPE html><title>Content of a.html.</title>

-----------------------------9051914041544843365972754266--
```

Also, you could for example send the file data encoded in base 64. (Useless note, but each boundary starts with *two dashes*, even if the boundary declared in the content type has dashes. This is why each boundary have two more dashes, even if it is hard to see. Also the last boundary always has two dashes at the end.)

Now, why don't we *always* send data with `multipart/form-data` ? Well the boundary also takes some room, and it takes room for each input in the form. So, for small form submission, you are probably better off with the first option.

Phew. Finally, and to conclude, this is how to send `multipart/form-data` with `curl`.

```bash
curl -X POST \
    -F name=Alphonse -F file=@file/on/my/system.txt \
    http://host.com/submit.php
```
