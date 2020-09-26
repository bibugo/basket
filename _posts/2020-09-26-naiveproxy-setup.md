

# install golang
as [here](https://www.geefire.eu.org/2020/09/20/install-golang-in-debian-10.html)

1. get forwardproxy with naïve fork
> naïve standalone version [here - https://github.com/klzgrad/naiveproxy](https://github.com/klzgrad/naiveproxy)
```bash
git clone -b naive https://github.com/klzgrad/forwardproxy
```

2. get caddy2 custom build tool `xcaddy`
```bash
go get -u github.com/caddyserver/xcaddy/cmd/xcaddy
```

3. compile caddy with module (with naive forwardproxy + dns cloudflare)
```bash
~/go/bin/xcaddy build --with github.com/caddyserver/forwardproxy=$PWD/forwardproxy --with github.com/caddy-dns/cloudflare
```

4. caddy is located pwd
```bash
./caddy version
./caddy list-modules
which caddy
mv ./caddy /usr/bin/caddy
```

# * pre build release download:
[here - https://github.com/bibugo/caddy2-with-cloudflare-naive/releases](https://github.com/bibugo/caddy2-with-cloudflare-naive/releases)
