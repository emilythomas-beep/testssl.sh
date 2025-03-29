### FAQs

This is a collection of frequently asked questions which should help to answer some of those or gain some insights. It could be helpful reading before filing issues.

#### Coding

* Why are you using bash, everybody nowadays uses (python|Golang|Java|etc), it's much faster and modern!
   * The project started in 2007 as series of OpenSSL commands in a shell script which was used for pen testing. OpenSSL then was the central part (and partly is) to do some basic operations for connections and certificates verification which would have been more tedious to implement in other programming languages. Over time the project became bigger and it in terms of resources it wasn't a viable option to convert it to (python|Golang|Java|etc). Besides, bash is easy to debug as opposed to a compiled binary. Personally, I believe its capabilities are often underestimated.

* But why don't you now amend it with a (python|perl|Golang|Java|etc) function which does \<ABC\> or \<DEF\> much faster?
   * The philosophy and the beauty of testssl.sh is that it runs *everywhere* with a minimal set of dependencies like typical Unix binaries. No worries about having a different version of libraries/ interpreter not installed.


#### Runtime

* I believe I spotted a false positive as testssl.sh complained about a finding \<XYZ\> but my OpenSSL command `openssl s_client -connect <host:port> <MoreParameters> </dev/null` showed no connection.
   * First of all modern operating systems have disabled some insecure features. You can temporarily re-enable those by using `openssl s_client -connect <host:port>  -cipher 'DEFAULT@SECLEVEL=0'  <MoreParameters> </dev/null `. Or use the (by testssl.sh project) supplied OpenSSL-bad version like `OPENSSL_CONF='' ./bin/openssl.Linux.x86_64 s_client -connect <host:port>  -cipher 'DEFAULT@SECLEVEL=0 <MoreParameters>'  </dev/null`.
   * There is other bad crypto though which you can't test this way, e.g. modern OS supply OpenSSL binaries which have [SSLv2 and SSLv3 disabled in the source code or at least when compiling](https://docs.openssl.org/3.3/man7/ossl-guide-tls-introduction/#what-is-tls) which you can't re-enable during runtime. OTOH the supplied OpenSSL-bad version  from our project has this and more enabled but doesn't support TLS 1.3 or modern elliptic curves.
   * To amend any lack of capabilities of any OpenSSL version testssl.sh is picking up we're using bash sockets ([random picked example](https://www.xmodulo.com/tcp-udp-socket-bash-shell.html)).
