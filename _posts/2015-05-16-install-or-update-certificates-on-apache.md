---
layout: post
title: "Install or update certificates on Apache"
description: ""
tags: [apache, certificates, openssl, web]
---
{% include JB/setup %}

I decided to write this post, as I spent some time researching this the last time I had to do it, so I figured I probably weren't the only one out there who were kinda clueless on this.

Whether you need to install or update a certificate on an Apache instance, the process is largely the same.
To sum it up you first have to create a certificate request (CSR), use the CSR during the ordering process at your favourite issuer, install it, and restart the daemon.

<!--more-->

### Create a key

If you don't already have a keyfile you would like to use for your CSR, and later your certificate, you need to create one. This can be done with the command below.
Follow the prompt to enter a passphrase, and reenter the passphrase to confirm it.

{% highlight bash %}
{% raw %}
openssl genrsa -des3 -out ~/keyfile.key 2048
{% endraw %}
{% endhighlight %}

### Create certificate request

To create the CSR run the following command, note how I pass the path of the keyfile as parameter.

{% highlight bash %}
{% raw %}
openssl req -new -key ~/keyfile.key -out ~/request.csr
{% endraw %}
{% endhighlight %}

### Order your certificate online

Whether your favorite supplier is RapidSSL, DigiCert or whatever, the process is largely the same. You choose the certificate type, and pass the content of the request.csr file, in order to receive your certificate.

### Install your certificate

If you're updating an existing certificate, you should be able to find the existing config lines with this command.

{% highlight bash %}
{% raw %}
grep -i -r "SSLCert" /etc/httpd/
{% endraw %}
{% endhighlight %}

Look for the following lines.

{% highlight bash %}
{% raw %}
SSLCertificateFile /etc/pki/tls/certs/certificate.crt
SSLCertificateKeyFile /etc/pki/tls/private/keyfile.key
SSLCertificateChainFile /etc/pki/tls/certs/CertificateAuthority.crt
{% endraw %}
{% endhighlight %}

If you're on the other hand configuring from scratch, you most likely won't have those lines to look for, though you may have them as samples.
But you should be able to locate your config file at the following path.

{% highlight bash %}
{% raw %}
/etc/httpd/sites-enabled/00-sitename
{% endraw %}
{% endhighlight %}

Add the lines noted above, and copy your certificate files and private key to the paths listed.

### Restart httpd

That should be it, now all you have to do is issue a restart of the httpd with the command below.

{% highlight bash %}
{% raw %}
sudo service httpd restart
{% endraw %}
{% endhighlight %}

Now, you'll notice that the daemon asks for the passphrase of the private key used for the certificate, and that's no good.
To remove the passphrase from the private key, run the command below.

{% highlight bash %}
{% raw %}
openssl rsa -in ~/keyfile.key -out ~/keyfile_nopwd.key
{% endraw %}
{% endhighlight %}

Now you need to copy over the new private key to the destination noted in your config. Once done, try issuing another httpd restart.
This time around is should just restart, without prompting for passphrase, and you're golden.

Now grab a Miller, and kick back :-)