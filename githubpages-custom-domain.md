## Publishing GitHub Pages to a custom (.ca) domain

Assume that you bought the domain acanadiandomain.ca from canspace.ca or from any registered domain registrar at CIRA
The instructions below are to setup [both the apex and subdomain](https://help.github.com/articles/setting-up-an-apex-domain-and-www-subdomain/)

### At GitHub 
1. Follow the instructions [to setup a www subdomain](https://help.github.com/articles/setting-up-a-www-subdomain/)
2. Ensure that you have [added ac custom domain to your GitHub Pages site](https://help.github.com/articles/adding-or-removing-a-custom-domain-for-your-github-pages-site/)

### At Canspace
1. Once you have bought acanadiandomain.ca domain from Canspace log in and follow the instructions [to manage your DNS entries](https://www.canspace.ca/clients/knowledgebase/32/How-do-I-manage-DNS-entries.html)
2. Specifically create a zone which in this case would be acanadiandomain.ca
3. Then edit the zone to add records. There may be some default records already setup
4. You will want to remove the CNAME entry for www.acanadiandomain.ca aliasing to acanadiandomain.ca
5. For a preliminary setup let's setup the www subdomain only
6. Add a CNAME entry for www.acanadiandomain.ca to YOUR-GITHUB-USERNAME.github.io 
7. Add an A entry for acanadiandomain.ca to the IP addresses listed [here](https://help.github.com/articles/setting-up-an-apex-domain/#configuring-a-records-with-your-dns-provider) 
8. Remove the A entry for the default ipadress provided by canspace.ca because you do not need their domain forwarding
9. You may want to check if your DNS provider supports ANAME or ALIAS. Could not find entries for that while creating DNS entry records in canspace.ca so I assume that it does not.
10. This is how the final setup may look like 
![canspace-manage-dns](https://anshumania.github.io/codenotes/images/githubpages-canspace-manage-dns.png)

```shell
# check if CNAME entries are correct
$> dig www.acanadiandomain.ca +nostats +nocomments +nocmd
;www.acanadiandomain.ca.		 IN	A
www.acanadiandomain.ca.	   14400 IN	CNAME	YOUR-USERNAME.github.io.
YOUR-USERNAME.github.io..  672	 IN	CNAME	sni.github.map.fastly.net.
sni.github.map.fastly.net. 26	 IN	A		151.101.XX.XXX

# the above may take a while
# you can check dns propagation on the web at 
# https://dnschecker.org or https://www.whatsmydns.net
# you can also query a particular nameserver for example google to check propagation
$> dig @8.8.8.8 www.acanadiandomain.ca +nostats +nocomments +nocmd

# check if A entries are correct
$> dig +noall +answer acanadiandomain.ca
acanadiandomain.ca.		11901	IN	A	192.30.252.154
acanadiandomain.ca.		11901	IN	A	192.30.252.153
```

### TroubleShooting
1. Check at the minimal if your own dns has the right settings propagated
2. Shift+f5 to force a refresh in your browser. 
3. You may need to close your browser or clear your cached depending on your setup.
4. Try incognito/private modes in your browser
5. Add the http cache expires header to your html page
6. If the CNAME and A entries are correct then github will [automatically set up the redirects appropriately](https://help.github.com/articles/setting-up-an-apex-domain-and-www-subdomain/)