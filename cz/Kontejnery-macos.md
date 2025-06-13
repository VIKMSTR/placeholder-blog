# Apple Container - nativní linuxové kontejnery na Macu?

Když chcete pracovat s OCI kontejnery na Macu, máte několik možností, po jakých nástrojích sáhnout.
Většina lidí, kteří chtějí pohodlný způsob práce s kontejnery a jsou ok s licencí sáhne po tradičním _Docker Desktopu_. Ti, kteří by chtěli třeba i podporu kubernetes, jdou do _Rancher Desktopu_ (který patří mezi můj oblíbený). (Nejen) fanoušci Redhatu jsou spokojení (i přes občasnou nekompatibilitu s Dockerem) s _Podman Desktopem_ a ti, kteří chtějí něco lehčího a jsou naprosto v pohodě s CLI prostředím použijí tooling jako docker client + _(co)lima_.

## Co se to děje?

Do toho teď tak trochu vstupuje Apple se svým Container nástrojem a libkou. A bacha - vím, že to zní u Applu divně, ale nejspíš to tak opravdu je - dali obojí s opensource licencí na Github - najdete je [tady](https://github.com/apple/container) a [tady](https://github.com/apple/containerization). Jableční fanoušci jsou nadšeni a mluví o "konečně nativní podpoře Docker/OCI kontejnerů na Macu". Ok, ale jaká je skutečnost? A opravdu peklo zamrzlo a jablko si pustilo tučňáka do systému? A čo si tak vojín Kefalín predstavujetě pod pojmom "nativní"?

Tak se zase uklidníme, napijem se vody a ujasníme si jednu věc - nativní jakože opravdu NATIVNÍ bez nějaké virtualizační vrstvy - OCI kontejnery na Macu nebudou nikdy. Deal with it.
A proč? Protože tyhle kontejnery jsou "linux thing". Tzn, že mají podporu přímo v linuxovém kernelu (kterej btw kontejnery mezi sebou sdílí), který zvládá věci jako resource limiting pomocí cgroups apod. V Mac jádru nic takového není.


>Drobná odbočka - principiálně by Mac vzhledem ke svému BSD-like původu měl spíš blíže k jiné technologii kontejnerizace - a to sice k "jails". To je něco trochu podobného v BSD světě - akorát je izolace mezi "žaláři" mnohem větší, jsou tak mnohem bezpečnější, ale zase nejsou tak flexibilní z hlediska deploymentu. Jo a především nejsou s OCI kontejnery kompatibilní. Prostě jinej svět. Neptejte se mě ně, nikdy jsem je nepotkal.

## Ale mě kontejnery fungujou - akorát teda ne nativně?

Takže způsob jakým tyhle "<YOURFAVORITEBRAND> Desktop" fungují je v podstatě ten, že vám vytvoří na stroji nějakou linuxovou virtuálku (ve Windows za pomocí WSL2 použijí tu z WSL), která má v sobě docker/podman a v které pouští ony kontejnery. Proto máte v nastavení Deskop appky ty šoupátka na počet jader a velikost paměti - tím přiřazujete resources právě té virtuálce :) Stejně jakoby jste vytvářeli virtuálku ve Virtualboxu. Colima to dělá stejně, akorát jen v CLI - nastartuje virtuální stroj, na který se už klasickým docker clientem, který běží u vás na hostujícím systému, připojíte.

No a teď přišel Apple s tím, že to možná není tak uplně bezpečný a že to moc žere a kdesi cosi. A dropnul právě "Containerization framework" a Container CLI (velmi originální pojmenování) na WWDC 2025. A co to reálně dělá a znamená? No, vlastně zase vytváří virtuálku, akorát to dělá přes nějaký virtualizační framework ve Swiftu. Tady je asi to zmatení se slovem "nativní" - prostě jak je to libka přímo od Apple, budeme se tvářit nativně. Ale zpátky, co je důležitější - tohle vytváří virtuální mašinu pro každý kontejner zvlášť. Nech to vstřebat - pro každý kontejner vytváří novou linuxovou virtuálku, ve které běží OCI kontejner.

Šílený, co?

## Jak to má bejt jako bezpečnější a především - jak to má míň žrát?

Apple se chlubí tím, že ty virtuálky jsou opravdu lightweight - že tam nejsou žádný zbytečný libky, žádný libc, používají musl (který známe z Alpine Linuxu) a že ten overhead by neměl být tak velký.
Co se ale musí nechat, je o něco vyšší bezpečnost. Krom toho, že ořezáním věcí které nepotřebujeme snížíme _attack vektor_ (nemůžeš mě napadnout přes libku, kterou nemám v systému), tak i ta izolace mezi kontejnery je najednou mnohem větší. Když třeba sdílíme soubory s kontejnerem (mountujeme), tak nejdřív předáme data do virtuálky (která je sdílená všemi kontejnery) a až pak se mountne přímo do toho filesystému kontejneru. Takže nějakej zlobivej kontejner by se mohl dostat k datům, které pro něj nejsou určený. Tady se filesharuje přímo pouze s virtuálkou přiřazenou ke kontejneru.


>Odbočka č.2. - pamatuji si tu mentální BOLEST, kdy jsem musel na prvním Dockeru for Windows nejdřív sdílet adresáře s Virtualboxem pro danou linux virtuálku a teprve poté mountovat do kontejnerů. A samozřejmě že můžeš jen mountovat z nasdílených adresářů, takže lidi to typicky odrbávali nasdílením celého D:/ disku, aby nemuseli řešit jestli zrovna daný adresář ve kterém pouštějí kontejner je accessible z VBoxu nebo ne. Ugh. Jsem rád, že tohle je pryč.

## Jak se s tím dělá?

Reálně se s tím pracuje tak, že na začátku pustíte `container system start`, to si sosne kernel, připraví si vytváření virtuálek, služby na networking a file sharing a rozjede si API. Pak už pomocí CLI fungujete jak jste zvyklí s dockerem: `container run`, `container exec`, `container logs`, `container ls`, ..
Je tu dokonce i builder, takže můžete dál normálně buildit svoje image, sice pouze z `Dockerfile` ale zato multiplatformě. Plus to umí samozřejmě přistupovat na chráněné docker registries ("container registry login", takže to budete moci i pushnout na váš privátní docker registry server. Btw - tady je hezký průvodce, jak na to.

Takže - vyplatí se přepnout na nový docker tooling od Applu? Pokud máte _Apple Silicon_ na svém stroji a současně i poslední verzi OS (Stable s nějakými omezeními stačí, nemusíte kvůli tomu stahovat betu Tahoe ), tak na takové to domácí zkoušení je to asi v pohodě. Pokud máte v dockeru celou sadu kontejnerů, které si mezi sebou musí povídat nebo nedejbože používáte Docker Compose - pak tohle pro vás zatím moc význam nemá a můžete čekat, jak se to vyvine.

Přece jenom, je to teprve verze 0.1.0 a ani Řím nezbuildovali za den.


VK, 11.6.2025
