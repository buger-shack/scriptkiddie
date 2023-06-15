# Security Headers

<figure><img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExZDhlMWQ2NTkzNDNiZDBhNTgzNzkzM2Y1OTNkYzQwNjIxNmViN2UzOCZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/AMfgcGOLMqADK/giphy.gif" alt=""><figcaption></figcaption></figure>

#### Source

{% embed url="https://owasp.org/www-project-secure-headers" %}

## Tools

### SSL/TLS/Ciphers

#### `testssl.sh`

{% embed url="https://github.com/drwetter/testssl.sh" %}
testssl.sh
{% endembed %}

{% embed url="https://www.ssllabs.com/ssltest" %}
SSL Scan
{% endembed %}

### Security Headers

{% embed url="https://github.com/saladandonionrings/NextGen-HeadersScanner" %}
Test Security Headers implementation with this tool
{% endembed %}

```bash
# install | usage
git clone https://github.com/saladandonionrings/NextGen-HeadersScanner.git
cd NextGen-HeadersScanner/
pip install -r requirements.txt
python h_scan -u https://$target
```

## :white\_check\_mark: Good

### Strict-Transport-Security (HSTS)

The HTTP Strict-Transport-Security response header (often abbreviated as HSTS) lets a web site tell browsers that it should only be accessed using **HTTPS**, instead of using HTTP.

| Value               | Description                                                                                               |
| ------------------- | --------------------------------------------------------------------------------------------------------- |
| `max-age=SECONDS`   | The time, in seconds, that the browser should remember that this site is only to be accessed using HTTPS. |
| `includeSubDomains` | If this optional parameter is specified, this rule applies to all of the site’s subdomains as well.       |

#### Best Synthax

```
Strict-Transport-Security: max-age=<expire-time>
Strict-Transport-Security: max-age=<expire-time>; includeSubDomains
Strict-Transport-Security: max-age=<expire-time>; preload
```

#### Directives

`max-age=<expire-time>` The time, in seconds, that the browser should remember that a site is only to be accessed using HTTPS.

`includeSubDomains Optional` If this optional parameter is specified, this rule applies to all of the site's subdomains as well.

`preload Optional` See Preloading Strict Transport Security for details. Not part of the specification.

### Content Security Policy (CSP)

A Content Security Policy (CSP) requires careful tuning and precise definition of the policy. If enabled, CSP has significant impact on the way browsers render pages (e.g., inline JavaScript disabled by default and must be explicitly allowed in policy). CSP prevents a wide range of attacks, including **Cross-site scripting** and other cross-site injections.

| Directive                   | Description                                                                                                                                                                                               |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `base-uri`                  | Define the base URI for relative URIs.                                                                                                                                                                    |
| `default-src`               | Define loading policy for all resources type in case a resource type’s dedicated directive is not defined (fallback).                                                                                     |
| `script-src`                | Define which scripts the protected resource can execute.                                                                                                                                                  |
| `object-src`                | Define from where the protected resource can load plugins.                                                                                                                                                |
| `style-src`                 | Define which styles (CSS) can be applied to the protected resource.                                                                                                                                       |
| `img-src`                   | Define from where the protected resource can load images.                                                                                                                                                 |
| `media-src`                 | Define from where the protected resource can load video and audio.                                                                                                                                        |
| `frame-src`                 | _(Deprecated and replaced by `child-src`)_ Define from where the protected resource can embed frames.                                                                                                     |
| `child-src`                 | Define from where the protected resource can embed frames.                                                                                                                                                |
| `frame-ancestors`           | Define from where the protected resource can be embedded in frames.                                                                                                                                       |
| `font-src`                  | Define from where the protected resource can load fonts.                                                                                                                                                  |
| `connect-src`               | Define which URIs the protected resource can load using script interfaces.                                                                                                                                |
| `manifest-src`              | Define from where the protected resource can load manifests.                                                                                                                                              |
| `form-action`               | Define which URIs can be used as the action of HTML form elements.                                                                                                                                        |
| `sandbox`                   | Specifies an HTML sandbox policy that the user agent applies to the protected resource.                                                                                                                   |
| `script-nonce`              | Define script execution by requiring the presence of the specified nonce on script elements.                                                                                                              |
| `plugin-types`              | Define the set of plugins that can be invoked by the protected resource by limiting the types of resources that can be embedded.                                                                          |
| `reflected-xss`             | Instruct the user agent to activate or deactivate any heuristics used to filter or block reflected cross-site scripting attacks, equivalent to the effects of the non-standard `X-XSS-Protection` header. |
| `block-all-mixed-content`   | Prevent the user agent from loading mixed content.                                                                                                                                                        |
| `upgrade-insecure-requests` | Instruct the user agent to download insecure HTTP resources using HTTPS.                                                                                                                                  |
| `referrer`                  | _(Deprecated)_ Define information the user agent can send in the `Referer` header.                                                                                                                        |
| `report-uri`                | _(Deprecated and replaced by `report-to`)_ Specifies a URI to which the user agent sends reports about policy violation.                                                                                  |
| `report-to`                 | Specifies a group (defined in the `Report-To` header) to which the user agent sends reports about policy violation.                                                                                       |

#### Example

```
Content-Security-Policy: script-src 'self'
```

### X-Frame Options

