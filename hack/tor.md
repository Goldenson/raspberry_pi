# Push your app to the deep

In this tutorial, we will see how running an app from tor network.

I've used a Ruby on Rails 5 app running locally on my macbook.

## Install the tor client

```
brew install tor
```

This will install tor on your machine and will allow us to create a connection
to the tor network.


## Run the web server locally

1 - Go to `/usr/local/etc/tor`
2 - Create a folder called `hidden_service` and `cd` inside it
3 - Paste your amazing app inside and `cd` inside it one more time
4 - Run your web server with `rails server`


## Edit config file

You can make a copy of `/usr/local/etc/tor/torrc.sample` and create your own
`torrc`.

Now we will create our hidden service by letting tor knows where to find our app.

Open `/usr/local/etc/tor/torrc` with your favorite editor and add the following lines:

```
HiddenServiceDir name_of_your_amazing_app
HiddenServicePort 80 127.0.0.1:3000
```


## Push to the deep

Time to deep! Run the `tor` command you should see something like this:

```
Mar 11 17:16:50.688 [notice] Tor 0.2.9.10 (git-934nefhwieuhf248) running on Darwin with Libevent 2.1.8-stable, OpenSSL 1.0.2k and Zlib 1.2.8.
Mar 11 17:16:50.689 [notice] Tor can't help you if you use it wrong! Learn how to be safe at https://www.torproject.org/download/download#warning
Mar 11 17:16:50.689 [notice] Read configuration file "/usr/local/etc/tor/torrc".
Mar 11 17:16:50.692 [warn] Path for HiddenServiceDir (name_of_your_amazing_app) is relative and will resolve to /usr/local/etc/tor/name_of_your_amazing_app. Is this what you wanted?
Mar 11 17:16:50.692 [notice] Opening Socks listener on 127.0.0.1:9050
Mar 11 17:16:50.000 [notice] Parsing GEOIP IPv4 file /usr/local/Cellar/tor/0.2.9.10/share/tor/geoip.
Mar 11 17:16:50.000 [notice] Parsing GEOIP IPv6 file /usr/local/Cellar/tor/0.2.9.10/share/tor/geoip6.
Mar 11 17:16:50.000 [notice] Bootstrapped 0%: Starting
Mar 11 17:16:51.000 [notice] Bootstrapped 80%: Connecting to the Tor network
Mar 11 17:16:52.000 [notice] Bootstrapped 85%: Finishing handshake with first hop
Mar 11 17:16:52.000 [notice] Bootstrapped 90%: Establishing a Tor circuit
Mar 11 17:16:53.000 [notice] Tor has successfully opened a circuit. Looks like client functionality is working.
Mar 11 17:16:53.000 [notice] Bootstrapped 100%: Done
```

This will generate a folder with the name of your app. Almost there my friend!

Now we need to `cd` inside this new folder and `cat` the content of the file called `hostname`
which was created by tor.

Bim baby! We now have our app available on tor : `44plirk7dok2hsd7.onion`
