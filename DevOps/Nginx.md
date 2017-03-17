# Nginx

```
http {
  client_max_body_size 10M;
}
```

## HTTP/2

To solve latency issues, HTTP/2 uses multiplexing. This does not mean you don't do concatenation anymore. In fact you still need to do it. Just split it up intelligently.

* [HTTP/2 FAQ](https://http2.github.io/faq)
* [The Right Way to Bundle Your Assets for Faster Sites over HTTP/2](https://medium.com/@asyncmax/the-right-way-to-bundle-your-assets-for-faster-sites-over-http-2-437c37efe3ff#.2qypy2vsy)

## SSL

PCI Security Council deprecated SSLv3 and TLS 1.0 for commercial transactions.

* [SSL and TLS Deployment Best Practices](https://github.com/ssllabs/research/wiki/SSL-and-TLS-Deployment-Best-Practices)
* [Is TLS fast yet?](https://istlsfastyet.com/)
* [SSL upgrades on rubygems.org and RubyInstaller versions](https://gist.github.com/luislavena/f064211759ee0f806c88)
* [The Guardian has moved to HTTPS](https://www.theguardian.com/info/developer-blog/2016/nov/29/the-guardian-has-moved-to-https)

---

* BEAST attack - 2011
* Logjam attack - 2015
* As of Jan 2016, you can't get SHA1 cert anymore. Use SHA256.

### Checklist

* OCSP stapling enabled?
* HTTP Strict Transport Security (HSTS) policy declared?