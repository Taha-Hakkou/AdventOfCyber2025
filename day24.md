# Day 24 (Exploitation with cURL)

In the absence of a browser, you can still speak HTTP directly from the command line. The simplest way is with cURL.

curl is a command-line tool for crafting HTTP requests and viewing raw responses. It's ideal when you need precision or when GUI tools aren't available.

Because this is a terminal, instead of rendering the webpage, what you'll see is the text representation of the page in HTML.


    -X POST tells cURL to use the POST method.
    -d defines the data we're sending in the body of the request.
    The data will be sent in URL-encoded format, which is the same as what HTML forms use.
    
To view exactly what the server returns (including headers and potential redirects), add the -i flag.

If the site responds with a Set-Cookie header, that's a good sign, it means you've successfully logged in or at least triggered a session.