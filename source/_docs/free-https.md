---
title: Early Access: Free and Automated HTTPS
earlyaccess: true
description: Upgrade to Free and Automated HTTPS, powered by Fastly and Let's Encrypt
---
Upgrade your sites's HTTPS and never manage a certificate again. Pantheon automatically adds all of your site's domains to a shared certificate and  serves it through our globally distributed content delivery network (CDN). Upgrade to take advantage of cost, performance, and security benefits.

## Sites Eligible for Early Access (as of 4/1/2017)
* Sites on a Professional plan with HTTPS already enabled
  - [Request invite](http://learn.pantheon.io/201701-HTTPS-Reg.html)


## Comparison with Legacy HTTPS

|    |     Legacy      |  Free & Automated  |
|----------|:-------------:|------:|
| Price | $30/mo per environment |  Free for all environments |
| Certificate type | Bring your own| Shared |
| Renewal | Manual | Automatic |
| Unique inbound IP Address | Yes | No |
| Client support | ?? | Some very older browsers not supported |
| SSL Labs Rating | A  | A+ |
| Protocol | vulnerable TLS 1.1 |    TLS 1.2 only   |
| Ciphers | vulnerable 3DES cipher | no 3DES cipher |
| Delivery | Served from Chicago | Global CDN |

## Upgrade Your Site

1. Click the **Start HTTPS Upgrade** button from the Site Dashboard.
2. Wait 25 minutes to an hour for HTTPS provisioning to complete.
3. Once HTTPS is ready, you will see action required on the **Domains & HTTTPS** tab for each environment, with instructions to configure DNS for each custom domain.  
4. Click **Show DNS Recommendations** next to each custom domain to identify DNS values needed to point the domain to your site. Domains that are not yet configured will indicate action is required.
5. Configure DNS using the provided destinations at the domain's DNS provider.

    <div class="alert alert-info">
    <h3 class="info">Pro Tip</h3>
Look up your DNS provider with this free web tool: <a href="https://mxtoolbox.com/DNSLookup.aspx">https://mxtoolbox.com/DNSLookup.aspx</a>
</div>

6. Wait for your DNS changes to fully propagate. DNS records are cached across the internet and can take up to 72 hours to propagate, depending on the time to live (TTL) configured for the domain's DNS records.

      <div class="alert alert-info">
      <h3 class="info">Pro Tip</h3>
Check the current state of DNS propagation from different parts of the world using this free web tool <a href="https://www.whatsmydns.net/">https://www.whatsmydns.net/</a>
</div>

7. Confirm the upgrade is complete from the **Domains & HTTPS** tab. Once DNS has been configured and propagated, you will see confirmation on this tab.

## Require HTTPS - Optional

### 301 Redirects

You're likely already issuing 301 redirects via the WordPress `wp-config.php` file or the Drupal `settings.php` file to require HTTPS and standardize on a common domain. Standardizing on a common domain (for example, `https://www.example.com` or `https://example.com`) is a best practice for SEO to prevent duplicate content, as well as session strangeness, where a user can be signed on one domain but logged out of other domains at the same time. See [recommended code samples for redirects](/docs/redirects/#require-https-and-standardize-domain) and adapt for your specific use case.

### HTTP Strict Transport Security Header

We also recommend sending a HTTP Strict Transport Security (HSTS) header with a max age of `15552000` (180 days). This is the last step to get an A+ SSL rating from [SSL Labs](https://www.ssllabs.com/ssltest/) and helps to protect your website against protocol downgrade attacks and cookie hijacking.


#### WordPress
* Activate the [LH HSTS](https://wordpress.org/plugins/lh-hsts/) plugin

#### Drupal
1. Install the
[HTTP Strict Transport Security](https://drupal.org/project/hsts) module
2. Visit module configuration page (/admin/config/security/hsts)
3. Check the **Enable HTTP Strict Transport Security** checkbox, set **Max Age** to `15552000` and **Save Configuration**



## Frequently Asked Questions

### Does upgrading involve HTTPS interruption or downtime?
No, after you update your DNS records, traffic will gracefully switch over and involves no downtime or HTTPS interruption.

**Caveat:** If after upgrading you add a new domain that is not already routed to Pantheon, then it will take 25 minutes to an hour for HTTPS to be ready for that new domain. Pre-provisioning HTTPS for new domains is planned after early access, with the full release.

### Which browsers and operating systems are supported?
All modern browsers and operating systems are supported. For details, see the **Handshake Simulation** portion of this [report](https://www.ssllabs.com/ssltest/analyze.html?d=pantheon.io).

### When do I stopped being billed $30/month?
Pantheon will remove legacy load balancers and stop billing within XYZ days of upgrading.

### Can I downgrade back to the legacy HTTPS?
Yes, if you wish to downgrade, please contact Pantheon support.

* If you request the upgrade before XYZ days after completing the upgrade, then your existing legacy load balancer will be available, with the same IP address and certificate.
* If you request the upgrade after XYZ days, you will be able to load a new certificate to a new load balancer with a new IP address.

### What level of encryption is provided?
High grade TLS 1.2 encryption with up-to-date ciphers. For a deep analysis of the HTTPS configuration on upgraded sites see [this A+ SSL Labs report for https://pantheon.io](https://www.ssllabs.com/ssltest/analyze.html?d=pantheon.io).

### How can I obtain an A+ SSL Labs rating?
Follow the steps to upgrade and send the [HSTS header](#http-strict-transport-security-header) as described above.

### Are wildcard certificates supported?
No, but you don’t need a wildcard certificate to secure communications for multiple domains because we will automatically deploy certificates for all domains on your site.

### Is Extended Validation supported?
No, please take a moment to fill out the [HTTPS survey](/docs/getting-support) if you require Extended Validation.

### Is the CDN configurable? Do I get access to hit rates or other statistics?
No, we pre-configured the CDN so you don’t have to hassle with configuration. We’ve optimized configuration for Drupal and WordPress sites. Hit rates or other statistics are not available.


## Glossary

### HTTPS


### TLS (Transport Layer Security)
TLS (Transport Layer Security) is a protocol for secure HTTP connections. This protocol replaces its less secure predecessor, the **SSL (Secure Socket Layer)** protocol, which we no longer support. Pantheon uses the term HTTPS to refer to secure HTTP connections.

### Server Name Indication (SNI)
Server name indication (SNI) is the technology replacing the expensive, legacy load balancers and allows multiple secure (HTTPS) websites to be served off the same IP address, without requiring all those sites to use the same certificate.

## Troubleshooting
### 503 Timeouts
If a web request exceed the 60 second timeout limit, you may encounter

- 503 connection timeout
- 503 first byte timeout
- 503 between bytes timeout

These errors occur when a request exceeds the 60 second timeout limit. There is currently no way to exceed or override the 60s timeout.

### 503 Header Overflow
If a request exceeds the 10K size limit for cookies, all cookies will be dropped and the request will continue to be processed as if no cookies had been sent at all. The header `"X-Cookies-Dropped: 1"` will be added to the request and response indicating that these have been truncated. You can either ignore this scenario in your PHP code or handle it (perhaps by displaying a custom error page). Previously, with legacy HTTPS a [502 - Upstream Header Too Big](/docs/errors-and-server-responses/#502-upstream-header-too-big) error would be returned.

### Infinite Redirect Loops
Errors referencing too many redirects may be a result of using the ` $_SERVER['HTTP_X_FORWARDED_PROTO']` variable within redirect logic located in your site's `wp-config.php` or `settings.php` file.
**Solution:**  Replace the offending redirect logic with a [recommended code sample](/docs/redirects/#require-https-and-standardize-domain) and adapt it for your specific use case.

### Moz Pro 804 HTTPS SSL error
Currently, Moz Pro is unable to crawl sites using Server Name Indication (SNI). While full support for SNI is not yet available, you may be eligible for beta access to SNI support. For details, see [Moz Pro, our web crawler, and sites that use SNI (804 HTTPS SSL) error](https://moz.com/community/q/moz-pro-our-web-crawler-and-sites-that-use-sni).