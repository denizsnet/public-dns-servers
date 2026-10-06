# Public DNS Servers

A small, verified list of **public DNS resolvers** with IPv4, IPv6, DNS-over-HTTPS (DoH) and DNS-over-TLS (DoT) addresses — as a Markdown table, [JSON](dns-servers.json) and [CSV](dns-servers.csv).

Every address was checked against the provider's own official documentation. The list is also published (in Turkish, with setup guides) at **[ipdns.com.tr — Güncel DNS adresleri](https://www.ipdns.com.tr/guncel-dns-adresleri)**.

_Last checked: October 2026_

## Addresses

| Provider | Variant | Filtering | IPv4 | IPv6 | DoH | DoT / Android Private DNS |
|---|---|---|---|---|---|---|
| Cloudflare | Standard | none | `1.1.1.1`<br>`1.0.0.1` | `2606:4700:4700::1111`<br>`2606:4700:4700::1001` | `https://cloudflare-dns.com/dns-query` | `one.one.one.one` |
| Cloudflare | Malware blocking | malware | `1.1.1.2`<br>`1.0.0.2` | `2606:4700:4700::1112`<br>`2606:4700:4700::1002` | `https://security.cloudflare-dns.com/dns-query` | `security.cloudflare-dns.com` |
| Cloudflare | Families (malware + adult) | malware, adult | `1.1.1.3`<br>`1.0.0.3` | `2606:4700:4700::1113`<br>`2606:4700:4700::1003` | `https://family.cloudflare-dns.com/dns-query` | `family.cloudflare-dns.com` |
| Google Public DNS | Standard | none | `8.8.8.8`<br>`8.8.4.4` | `2001:4860:4860::8888`<br>`2001:4860:4860::8844` | `https://dns.google/dns-query` | `dns.google` |
| Quad9 | Secure (recommended) | malware | `9.9.9.9`<br>`149.112.112.112` | `2620:fe::fe`<br>`2620:fe::9` | `https://dns.quad9.net/dns-query` | `dns.quad9.net` |
| Quad9 | Secure + ECS | malware | `9.9.9.11`<br>`149.112.112.11` | `2620:fe::11`<br>`2620:fe::fe:11` | `https://dns11.quad9.net/dns-query` | `dns11.quad9.net` |
| Quad9 | Unfiltered | none | `9.9.9.10`<br>`149.112.112.10` | `2620:fe::10`<br>`2620:fe::fe:10` | `https://dns10.quad9.net/dns-query` | `dns10.quad9.net` |
| AdGuard DNS | Default (ad blocking) | ads, trackers | `94.140.14.14`<br>`94.140.15.15` | `2a10:50c0::ad1:ff`<br>`2a10:50c0::ad2:ff` | `https://dns.adguard-dns.com/dns-query` | `dns.adguard-dns.com` |
| AdGuard DNS | Family protection | ads, trackers, adult, safe search | `94.140.14.15`<br>`94.140.15.16` | `2a10:50c0::bad1:ff`<br>`2a10:50c0::bad2:ff` | `https://family.adguard-dns.com/dns-query` | `family.adguard-dns.com` |
| AdGuard DNS | Unfiltered | none | `94.140.14.140`<br>`94.140.14.141` | `2a10:50c0::1:ff`<br>`2a10:50c0::2:ff` | `https://unfiltered.adguard-dns.com/dns-query` | `unfiltered.adguard-dns.com` |
| OpenDNS (Cisco) | Home | configurable with account | `208.67.222.222`<br>`208.67.220.220` | — | `https://dns.opendns.com/dns-query` | — |
| OpenDNS (Cisco) | FamilyShield | adult | `208.67.222.123`<br>`208.67.220.123` | — | `https://familyshield.opendns.com/dns-query` | — |
| CleanBrowsing | Security | malware | `185.228.168.9`<br>`185.228.169.9` | — | — | — |
| CleanBrowsing | Adult filter | malware, adult | `185.228.168.10`<br>`185.228.169.11` | — | — | `adult-filter-dns.cleanbrowsing.org` |
| CleanBrowsing | Family filter | malware, adult (stricter) | `185.228.168.168`<br>`185.228.169.168` | — | — | `family-filter-dns.cleanbrowsing.org` |
| Yandex DNS | Basic | none | `77.88.8.8`<br>`77.88.8.1` | — | — | — |
| Yandex DNS | Safe | malware | `77.88.8.88`<br>`77.88.8.2` | — | — | — |
| Yandex DNS | Family | malware, adult | `77.88.8.7`<br>`77.88.8.3` | — | — | — |
| Comodo Secure DNS | Standard | malware | `8.26.56.26`<br>`8.20.247.20` | — | — | — |