X-Frame-Options response header improve the protection of web applications against **Clickjacking**. It declares a policy communicated from a host to the client browser on whether the browser must not display the transmitted content in frames of other web pages.

| Value                | Description                                             |
| -------------------- | ------------------------------------------------------- |
| `deny`               | No rendering within a frame.                            |
| `sameorigin`         | No rendering if origin mismatch.                        |
| `allow-from: DOMAIN` | Allows rendering if framed by frame loaded from DOMAIN. |

#### Example

```
X-Frame-Options: deny
```

### Referrer-Policy

The Referrer-Policy HTTP header governs which referrer information, sent in the Referer header, should be included with requests made.

| Value                             | Description                                                                                                                                                                                                             |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `no-referrer`                     | The `Referer` header will be omitted entirely. No referrer information is sent along with requests.                                                                                                                     |
| `no-referrer-when-downgrade`      | This is the user agent’s default behavior if no policy is specified. The origin is sent as referrer to a-priori as-much-secure destination (HTTPS → HTTPS), but isn’t sent to a less secure destination (HTTPS → HTTP). |
| `origin`                          | Only send the origin of the document as the referrer in all cases. (e.g. the document `https://example.com/page.html` will send the referrer `https://example.com/`.)                                                   |
| `origin-when-cross-origin`        | Send a full URL when performing a same-origin request, but only send the origin of the document for other cases.                                                                                                        |
| `same-origin`                     | A referrer will be sent for same-site origins, but cross-origin requests will contain no referrer information.                                                                                                          |
| `strict-origin`                   | Only send the origin of the document as the referrer to a-priori as-much-secure destination (HTTPS → HTTPS), but don’t send it to a less secure destination (HTTPS → HTTP).                                             |
| `strict-origin-when-cross-origin` | Send a full URL when performing a same-origin request, only send the origin of the document to a-priori as-much-secure destination (HTTPS → HTTPS), and send no header to a less secure destination (HTTPS → HTTP).     |
| `unsafe-url`                      | Send a full URL (stripped from parameters) when performing a same-origin or cross-origin request.                                                                                                                       |

#### Example

```
Referrer-Policy: no-referrer
Referrer-Policy: no-referrer-when-downgrade
Referrer-Policy: origin
Referrer-Policy: origin-when-cross-origin
Referrer-Policy: same-origin
Referrer-Policy: strict-origin
Referrer-Policy: strict-origin-when-cross-origin
Referrer-Policy: unsafe-url
```

### X-Content-Type-Options

Setting this header will prevent the browser from interpreting files as something else than declared by the content type in the HTTP headers.

| Value     | Description                                                                                 |
| --------- | ------------------------------------------------------------------------------------------- |
| `nosniff` | Will prevent the browser from MIME-sniffing a response away from the declared content-type. |

**Example**

```
X-Content-Type-Options: nosniff
```

### X-Permitted-Cross-Domain-Policies <a href="#x-permitted-cross-domain-policies" id="x-permitted-cross-domain-policies"></a>

A cross-domain policy file is an XML document that grants a web client, such as Adobe Flash Player or Adobe Acrobat (though not necessarily limited to these), permission to handle data across domains. When clients request content hosted on a particular source domain and that content makes requests directed towards a domain other than its own, the remote domain needs to host a cross-domain policy file that grants access to the source domain, allowing the client to continue the transaction. Normally a meta-policy is declared in the master policy file, but for those who can’t write to the root directory, they can also declare a meta-policy using the `X-Permitted-Cross-Domain-Policies` HTTP response header.

| Value             | Description                                                                                                            |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `none`            | No policy files are allowed anywhere on the target server, including this master policy file.                          |
| `master-only`     | Only this master policy file is allowed.                                                                               |
| `by-content-type` | \[HTTP/HTTPS only] Only policy files served with Content-Type: text/x-cross-domain-policy are allowed.                 |
| `by-ftp-filename` | \[FTP only] Only policy files whose file names are crossdomain.xml (i.e. URLs ending in /crossdomain.xml) are allowed. |
| `all`             | All policy files on this target domain are allowed.                                                                    |

#### Example <a href="#example-4" id="example-4"></a>

```
X-Permitted-Cross-Domain-Policies: none
```

## :x: Deprecated

{% tabs %}
{% tab title="HTTP Public Key Pinning (HPKP)" %}
{% hint style="danger" %}
**No longer recommended. Deprecated.**

Though some browsers might still support it, it may have already been removed from the relevant web standards, may be in the process of being dropped, or may only be kept for compatibility purposes.&#x20;

Be aware that this feature may cease to work at any time.
{% endhint %}
{% endtab %}

{% tab title="X-XSS-Protection" %}
{% hint style="danger" %}
**No longer recommended. Deprecated.**

This feature is non-standard and is not on a standards track. Do not use it on production sites facing the Web: it will not work for every user. There may also be large incompatibilities between implementations and the behavior may change in the future.

Please use `Content-Security-Policy` instead.
{% endhint %}
{% endtab %}

{% tab title="Expect-CT" %}
{% hint style="danger" %}
**No longer recommended. Deprecated.**

This feature is no longer recommended. Though some browsers might still support it, it may have already been removed from the relevant web standards, may be in the process of being dropped, or may only be kept for compatibility purposes

Obsolete since June 2021.
{% endhint %}
{% endtab %}
{% endtabs %}
