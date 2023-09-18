# 'Working with CORS' Talk Content

## Table of Contents

-   [What is Origin?](#what-is-origin)
-   [Why is CORS Required?](#why-is-cors-required)
-   [What is CORS?](#what-is-cors)
-   [Do All Cross-Origin Requests Require CORS?](#do-all-cross-origin-requests-require-cors)
-   [Demo Preparation](#demo-preparation)
-   [Non-Credentialed Requests without CORS](#non-credentialed-requests-without-cors)
-   [Non-Credentialed Requests with CORS](#non-credentialed-requests-with-cors)
-   [Recap](#recap)
-   [Simple Requests](#simple-requests)
-   [Non-Credentialed Requests through a CORS Proxy](#non-credentialed-requests-through-a-cors-proxy)
    -   [What is a CORS Proxy?](#what-is-a-cors-proxy)
    -   [CORS Proxy Demo](#cors-proxy-demo)
-   [Recap](#recap-1)
-   [Credentialed CORS Requests](#credentialed-cors-requests)
-   [Cookies](#cookies)
-   [Recap](#recap-2)
-   [Images](#images)
-   [Fonts](#fonts)
    -   [Loading Fonts Using `@font-face` in CSS](#loading-fonts-using-font-face-in-css)
    -   [Loading Fonts Using the <link> Tag in HTML](#loading-fonts-using-the-link-tag-in-html)
        -   [The `crossorigin` Attribute](#the-crossorigin-attribute)
    -   [Loading Fonts Using `@import` in CSS](#loading-fonts-using-import-in-css)
-   [Recap](#recap-3)
-   [To Do](#to-do)
-   [Resources](#resources)

## What is Origin?

-   URI scheme + Domain/Host + Port = Origin
    -   Eg: http + localhost + 3000 = `http://localhost:3000`
    -   URI scheme: HTTP, HTTPS, etc.
    -   Domain/Host: harshkapadia.me, talks.harshkapadia.me, localhost, etc.
        -   Sub-domains are considered to be different hosts.
    -   Port: 80 (HTTP), 443 (HTTPS), 3000, 5000, 8000, 8080, 9000, etc.
-   'Same-Origin Request' and 'Cross-Origin Request'
-	[Don't confuse 'origin' and 'site'!](https://web.dev/same-site-same-origin)
	-	`same-origin` vs `cross-origin` vs `(schemeful) same-site` vs `schemeless same-site` vs `cross-site`

## Why is CORS Required?

-   Needed due to the Same-Origin Policy (SOP), which was implemented to prevent resource abuse (Intellectual Property) and more importantly to improve security to prevent [Cross Site Request Forgery (CSRF) attacks](https://stackoverflow.com/a/27294846/11958552) (Session Cookies are sent to a another domain because Cookies and implicitly sent with requests by the browser, causing leakage of the user-sensitive info present in the cookies to the malicious API).
-   CORS enables servers to authorize selected sites to access cross-origin content. So it lifts/relaxes the SOP restrictions for certain sites.
-   Purely browser based restriction. The request to the cross-origin target is made, but the response is dropped by the browser.
    -   As the restriction is browser based though, another server can still make unrestricted requests to the original server, unless some other security mechanism like an API Key mechanism restricts it.
    -   Why is it okay for servers and why do only browsers need CORS?
        -   Well because CSRF attacks occur on the client side.

## What is CORS?

-   Way to relax SOP.
-   Gives control to the server over who can request for resources.
-   Allows servers to safely allow some cross-site requests.
    -   Don't talk about allowing multiple Origins, as that is for later with the `Vary` response header.

## Do All Cross-Origin Requests Require CORS?

-   No.
-   CSS, HTML elements such as `<img />` and `<form>`, etc. don't require CORS, but [Fetch requests, imported font files, images in a canvas, etc. require CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#what_requests_use_cors).
    -   Reason: Cross origin requests could always made using forms and image tags before the SOP (It would've broken too many sites if those rules were changed by SOP - just how the model of the web was.), but it prevented cross-origin requests from XMLHTTPRequest (XHR)/Fetch, so CORS was introduced to relax those restrictions. Now in some of those cross-origin XHR/Fetch request cases, the conditions are the same as those in form and image tags, so as those tags don't require a preflight request, it is not required here as well. ([Source](https://security.stackexchange.com/a/221771))

## Demo Preparation

-   [Demo GitHub repo](https://github.com/HarshKapadia2/cors)
-   Show the two domains, the client on `http://localhost:3000` and the server on `http://localhost:5000`, and talk about their structure.
    -   It is important to mention that requests from the client to the server in this case will be Cross-Origin Requests.
-   Explain Credentialed Requests (Requests with `Authorization`/`Authentication` headers, Cookies like Session Cookies, etc.) and Non-Credentialed requests.

## Non-Credentialed Requests without CORS

-   Make the 'Request without CORS' request. Let it fail and then show the two requests in the Network tab in DevTools. Then show the error on the console, explain it and go to the preflight headers, show the general section, the request section and then point out the missing header as per the console error in the response section of the preflight request in the Network tab.

## Non-Credentialed Requests with CORS

-   Now make the 'Request with CORS' request. Once it succeeds, show the general section, the request section and then point out the `Access-Control-Allow-*` headers in the response section of the preflight request in the Network tab.
-   Go to the Fetch request after that and show the response `Access-Control-Allow-Origin` header. Then show the payload and response body.
    -   Why is the `Access-Control-Allow-Origin` header required in the Fetch response as well?
-   Go to the server and show the POST and OPTIONS routes for `/noCredentials` and explain then. Show the middleware response after that.

## Recap

-   Recap
    -   Preflight request
        -   Restricts the actual request from being made.
    -   Important request headers (`Origin`, `Access-Control-Request-Method` and `Access-Control-Request-Headers`)
    -   Important response headers for both preflight and actual response (`Access-Control-Allow-Origin`, `Access-Control-Allow-Methods` and `Access-Control-Allow-Headers`)
-   QnA

## Simple Requests

-   Move on to the Simple Request (Preflight Request not reqd.). Show the `Content-Type` header and `body` difference in `script.js`.
-   Make the Simple Request and explain [the conditions that don't cause a Preflight Request](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests).
    -   The reason for certain conditions not requiring a Preflight Request is explained above in the answer of '[Do All Cross-Origin Requests Require CORS?](#do-all-cross-origin-requests-require-cors)'
-   Note that although a Preflight Request is not made, the `Access-Control-Allow-Origin` response header still needs to be sent in response to the actual request.

## Non-Credentialed Requests through a CORS Proxy

### What is a CORS Proxy?

-   As explained in the '[Why is CORS Required?](#why-is-CORS-required)' section, CORS is a browser-based restriction.
-   So if a server makes a request to the target server for the client, thus acting as a proxy, the request would succeed irrespective of CORS (not considering any authentication measures that would have to be included).
-   Yes one has to deal with CORS with the proxy, but as it is in the Developer's control (The Developer spins up the proxy server.), they can make CORS requests to it by setting the headers they want. The proxy server then makes a successful request to the target server (as CORS is not required between server-to-server requests) and sends back the target server's response to the requesting client.
-   This is exactly what a CORS Proxy is, what is does and why it is required.
-   It is better to abide by CORS than use a CORS proxy in production.
-   If a CORS proxy is required in production, then one should spin up their own proxy (self-host) and not trust a public CORS proxy like [CORS Bridged](https://blog.grida.co/cors-anywhere-for-everyone-free-reliable-cors-proxy-service-73507192714e), especially if the requests are protected by API key, Cookies, etc.
-   Valid use cases of COR proxies in production and development scenarios?

### CORS Proxy Demo

-   Show the setup of the proxy server running on `http://localhost:8000` and explain that requests from the client at `http://localhost:3000` to the proxy server are Cross-Origin Requests. Also explain that although requests from the proxy server running at `http://localhost:8000` to the target server running at `http://localhost:5000` are Cross-Origin Requests, CORS does not apply here, as these are server-to-server requests, where the CORS standard does not apply.
-   Make the request and show the preflight and Fetch request responses.

## Recap

-   Simple Requests
-   CORS Proxies
-   QnA

## Credentialed CORS Requests

-   Credentials can be sent in the `Authentication`/`Authorization` headers or in Cookies (like Session Cookies).
-   Show that the 'CORS request with wildcard (`*`) CORS response header values' is the same request as before, but with the added `{ credentials: "include"}`. Make the request and let it fail. Show the error in the console and then the preflight response with the wildcard (`*`) CORS response headers.
-   Mention that all the three CORS response headers, namely `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods` and `Access-Control-Allow-Headers`, need to have specific values and not wildcards (`*`).
-   Make the 'CORS request with specific CORS response header values', let it succeed and then show the preflight CORS response header values. **Make sure to show the `Access-Control-Allow-Credentials` header.**
    -   Then show the actual Fetch request and show that the `Access-Control-Allow-Origin` header has a specific Origin value and that **the `Access-Control-Allow-Credentials` header is also present**.
-   Show the server setup for both requests.
-   Conclude by saying that all the restrictions that get added to CORS response header values and the new `Access-Control-Allow-Credentials` response header are due to the `{ credentials: "include" }` option in Fetch. It helps improve security.
    -   How does it help increase security?
        -   It adds more restrictions and the next reason (Cookies) is mentioned in the next section, which helps mitigate CSRF attacks.

## Cookies

-   What difference does the `{ credentials: "include" }` Fetch option make? (Demo first and then explain.)
-   Set the `corsDemo` Cookie.
-   Make the 'CORS Request without `{ credentials: "include" }`' and show that the `Cookie` request header is not present. Also show that the response returns an empty string for the `receivedCookies` value.
-   Then make the 'CORS Request with `{ credentials: "include" }`' and show that the `Cookie` request header is being sent by the browser. Show that the response contains the `corsDemo` Cookie in the `receivedCookies` value.
-   So the `{ credentials: "include" }` Fetch option decides whether the browser should include Cookies with the request or not.
    -   Why is there an option to dictate whether Cookies have to be sent with the request not not?
        -   Cookies might contain sensitive user data and Cookies are implicitly sent to the server with a request by the browser (This is how Cookies are supposed to work.), which can cause sensitive user data to be leaked to a malicious API. This is a Cross Site Request Forgery (CSRF) attack.
        -   So to prevent CSRF attacks from happening unknowingly, Fetch provides the `{ credentials: "include" }` option which puts the onus on the Developer to make a decision to opt into the 'implicit-sending-of-Cookies' behaviour of the browser by including the option. Also, the restrictions added by the option increase security and thus aid the Developer in writing even more secure software.
-   Note that the `Authorization` header was not affected by the presence or absence of the option.

## Recap

-   If Cookies are needed to be sent, the `{ credentials: "include" }` Fetch option has to be added, without which Cookies will not be sent.
-   If the `{ credentials: "include" }` Fetch option is added, it introduces extra restrictions which need to be handled.
-   The presence or absence of the `{ credentials: "include" }` Fetch option does not affect the headers (like `Authorization`) that can or cannot be sent. The `Access-Control-Allow-Headers` response header still controls that.
-   QnA

## Images

-   Show that images from Wikipedia and the server running at `http://localhost:5000` both render and that both have the `Access-Control-Allow-Origin` header.
-   Change the value of the `Access-Control-Allow-Origin` header to a random host and show that the image still renders. Serving the image without that header also still renders the image.
-   Images requested through the `<img />` tag are exempt from CORS.
    -   This implies that they not only don't need the Preflight Request, but the CORS response headers in the actual request are not respected as well (their presence or value doesn't matter).
    -   Why? If CORS was implemented, the previously existing behaviour of allowing cross-origin image requests (just how the Web model was made) would not work, thus breaking billions and billions of existing web sites/web apps. ([Source](https://stackoverflow.com/questions/33208474/why-do-font-files-have-to-abide-by-cors-rules-but-images-dont))
-   This was about images which use the `<img />` tag, but aren't included in a `<canvas>`. Those images require CORS.
    -   Why?

## Fonts

-   [Just like images](#images), cross-origin CSS stylesheets also do not require CORS.
-   Fonts that are loaded cross-origin though, always require CORS.
    -   Why? It was done to allow fonts to be used only on sites that have the font author's permission to use the font. ([Source](https://stackoverflow.com/questions/33208474/why-do-font-files-have-to-abide-by-cors-rules-but-images-dont))
-   Fonts are always loaded using [the `@font-face` CSS At-rule](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face).

### Loading Fonts Using `@font-face` in CSS

-   Show the `Access-Control-Allow-Origin` header in the response to the font request in the Network tab of the browser DevTools.
-   Change that header's value to a random host and show the request failing and the error in the console.
-   Explain the conditions that meet a Simple Request's conditions, which explains why there isn't a Preflight Request.

### Loading Fonts Using the `<link>` Tag in HTML

-   This is one of the ways Google Fonts loads its fonts.

    In HTML:

    ```html
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
    	href="https://fonts.googleapis.com/css2?family=Poppins&display=swap"
    	rel="stylesheet"
    />
    ```

    In CSS:

    ```css
    body {
    	font-family: Poppins;
    }
    ```

-   What Google Fonts does
    -   They first download a stylesheet that contains the `@font-face` CSS At-rules for all the requested fonts.
        -   Note that this is CSS being downloaded, so it doesn't need CORS. (Reason explained in the first point of this section.)
    -   The `@font-face` At-rules in the downloaded stylesheet then cause the actual font files to be downloaded.
        -   As these are font files, they need to abide by CORS.
-   Show the two requests in the Network tab. Show the CSS having the `@font-face` CSS At-rule and then the font request's initiator (in the Initiator tab) being that same CSS file.
-   Change the `Access-Control-Allow-Origin` header's value to a random host and **show that the CSS request is succeeding** but the font request is failing with an error in the console.

#### The `crossorigin` Attribute

-   Talk about [the `preconnect` Resource Hint](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preconnect) that is being used to improve the font loading performance.
-   Why are there two requests, one with the `crossorigin` attribute and one without?
    -   The browser opens separate connections for requests that use CORS and that don't use CORS.
    -   The stylesheet does not require CORS, thus the `<link>` tag without the `crossorigin` attribute is used.
    -   The font file requires CORS, thus the `<link>` tag with the `crossorigin` attribute is used, which informs the browser that this will be a Cross-Origin Request.
    -   Source and further reading: [preconnect resource hint and the crossorigin attribute](https://crenshaw.dev/preconnect-resource-hint-crossorigin-attribute)
    -   `crossorigin` and `crossorigin="anonymous"` mean the same.

### Loading Fonts Using `@import` in CSS

-   This is one of the ways Google Fonts loads its fonts.

    In CSS:

    ```css
    @import url("https://fonts.googleapis.com/css2?family=Poppins&display=swap");

    body {
    	font-family: Poppins;
    }
    ```

-   This does exactly what `<link href="https://fonts.googleapis.com/css2?family=Poppins&display=swap" rel="stylesheet" />` does in HTML, as explained in the above sub-section.

## Recap

-   CSS and `<img />` not in canvas do not require CORS due to backward compatibility reasons.
-   Font files require CORS due to author permission issues.
-   QnA

## To Do

-   Caching
    -   Optimization: caching CORS content (max-age header)
    -   `Vary` response header for responses varying on the basis of differing `Origin`, `Cookie` or other request header values. (Talk about allowing multiple origins through CORS. Maybe demo it?)

## Resources

-   [dev.harshkapadia.me/resources#cors](https://dev.harshkapadia.me/resources#cors)
-   [Same-Origin Policy](https://portswigger.net/web-security/cors/same-origin-policy)
-   [Same origin Policy and CORS](https://stackoverflow.com/questions/14681292/same-origin-policy-and-cors-cross-origin-resource-sharing)
-   [How exactly CORS is improving security](https://stackoverflow.com/questions/71294134/how-exactly-cors-is-improving-security)