**Filtering** describes what the resolver blocks by default. "none" means the resolver returns answers without filtering.

**DoT** column: use this hostname in Android's *Private DNS* setting (Android 9+). Enter the hostname, not an IP address.

## Which one should I use?

- **Fast and unfiltered:** Cloudflare `1.1.1.1` or Google `8.8.8.8`.
- **Block malware and phishing:** Quad9 `9.9.9.9` or Cloudflare `1.1.1.2`.
- **Block adult content for a family network:** Cloudflare for Families `1.1.1.3`, AdGuard Family or OpenDNS FamilyShield. Set it on the router so every device is covered.
- **Block ads at DNS level:** AdGuard DNS `94.140.14.14`.

DNS filtering is a useful first layer, not a complete parental-control or security solution.

## Check a resolver from the command line

```bash
# Ask a specific resolver
dig @1.1.1.1 example.com A +short
nslookup example.com 9.9.9.9          # Windows

# Compare answers from several resolvers
for r in 1.1.1.1 8.8.8.8 9.9.9.9; do echo "$r: $(dig @$r example.com A +short | head -1)"; done
```

To compare many resolvers at once in the browser, use the [DNS Propagation Checker](https://ipdnshub.com/propagation). To see every record of a domain, use [DNS Record Lookup](https://ipdnshub.com/dnsrecord). More commands: [dns-network-cli-cheatsheet](https://github.com/denizsnet/dns-network-cli-cheatsheet).

## Using the data

```js
const res = await fetch("https://raw.githubusercontent.com/denizsnet/public-dns-servers/main/dns-servers.json");
const { servers } = await res.json();
console.log(servers.filter(s => s.filtering === "none").map(s => s.provider));
```

## Not included

**ISP resolvers** (for example Turkish ISPs) are not listed: most ISPs do not publish their resolver addresses officially, and the addresses shared on forums change over time. Your router receives the correct ISP resolver automatically.

## Contributing

Spotted an outdated address? Open an issue with a link to the provider's official documentation.

---

## Türkçe

Bu depo, herkese açık DNS sunucularının **doğrulanmış** IPv4, IPv6, DoH ve DoT adreslerini içerir. Tüm adresler sağlayıcıların resmi sayfalarından kontrol edilmiştir.

- Türkçe açıklamalar ve güncel liste: [Güncel DNS adresleri](https://www.ipdns.com.tr/guncel-dns-adresleri)
- Bilgisayar, telefon ve modemde DNS değiştirme: [DNS ayarları nasıl değiştirilir](https://www.ipdns.com.tr/dns-ayarlari-nasil-degistirilir-resimli-anlatim)
- Windows: [Windows 10 ve 11'de DNS sunucularını değiştirme](https://www.ipdns.com.tr/windows-10da-ve-windows-11de-dns-sunucularini-degistirme)
- Android: [Android'de Özel DNS ayarı](https://www.ipdns.com.tr/android-ozel-dns-ayari)
- Bir alan adının DNS kayıtlarını görmek için: [DNS sorgulama](https://www.ipdns.com.tr/dns-sorgulama)

## License

Data and documentation: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). You can reuse them, including commercially, with attribution and a link to this repository.
