# Deployment notes

Nếu là simple static front-end projects with HTML CSS javascript and do not require a server to run thì không cần dùng server như Apache. Just use static host like: Netlify, vercel, github pages

Purchase domain names and link to your hosting account via DNS

All live projects should use HTTPS/SSL

Full-stack projects, API cần dùng: AWS, digital ocean

## Web Hosting and Domain Hosting

Most people purchase both from the same provider

DNS match meaningful URLs with ip addresses

Có một công ty host domain, mình sẽ trả tiền mua nó.

Web hosting là chỗ store your files.

## Structure of URL

URL: Uniform Resource Locator

The `http://` part of the URl is the protocol/scheme

`http` transfer data in plain text, `https` encrypt data.

Nếu send passwords over a `http` protocol thì sẽ bị thấy.

`example.com.` is a **domain name**. Parts of DNs are separated by a period `.`; The trailing `.` is the **root** of the Internet's namespace.

Domain name are case-insensitive.

- `example` is the **secondary level domain** (SLD); the name of the website
- `com` top level domain (TLD). It gives you an idea of what sort of an entity the organization behind the website is.

Top level domain entities could be: `.com.`, `.gov.`, `.edu.`

In `blog.example.com`, the `blog` is the **sub-domain** of the SLD `example.`  
`www` is a sub-domain. Modern websites often omit `WWW` in URLs because it’s not required for functionality, but HTTP or HTTPS is essential for website security and performance.

For example, Google’s root domain is `www.google.com`. Subdomains của Google như là: `docs.google.com`, `ads.google.com`, `keep.google.com`

Sub-domain có thể có multiple layers separated by period symbol. For example, in `https://amazon.com.scamwebsite.com`, `scamwebsite.com` is the name of the website, not `example.com`. Domain name phải đọc right -> left (như văn ngôn cổ hihi).

In `anhao.com/public/home-page` thì `/public` là **page path** or directory, `/home-page` là **resource** (HTML web page).

In `example.com/?type=public&post=new-blog-post`, the stuff appearing after the `?` symbol is called a **query string**.

Do not click links you are suspicious (email, social media, text mobile)

## Bandwidth

Bằng thông không giới hạn (KGH)

## Manage domains

DNS Glossary:

- **Zone File**: The DNS configuration file for a domain.
- **Host Record**: Specifies the subdomain (e.g., @ = root, www, mail).

**DNS (Domain Name System) records** are instructions stored in your domain’s zone file. They translate human-readable domain names (like yourdomain.com) into technical data (like IP addresses or mail server locations) that browsers and email clients use.

Common DNS Record Types and What They Do:

- Type `A` points domain to an IPv4 address. Example: `@ → 192.0.2.1`
- Type `AAAA` points to an IPv6 address; Example: `@ → 2001:db8::1`
- `CNAME` Creates an alias to another domain; `www → yourdomain.com`

In a DNS zone file, the `@` symbol is a shortcut representing the domain name that the zone file is authoritative for, often called the "current origin". When you see an "@" in a DNS record, it signifies that the record applies to the root of the domain itself, rather than a specific subdomain like `www.` or "mail"
