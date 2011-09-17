# FakeSMTP

FakeSMTP is a PHP script written to act like any traditional postfix SMTP server. Used in conjunction with inted, the script is called when a mail client connects to port 25 on the server.

This script does not actually send any emails (although this could theoretically be combined with sendmail if one wanted to). It also does not contain any authorization functionality, and thus acts solely as a badly configured open SMTP relay. I wrote it to create a honeypot for spammers as well as for testing & proof of concept.


## Installation

fakeSMTP relies on inetd. First copy the script to a suitable path, and make sure your script is executable (chmod +x fake-server.php). Once inetd is installed (also known as openbsd-inetd), add a line to your /etc/inetd.conf :

```
smtp   stream    tcp    nowait    root    /path/to/fake-server.php
```

## Usage

```php
<?php
require('fakeSMTP.php');

$hp = new fakeSMTP;
$hp->serverHello = 'axllent.org ESMTP Postfix'; // Server identity (optional)
$hp->logFile = '/tmp/emails.log'); // Log the transaction files (optional)
$hp->receive();
if (!$hp->mail['rawEmail']) {
    exit; // Script failed to receive a complete transaction
}
/* The script returns all the mail parts which you can process
in $hp->mail(array) - read source for all details */
```
