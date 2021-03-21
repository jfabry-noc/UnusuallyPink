+++
title = "PSA: Get Ready For New Let's Encrypt Validation"
date = "2019-04-08"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["certificates", "encryption", "lets-encrypt", "web-development"]
tags = ["certificates", "encryption", "lets-encrypt", "web-development"]
+++

If you’re using [Let’s Encrypt](https://letsencrypt.org/), now would be a really great time to make sure that you’re ready for them to stop supporting ACME TLS-SNI-01 domain validation. I got an email a couple of days ago (as I assume everyone using Let’s Encrypt did) letting me know this change was coming. I had nothing to actually do, but going through the validation was super easy and is likely worth the time to ensure your site(s) aren’t impacted. March 13th is the deadline for ACME TLS-SNI-01 to no longer function, so there’s still a lot of time to take a couple of minutes and verify you’re in good shape.

\*Note: I’m using `certbot`, which makes this whole thing super easy. If you’re **not** using `certbot` then your steps will be different.\*

The [Let’s Encrypt staging environment](https://letsencrypt.org/docs/staging-environment/) already has disabled ACME TLS-SNI-01 validation, so checking against that is a good test. As a `certbot` user, I also needed to validate that I was using at least version 0.28 of the application, which is simple enough to do via:

```
certbot --version    
```

That appears to be the latest version offered by the PPA: `ppa:certbot/certbot`

Testing a `certbot` run against the staging environment is toggled via the `--dry-run` switch. If you do a dry run of your renewal against the staging environment and everything comes back successful, you should be in good shape:

```
sudo certbot renew --dry-run
```

My certs all validated successfully, so everything is ready to go for the change. I presume if there are any failures then the dry run will alert you to what needs to be fixed; I can’t say for sure since I was lucky enough to not see any of those. Full instructions from Let’s Encrypt are [available on their site](https://community.letsencrypt.org/t/how-to-stop-using-tls-sni-01-with-certbot/83210), though.

Happy encrypting!
