<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>FAQ - Reverse DNS for your Azure-assigned IP address

### <a name="how-much-do-reverse-dns-records-cost"></a>How much do reverse DNS records cost?
They’re free!  There is no additional cost for reverse DNS records or queries.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Will the reverse DNS records for my Azure-assigned Public IP Address resolve from the internet?
Yes. Once you set the reverse DNS property for your Public IP Address, Azure manages all the DNS delegations and DNS zones required to ensure that reverse DNS record resolves for all internet users.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>Will a default reverse DNS record be created for my Public IP Addresses?
No. Reverse DNS is an opt-in feature. No default reverse DNS records are created if you choose not to configure them.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>What is the format for the fully-qualified domain name (FQDN)?
FQDNs are specified in forward order, and must be terminated by a dot (e.g., “app1.contoso.com.”).

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>What happens if the validation checks for the reverse DNS I’ve specified fail?
Where the validation for reverse DNS checks fail, the service management operation will fail. Please correct the reverse DNS value as required, and retry.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Can I manage reverse DNS for my Azure Website?
Reverse DNS is not supported for Azure Websites. Reverse DNS is supported for Azure Virtual Machines.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Can I configure multiple reverse DNS records for my Public IP Address?
No. Azure supports a single reverse DNS record for each Public IP Address. Each Public IP Address however can have their own reverse DNS record.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Can I configure reverse DNS records for an IPv6 Public IP Address?
No.  At this time, reverse DNS records are supported for IPv4 Public IP Addresses only.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Can I configure a reverse DNS record for my Public IP Address without having a DomainNameLabel specified?
No. To leverage reverse DNS records for your Public IP Addresses, you must specify the DomainNameLabel property.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Can I send emails to external domains from my Azure Compute services?
No. [Azure Compute services do not support sending emails to external domains](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
