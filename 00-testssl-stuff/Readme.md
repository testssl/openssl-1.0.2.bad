
Compilation instructions
------------------------

**WARNING: Never use the resulting binaries for anything other than testing!**


If you want to compile OpenSSL yourself, here are the instructions:

1.) ```git clone --depth 1 https://github.com/testssl/openssl-1.0.2.bad &&
    cd openssl-1.0.2-bad```

2.) Now, there are two options to compile this. Recommended is the first one below.

-  A. Best is use the script make-openssl.sh from his directory. It configures and makes the binary of your choice.

-  B. Alternatively you can also compile everything manually. This branch has IPv6 builtin but you need the switch `-DOPENSSL_USE_IPV6` to enable IPv6.

	**for 64Bit including Kerberos ciphers:**

	    ./config --prefix=/usr/ --openssldir=/etc/ssl enable-zlib enable-ssl2 enable-rc5 enable-rc2 \
	    enable-GOST enable-cms enable-md2 enable-mdc2 enable-ec enable-ec2m enable-ecdh enable-ecdsa \
	    enable-seed enable-camellia enable-idea enable-rfc3779 enable-ec_nistp_64_gcc_128 \
	    --with-krb5-flavor=MIT experimental-jpake -DOPENSSL_USE_BUILD_DATE -DOPENSSL_USE_IPV6

	**for 64Bit, static binaries:**

	    ./config --prefix=/usr/ --openssldir=/etc/ssl enable-zlib enable-ssl2 enable-rc5 enable-rc2 \
	    enable-GOST enable-cms enable-md2 enable-mdc2 enable-ec enable-ec2m enable-ecdh enable-ecdsa \
	    enable-seed enable-camellia enable-idea enable-rfc3779 enable-ec_nistp_64_gcc_128 \
	    -static experimental-jpake -DOPENSSL_USE_BUILD_DATE -DOPENSSL_USE_IPV6

	**for 32 Bit including Kerberos ciphers:**

	    ./config --prefix=/usr/ --openssldir=/etc/ssl enable-zlib enable-ssl2 enable-rc5 enable-rc2 \
	    enable-GOST enable-cms enable-md2 enable-mdc2 enable-ec enable-ec2m enable-ecdh enable-ecdsa \
	    enable-seed enable-camellia enable-idea enable-rfc3779 no-ec_nistp_64_gcc_128 \
	    --with-krb5-flavor=MIT experimental-jpake -DOPENSSL_USE_BUILD_DATE -DOPENSSL_USE_IPV6

	 **for 32 Bit, static binaries:**

	    ./config --prefix=/usr/ --openssldir=/etc/ssl enable-zlib enable-ssl2 enable-rc5 enable-rc2 \
	    enable-GOST enable-cms enable-md2 enable-mdc2 enable-ec enable-ec2m enable-ecdh enable-ecdsa \
	    enable-seed enable-camellia enable-idea enable-rfc3779 no-ec_nistp_64_gcc_128 \
	    -static experimental-jpake -DOPENSSL_USE_BUILD_DATE -DOPENSSL_USE_IPV6

	Four GOST [1][2] ciphers come via engine support automagically with this setup. Two additional GOST
	ciphers can be compiled in (``GOST-GOST94``, ``GOST-MD5``) with ``-DTEMP_GOST_TLS`` but as of now they make
	problems under some circumstances, so unless you desperately need those ciphers I would stay away from
	``-DTEMP_GOST_TLS``.

	If you don't have / don't want Kerberos libraries and devel rpms/debs, just omit "--with-krb5-flavor=MIT"
	(see examples).  If you have another Kerberos flavor you would need to figure out by yourself.

   * make depend

   * make

   * make report (check whether it runs ok!)

   * ``./apps/openssl ciphers -V 'ALL:COMPLEMENTOFALL' | wc -l`` lists for me
      * 193(+4 GOST) ciphers including kerberos
      * 179(+4 GOST) ciphers without kerberos


Note that newer distributions provide more modern ciphers which this old openssl-1.0.2-bad doesn't have.  And will never have. Therefore openssl-1.0.2-bad has a lot of legacy ciphers don't have.
Kerberos support is very seldom used.


Enjoy, Dirk

[1] https://en.wikipedia.org/wiki/GOST_%29block_cipher%29

[2] http://fossies.org/linux/openssl/engines/ccgost/README.gost
