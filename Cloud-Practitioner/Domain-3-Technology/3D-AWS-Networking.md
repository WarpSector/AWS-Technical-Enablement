# Domain 3: Technology
# (3D: Amazon Web Services Networking Services)

## Amazon Route 53
  * ### Amazon Route 53** is the AWS Domain Name Service (DNS).
    * Recall that websites are "read" by computers as IP addresses.
    * When you type in a website address into the browser, your computer sends a query to the DNS Server.
    * The DNS server has a "zone file" with a list of IP addresses.
    * It finds the IP address you queried from its "zone file" and returns it to your computer (if the first DNS server can't find the IP, it forwards the request to the next DNS server until the query is answered).
    * Once your computer receives the destination IP address, it performs an HTTP GET protocol over the internet to connect to the destination website.
  * ### Fully Qualified Domain Names (FQDNs)
    * When we're using DNS, we're actually using something called a FQDN.
    * **Example:** www.example.com <-- This is a FQDN.
      * There's usually a "." after the ".com" - so it's ".com.", but we don't use a dot after we type in the web address. This dot "." after the ".com" is the **root domain**.
      * The ".com" is the **top-level domain**.
      * The ".example" is the **subdomain**.
      * The "www" is the **hostname**.
