# CSRF

<details>

<summary>What is a CSRF attack ?</summary>

In a **C**ross-**S**ite **R**equest **F**orgery attack, a **malicious site tricks a victim's browser into making an unwanted request** to a different site on which the victim is authenticated, potentially causing the victim to perform an action on that site without their knowledge or consent.

#### How does it work?

_Imagine a scenario where you're logged into your online banking. While still logged in, you visit a different website that has some malicious code. This malicious website can send a request to your bank's website to transfer money without your knowledge. If the bank's website doesn't have proper CSRF protections, it would think that you made the request because your authentication cookies are automatically included by your browser._

#### Key points about CSRF:

1. **Relies on the authenticated state**: The attack works because browsers automatically include any cookies associated with a domain in requests made to that domain. So if you're authenticated to a website, that means any requests made to that website (even from a different website) will include your cookies.
2. **Doesn't steal data directly**: CSRF isn't about stealing data. Instead, it tricks the victim into performing actions without their knowledge.
3. **Exploits trusted relationships**: CSRF exploits the trust a website has in the user's browser, not necessarily a flaw in the website's design (although lack of CSRF protections is considered a design flaw).

#### How to prevent CSRF attacks?

1. **Use Anti-CSRF Tokens**: The most common way to prevent CSRF attacks is to use anti-CSRF tokens. This involves sending a random token in each request which the server verifies. Since the malicious site won't know this token, it can't forge a valid request.
2. **SameSite Cookie Attribute**: Modern browsers support the `SameSite` cookie attribute, which can prevent the browser from sending cookies along with cross-site requests, mitigating the risk of CSRF attacks.
3. **Check the `Referer` and `Origin` Headers**: Servers can check these HTTP headers to see if a request is coming from a trusted origin.
4. **Require Reauthentication for Sensitive Actions**: For very sensitive operations, like changing a password, always prompt users to re-enter their current password.
5. **Be cautious with CORS**: Cross-Origin Resource Sharing (CORS) headers shouldn't be used recklessly, as they can allow unwanted cross-site interactions.

</details>

<figure><img src="../../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>
