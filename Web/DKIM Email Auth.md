# Email Authentication with DKIM

It is possible to spoof an email senders domain and so send malicious email purporting to be from a valid domain.
DKIM (Domain Key Identified Mail) attempts to resolve this issue using DNS TXT entries to store pubic keys for an email source a receiver can then use the key to decrypt the message (or part of the message).


Sample mail header from Google:

~~~~
Received-SPF: pass (google.com: domain of 3w5cWVwwKAwwm114-z03q1xAs00sxq.o0y30n5tqt63xqA4.zq5@unified-notifications.bounces.google.com designates 2607:f8b0:400e:c03::245 as permitted sender) client-ip=2607:f8b0:400e:c03::245;
Authentication-Results: mx.google.com;
       dkim=pass header.i=@google.com;
       spf=pass (google.com: domain of 3w5cWVwwKAwwm114-z03q1xAs00sxq.o0y30n5tqt63xqA4.zq5@unified-notifications.bounces.google.com designates 2607:f8b0:400e:c03::245 as permitted sender) smtp.mailfrom=3w5cWVwwKAwwm114-z03q1xAs00sxq.o0y30n5tqt63xqA4.zq5@unified-notifications.bounces.google.com;
       dmarc=pass (p=REJECT dis=NONE) header.from=google.com
Received: by mail-pa0-x245.google.com with SMTP id zy2so36000852pac.1
        for <XXXXXXXXXXXXXX>; Tue, 19 Apr 2016 13:40:35 -0700 (PDT)
DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed;
        d=google.com; s=20120113;
        h=mime-version:date:message-id:subject:from:to;
        bh=YKu0gVc4XjX6Rxxmd2pm+HOaO9QFH6GLvPQVmAI9utQ=;
        b=Vi8vF2bazOyf6dNk7saV6/M9vAu7kWHPAsF42v7e7Y49i9TK/cz5IIoSlBW4gHQ8nt
         wE/L4RQOVBTXiLc8Q6DoUUQCRsn9uSvV7pSotlo+YxIA7CYui/bHr0MiU70cMjZwV2Gu
         sJwxoXYgBYvuvJ7T71me+r6jYs3dMZReFWGzx1RVmj4TLRtwozNqemJjIqChKtnMXdHL
         opzQptlgFB/IB9e51eyj1biS4knb4xITUhFHHIMRSQ51UCvMT+yS91rfAYv/URtCmVo8
         rTZDGzncnFLNATk+X/xgfu2iI0ICB6f4K2dzLzmaZETxYpKwK3fjL3qYtCq4hEScvdiY
         UdgQ==
X-Google-DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed;
        d=1e100.net; s=20130820;
        h=x-gm-message-state:mime-version:date:message-id:subject:from:to;
        bh=YKu0gVc4XjX6Rxxmd2pm+HOaO9QFH6GLvPQVmAI9utQ=;
        b=OkkUtwE8aku5qKbkBWAKkp1I0Y6FG7FZgok6qPZuMBsmx8FhC/b70/VtvRXVV/tgLC
         ODDHeq+ZV6taoI4B2gR1RrxbgOa7TTG13hKb3/xU2tYG5pzvSLEHeJB5k2dfZl/eqLQo
         sSOQQibDL9MICdKUX4QWmrX3jHBWNYOtbsn7mTMwfi0Ld9XOPygznPaxyKVcWlwrXpBo
         NKgljQL/9Nq7YVeMGv6OJUx0BJ9YvsOqJBhnje5NtboSqp71zs9vtNw3V9nsJjwZl5hG
         Wy2iAC86r6iry7qtgzjp1q1AL6J4IuAve7A6efsG0sAvUONLLyDao75Lb+dO9OhFEW7g
         C8qQ==
X-Gm-Message-State: AOPr4FXY0mqodnZ+GjDbB6CA0fufw4gFUW4ANQ2LrH37c107zdQVc5cd9/MU/oqNpn53PgY1ormC70nDt9ongHs=
~~~~


There are two types of DNS TXT records setup for DKIM:

### Policy Records

Policy records are prefixed with "_domainkey".
They contain information about the domain's usage of DKIM, wherether all or some emails are encrypted.
Typical values are: "o=~"; "o=~\; r=IT CONTACT EMAIL HERE"
"o=-" means all mail is encrypted. "o=~" means some mail is encrypted.


### Public Key Records

These DNS TXT records contain the public keys used to encrypt the messages.
They are typically of the format:
~~~~
[selector]._domainkey.[domain];    IN TXT    "v=DKIM1; p=[Public Key]"
~~~~

If you look at the above example mail header, you see a number of fields after DKIM-Signature & X-Google-DKIM-Signature.

DKIM Headers:

|Header|Description|
|------|-----------|
|v|Version if DKIM in Use. e.g. "v=DKIM1"|
|a|algorithm used to generate keys e.g. a=rsa-sha256|
|c|Pre-processing of headers|
|d|The Domain issuing the signature|
|s|The selector used to find the DKIM public key in DNS|
|t|Signature timestamp|
|bh|A crypto hash of part of the message|
|h|A list of mail headers that have been signed|
|b|The mail signature itself|


So, for the google message:

~~~~
d=1e100.net; s=20130820
~~~~

We should lookup the DKIM key using dig:

~~~~
# dig -t TXT 20130820._domainkey.1e100.net
...
;; QUESTION SECTION:
;20130820._domainkey.1e100.net. IN      TXT

;; ANSWER SECTION:
20130820._domainkey.1e100.net. 86400 IN TXT     "k=rsa\; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAnOv6+Txyz+SEc7mT719QQtOj6g2MjpErYUGVrRGGc7f5rmE1cRP1lhwx8PVoHOiuRzyok7IqjvAub9kk9fBoE9uXJB1QaRdMnKz7W/UhWemK5TEUgW1xT5qtBfUIpFRL34h6FbHbeysb4szi7aTg" "erxI15o73cP5BoPVkQj4BQKkfTQYGNH03J5Db9uMqW/NNJ8fKCLKWO5C1e+NQ1lD6uwFCjJ6PWFmAIeUu9+LfYW89Tz1NnwtSkFC96Oky1cmnlBf4dhZ/Up/FMZmB9l7TA6gLEu6JijlDrNmx1o50WADPjjN4rGELLt3VuXn09y2piBPlZPU2SIiDQC0qX0JWQIDAQAB"

;; AUTHORITY SECTION:
1e100.net.              172800  IN      NS      ns1.google.com.
1e100.net.              172800  IN      NS      ns2.google.com.
1e100.net.              172800  IN      NS      ns4.google.com.
1e100.net.              172800  IN      NS      ns3.google.com.

;; ADDITIONAL SECTION:
ns1.google.com.         109791  IN      A       216.239.32.10
ns2.google.com.         23359   IN      A       216.239.34.10
ns3.google.com.         109790  IN      A       216.239.36.10
ns4.google.com.         23359   IN      A       216.239.38.10
...
~~~~
