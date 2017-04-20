<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>FAQ - Hosting your ARPA zone in Azure DNS

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Can I host ARPA zones for my ISP-assigned IP blocks on Azure DNS?
Yes. Hosting the ARPA zones for your own IP ranges in Azure DNS is fully supported.

Simply [create the zone in Azure DNS](dns-getstarted-create-dnszone.md), then work with your ISP to [delegate the zone](dns-domain-delegation.md).  You can then manage the PTR records for each reverse lookup in the same way as other record types.

You can also [import an existing reverse lookup zone using the Azure CLI](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>How much does hosting my ARPA zone cost?
Hosting the ARPA zone for your ISP-assigned IP block in Azure DNS is charged at [standard Azure DNS rates](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Can I host ARPA zones for both IPv4 and IPv6 addresses in Azure DNS?
Yes.