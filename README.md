## Warszawska Wyższa Szkoła Informatyki
# Sytemy Operacyjnee
# Sprawozdanie 3
## **TEMAT** : _Instalacja i konfiguracja roli DNS w systemie Ubuntu Server_

#### Wykonanie S10147EP
#### Eryk Perchuć Z202
---
##### Cele ćwiczenia:
- Przeprowadzenie instalacji programu bind9 zgodnie z wytycznymi podanymi przez zleceniodawcę
- Przeprowadzenie wstępnej konfiguracji serwera
- Zapoznanie z plikami konfiguracyjnymi programu
- konstrukcja pliku /etc/netplan/*
- konstrukcja pliku /etc/db.*
- konstrukcja pliku /etc/bind9/named.conf
- konstrukcja pliku /etc/bind9/named.conf.options
- konstrukcja pliku /etc/bind9/named.conf.local
- Zapoznanie z narzędziami do testowania połączeń i pozyskiwania podstawowych informacji sieciowych: ping, ip a, nslookup
---
##### Podstawy teoretyczne:
1. Polecenia do diagnostyki sieciowej i ich atrybuty ip, nslookup, host, dig
2. Co to jest serwer DNS i zasada jego działania
3. Konstrukcja plików /etc/hosts i /etc/hostname/
4. Budowa plików /etc/bind9/named.conf.local i plików typu db.*
---
1. 
- IP
```sh
ip [OPTION] OBJECT {COMMAND | help}
where  OBJECT := { link | address | addrlabel | route | rule | neigh | ntable |
                   tunnel | tuntap | maddress | mroute | mrule | monitor | xfrm |
                   netns|l2tp|fou|macsec|tcp_metrics|token|netconf|ila|
                   vrf | sr | nexthop }
       OPTIONS := { -V[ersion] | -s[tatistics] | -d[etails] | -r[esolve] |
                    -h[uman-readable] | -iec | -j[son] | -p[retty] |
                    -f[amily] { inet | inet6 | mpls | bridge | link } |
                    -4 | -6 | -I | -D | -M | -B | -0 |
                    -l[oops] { maximum-addr-flush-attempts } | -br[ief] |
                    -o[neline] | -t[imestamp] | -ts[hort] | -b[atch] [filename] |
                    -rc[vbuf] [size] | -n[etns] name | -N[umeric] | -a[ll] |
                    -c[olor]}```
- nslookup
```sh
nslookup [-option] [name | -] [server]
nslookup -type=ns *****
nslookup -type=soa ****
nslookup - query=mx ****
nslookup -type=any ****
nslookup -type=ptr ****
nslookup -timeout=* ****
nslookup -debug ****
```
 - host
```sh
host [-aCdilrTvVw] [-c class] [-N ndots] [-t type] [-W time]
            [-R number] [-m flag] hostname [server]
       -a is equivalent to -v -t ANY
       -A is like -a but omits RRSIG, NSEC, NSEC3
       -c specifies query class for non-IN data
       -C compares SOA records on authoritative nameservers
       -d is equivalent to -v
       -l lists all hosts in a domain, using AXFR
       -m set memory debugging flag (trace|record|usage)
       -N changes the number of dots allowed before root lookup is done
       -r disables recursive processing
       -R specifies number of retries for UDP packets
       -s a SERVFAIL response should stop query
       -t specifies the query type
       -T enables TCP/IP mode
       -U enables UDP mode
       -v enables verbose output
       -V print version number and exit
       -w specifies to wait forever for a reply
       -W specifies how long to wait for a reply
       -4 use IPv4 query transport only
       -6 use IPv6 query transport only
```
- dig
```sh
Dig **** +noall & +answer
Dig –t mx ****
Dig -t dx ****
Dig -t any ***
Dig -x ****
```
2. 
- DNS (Domain Name System) to protokół, którego główna funkcja polega na tłumaczeniu łatwych do zapamiętania przez człowieka nazw domen na zrozumiałe dla komputerów dane liczbowe. Serwer DNS wyszukuje adres IP danej strony na podstawie wpisu użytkownika zamieszczonego w polu adresu wyszukiwarki.
Jak działa serwer DNS?
Działanie systemu DNS przebiega zgodnie z następującymi etapami:
- Użytkownik wpisuje w polu adresu przeglądarki określoną nazwę domeny, np. http://site.eryk.cf/nsfw/
- System za pośrednictwem przeglądarki nawiązuje komunikację z lokalnym serwerem DNS, przesyłając prośbę o przetłumaczenie nazwy domeny na adres IP.
- Lokalny serwer przesyła zapytanie o numer IP do jednego z 13 głównych serwerów na świecie (roor-servers).
- Serwer główny przekazuje informację na temat lokalizacji (adresu IP) serwera, na którym przechowywane są strony internetowe z określoną końcówką domeny – w tym przypadku .pl.
- Serwer DNS dostarcza w informacji zwrotnej do komputera użytkownika (do przeglądarki stron internetowych) numer odpowiadający konkretnej domenie.
- Komputer nawiązuje połączenie z adresem IP, co umożliwia wyświetlenie zawartości strony www. Wszystko dzieje się w bardzo krótkim czasie, dlatego działanie serwera DNS jest dla nas niezauważalne.

3. 
