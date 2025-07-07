# Vlastní hosting s Cloudflare tunely

Ok, asi všichni známe ten pocit, že se nám válí někde v šuplíku nějaké to RasPi nebo podobná věc. Tzn, není to uplná plečka, žádný dělo - ale prošlo si nějakým životním cyklem ve stylu: zkusím si udělat XY -> koupím RasPi -> ok, funguje, ale je to nějaký pomalý, tohle mi tam chybí -> Jé hele, vyšla nová verze RasPi -> koupím si nové Raspi s větší RAMkou (staré jde do šuplíku mezi kabely a elektrobordel).

A nebo jenom prostě hledáte nějaké další využití pro váš homelab / minipočítač. Stejně jako já.A tak sem se rozhodl si z něj udělat hosting na vlastní software.

To znamená, abych svůj SW měl k dispozici veřejně a dosažitelný odkudoliv na světě. Prostě všide jinde než na localhostu nebo domácí LAN.

## Není jednodušší si zaplatit cloudovou VPS?

Jakože je, ale nechci do toho rvát moc peněz - raspičko už mám, na energiích mě to taky nesežere a stejně to pravidelně updatuju, tak mi nedává smysl kvůli pár appkám platit cloud.

## Ok, takže VPNka?

Zadrž! VPNka je rozhodně možnost. Musel bych si nastavit router, aby buď handloval VPNserver nebo na něm tunelovat port na raspi, kde to ohandluju.
Pak je tu záležitost security, s principu se přes VPNku dostanu do celé LAN sítě, pokud neudělám nějakou hardcore síťařskou magii s VLANy. Navíc budu muset na svoje zařízení instalovat VPN klienty. Meh. Ale takový Wireguard mě taky napadl, to zas ne, že ne.

## Tak jak s tím Cloudflare?

První věc (kterou jsem už udělal z jiného důvodu dříve) bylo přenesení DNS serveru a domény z W(V)edosu (kde mám regnutou doménu) ke Cloudflare. To je celkem easy (dáte add domain a vyklikáte si to) a teď si můžu přidávat další subdomény.

### Krok 1: Rozjedu si cloudflare tunel

V Zero Trust v Cloudflare dashboardu si vykliknu novej tunel, tam mi to vygeneruje nějaký registrační token.

Na Raspi si stáhnu cloudflared binárku a povolím ji jako systemd service, aby mi běžela na pozadí jako démon. Příkazem z nastavení tunelu si tunel zaregistruju.

Tak a teď mám tunel, který mi zajišťuje provoz mezi Raspi a Cloudflare infrastrukturou.

### Krok 2: Přidání aplikace do Cloudflare

V Zero Trust / Access / Applications si přidám novou Application. Kliknu na Self-hosted a jdu nastavovat.

Jasně, nejdřív si ji pojmenuju. Pak dám `+ Add public hostname` - tam si vytvořím subdoménu pro mou cloudflarem spravovanou doménu. Tady to bude celé dostupné. Takže něco jako `dev.mydomain.com` nebo si vymyslete něco originálního.

Následuje super výhoda Cloudflare - zabezpečení. Aby vám tam nelezl jen tak někdo, určitě doporučuju nastavit autentizaci. Je tu klasickej OAuth, takže si tam můžete v _Manage login methods_ napojit přihlašování přes Google, Github, atd. Každá metoda se nastavuje po svém, ale máte tam návod, co kde máte vyklikat a nastavit. Pak si nově vytvořenou login metodu jen vyberete a pokud je jediná vybraná, tak můžete zaškrtnout ať se přeskočí obrazovka s výběrem metody při loginu (klidně můžete mít k dispozici víc možností loginu pro jednu aplikaci).

Nezapomeňte ještě v _Access Policies_ nastavit policy, koho to má vlastně na onu aplikaci pustit. Já tam dal svůj email, takže OAuth login token s mým emailem to autorizuje a pustí dál k aplikaci. Jiný email to zahodí.

Můžete si pohrát i s dalším nastavením, ale tohle minimum by mělo stačit pro bezpečný přístup. Už jsme skoro tam.

### Krok 3: Spojit to dohromady

Vlezeme zase do Networks / Tunnels a vybereme náš tunel a dáme _"Edit"_.
Nahoře je záložka _"Public hostnames"_. Kliknete na modrý button pro přidání dalšího public hostname. Tam už jen nastavíte párování vaší aplikace pod danou subdoménou (ono `dev.mydomain.com`) a adresa z pohledu druhého konce tunelu (na raspi). Takže třeba `http://localhost:8080` nebo jiné.

Tady poznámka, která vám ušetří čas a nervy - **vybodněte se na HTTPS na straně Raspi, Cloudflare se o certifikát postará.** Zkoušel jsem si rozjet Caddyho s HTTPS a byl to naprosto zbytečný PAIN. V tom smyslu, že se mi certifikáty hádaly mezi sebou, musel jsem přidávat vyjímky do konfigurace Caddyho, atd, atp.

## Hotovo?

Jo, takhle mi to funguje. Mám dvě subdomény na jednom tunelu, odlišené portem, na jedné mi jede Home Assistant na druhé reverzní proxy pro moje aplikace. Rozjeté v Raspi je to přes Docker s bridgovanou sítí.

## Co to koštuje?

Celý tohle workflow mám zdarma. Jasně, je celkem pravděpodobný, že když bych těch domén měl hodně nebo generoval velký traffic, asi by to nějakou zlatku už stálo, ale tam asi nikdy nedosáhnu. Navíc ani nepotřebuju žádné super detailní dashboardy ohledně provozu, kterých je tam taky pár pod paywallem.

Jediný co vás to stojí, je fakt - že je to vlastně vendor-lock. Využíváte služby a infrastrukturu třetí strany a když si usmyslí zítra zaříznout počet tunelů nebo aplikací pro neplatící, nic s tím neuděláte.

Na druhou stranu - je to asi nejjednodušší a nejbezpečnější způsob jak si tenhle vlastní server rozjet. Hele a neříkám, že je to nejlepší věc na světě, jen že poměr "cena/čas/výkon" je parádní a pokud budete mít větší nároky, tak až potom se teprve kouknout po něčem jiném.

VK,6.7.2025
