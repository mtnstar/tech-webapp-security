---
theme: gaia
_class: lead
paginate: true
backgroundImage: url(./bg.jpg)
color: white
---
# Web App Security 

---
# Content

1. Overview
1. CSRF
1. SameSite Cookies
1. CSP
1. Discussion

---
# Overview

1. Social Engineering
1. CSRF
1. Injection
1. HTTP Sessions
1. User Management

---
# ⚡CSRF

![opacity:.7 w:500 center](./img/csrf.png)

<!-- [source](https://medium.com/tresorit-engineering/modern-csrf-mitigation-in-single-page-applications-695bcb538eec) -->

---
```html
<a href="http://www.harmless.com/" onclick="
  var f = document.createElement('form');
  f.style.display = 'none';
  this.parentNode.appendChild(f);
  f.method = 'POST';
  f.action = 'http://www.example.com/account/destroy';
  f.submit();
  return false;">To the harmless survey</a>
```

---
# 🛡️ CSRF

* Use GET for read operations only 
* Use POST/PUT/PATCH/DELETE for changing operations
* Use CSRF tokens
* CORS
* SameSite Cookies

---
# CSRF and Single Page Applications

* no forms
* requests through API (XMLHttpRequest, fetch())
* api running on subdomain + CORS (Cross-origin resource sharing)

---
# SameSite Cookies

<style scoped>
  table { font-size: 80% }
</style>

```
Set-Cookie: widget_session=abc123; SameSite=Lax; Secure
```

| SameSite | |
|--|--|
| None | disable SameSite cookie setting, requires Secure |
| Lax | GET only: top-level navigation, for third party website access |
| Strict | only sent in first-party context |

---
# Content Security Policy (CSP)

<style scoped>
  table { font-size: 60% }
</style>

The application security Swiss Army Knife

| Setting | Values / Description |
|--|--|
| default-src | default policy for fetching all resources |
| script-src | allowed JavaScript sources |
| style-src | allowed CSS sources |
| connect-src | AJAX, Web Sockets, fetch(), ... sources |
| ... | ... |
| report-to | send failing request info to endpoint defined in Reporting_Endpoints header|

[content-security-policy.com](https://content-security-policy.com/)

---
# Rails 💗 CSP

```ruby
# config/initializers/content_security_policy.rb
Rails.application.config.content_security_policy do |policy|
  policy.default_src :self, :https
  policy.font_src    :self, :https, :data
  policy.img_src     :self, :https, :data
  policy.object_src  :none
  policy.script_src  :self, :https
  policy.style_src   :self, :https
 
  # Specify URI for violation reports
  policy.report_uri "/csp-violation-report-endpoint"
end
```

---
# Discussion

- How to secure our web apps?

---
# Sources

- [CSRF and SPA](https://medium.com/tresorit-engineering/modern-csrf-mitigation-in-single-page-applications-695bcb538eec)
- [Rails Security Guides](https://guides.rubyonrails.org/security.html)
- [SameSite Cookies](https://web.dev/samesite-cookies-explained/)
- [SameSite Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite)
- [Content Security Policy](https://www.youtube.com/watch?v=d0D3d0ZM-rI)
