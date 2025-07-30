# Jak jsem rozjížděl Vim a proč to sakra dělám

Vim, resp. Neovim jsou samozřejmě známé vlastně hlavně kvůli memu (nebo vtipu), že "je něco těžké jako vylézt z vimu". Hahaha, strašně vtipný, jsem si skoro jistej, že :q! znají i ti, kteří vim nepoužívají. Protože jednou za čas se vám prostě někde otevře, potřebujete upravit tu jednu pitomou možnost toho konfiguráku a pak se z něj chcete dostat ven. A tak zabijete celou session a otevřete si novej terminál.

## Co tě k tomu vede, chceš bejt cool hacker nebo co?

Kdo by nechtěl! Ale to není ten hlavní důvod. Hele na svou práci používám IntelliJ IDEA. Nedám na ni dopustit, je to skvělej tool, Community verze je good enough, Ultimátka stojí za všechny peníze. Je nabušená funkcionalitou a s dalšími pluginy je prostě komplexním supertoolem.

ALE! Protože už nevyvíjím primárně v Javě (mým primárním jazykem je Go), tak vlastně moc těch věcí k životu nepotřebuju. Maven/Gradle, Spring a všechny syntax helpery na config file a navigaci po anotacích, a určitě spousta dalších věcí - hážu do koše. Reálně tak, že už si ani nepamatuju, kde je najdu v jakým menu nebo jakou mají namapovanou hotkey.

Hlavní co chci je navigace v kódu - najdi mi kde se funkce volá, přejdi z volání funkce na její definici, udělej mi search nebo search a replace nějakého stringu v rámci projektu, pusť testy, pusť build, otevři terminál, možná udělej nějakou operaci s gitem. To je pro mě aktuálně to hlavní. Jo a syntax highlight a napovídání při psaní, ale to je tak nějak automatika.

Navíc, co jsem si zvykl dělat bylo otevřít si IDEA ve fullscreenu, v zen mode - jen kód s minimem meníček a věcí kolem. Ideálně dva / tři soubory vedle sebe. Ono to funguje, ale když sem zjistil že na přepnutí mezi těmito soubory je nějaká obskurní hotkey, která mi koliduje s nějakou systémovou v Macos, tak nějak jsem ztratil trpělivost.

## A nestálo by za to se tu IDEA naučit prostě pořádně používat?

Jako když si sečtu čas, kterej věnuju učení se vimu, tak to vlastně není uplně tak špatný nápad. Ale přiznám se, že jsem byl trochu zviklán tou vizí vůbec nepotřebovat myš při programování a dělat všechno přes klávesnici. Protože i tak se snažím co nejvíc úkonů dělat bez myši a tohle je další krok.

Současně ale ta vstupní bariéra do vimu je přece jenom docela velká. Uměl jsem základy ještě ze školy, ale na nějakou rozumnou práci to nestačilo. Navíc fakt netuším jak si to celý nastavit tak, aby mi to splňovalo moje požadavky, jaké pluginy použít a tak. Ricing konfigurace vimu je neřest, kterou nechci uplně disponovat, zas tolik času v tom utopit nechci.

A tak jsem sáhl po **Lazyvim**.

## Aha, takže jsi vyměnil jeden bloatware za druhý, chápu to správně?

Jo a ne. Nebo takhle - jasně je to dost ohnutý (neo)vim, který si toho s sebou nese fakt hodně - pluginy téměř na všechno co vás napadne, nerdtree sidebar, buffer taby, dokonce v sobě má integrovaný lazygit (což je btw parádní fičura). Ale současně natahuje ty pluginy až tehdy, když jsou potřeba. Máš otevřený frontend? Tak golang plugin necháme spát, tady máš ten na typescript!

A taky ta konfigurace, kde si reálně interaktivně povolíte pluginy, které chcete nainstalovat, ony se vám sosnou a jedete. Pecka.

## Nojo, jenže ta učicí křivka bude ještě strmější, ne? Když se musíš kromě základního vimu učit ještě lazyvim commandy a bindingy...

To je něco, kvůli čemu jsem tohle celé už jednou vzdal. Prodírat se kombinací dokumentace neovimu + lazygitu + jednotlivých pluginů byla nekonečná bolest. Jakože fakt, třeba `<leader> (což je mezerník) + t` mělo otevřít nabídku testů, jenže nic se nedělo. To, že si musíte nakonfit neotest s patřičnou integrací pro daný jazyk už je věc, kterou najdete na třech různých místech.

Jenže tady zrovna přichází na scénu AI a oblast, ve které je fakt dobrá a užitečná.

## AI ale strašně často kecá...

Jo, to je pravda. Taky mi předhazovala nějakou zkratku, která byla reálně jinak - jenže tady ji můžeš hned konfrontovat - prostě to zkusíš a když to nefunguje, tak ti to buď dohledá jinak nebo holt už do těch dokumentací asi budeš muset zajít. Ale zatím se mi to povedlo dycky vyřešit bez dlouhého hledání.

## Takže jak se s tím jako učíš pracovat?

Prostě jsem si vzal kód, a ptal se AI jak zrovna nějakej danej krok programování mám udělat v lazyvimu. Jak si otevřu soubor, jak se přepnu mezi okny a bufferem, jak pustím testy, jak najdu volání funkce. A ona mi ochotně odpovídala. Dokonce mi nabídla víc možností jak daného cíle dosáhnout a já si vybral ten, co mi sedí nejvíc.
Když to nefungovalo, zeptal jsem se jinak nebo to jinak opsal. Pokud vše fungovalo, zapsal jsem si to do **svého osobního cheat sheetu**.
A tak si stavím vlastní cheat sheet a tu AI potřebuju čím dál méně.

## Hmm, a je ti to k něčemu?

I s tím málem co zatím umím si připadám rychlejší v orientaci v kódu. Textový operace mi dělají furt trochu problém, stejně jako vzpomenout si, jestli se potřebuju přepnout mezi buffery `(shift + hjkl)` nebo okny `(ctrl + hjkl)` nebo dokonce panely v Ghostty `(command + hjkl)`.

Celý je to ale mnohem responzivnější než IDEA, která někdy prostě strašně dlouho chroustá a přemýšlí na pozadí. Tady je to všechno rychlý jak průjem a žere to desetinu RAMky. Spousta lidí určitě ocení tu integraci lazygitu. To je asi nejpohodlnější způsob jak používat git z klávesnice. Pravda, já to tolik neocením, protože na to co často používám už mám aliasy (třeba i na conventional commits), ale na nějaký ten _squash_ nebo _rebase_ to taky použiju.

## Takže budeš jeden z těch otravných lidí, co budou všude a všem doporučovat (neo)vim?

Ne, to fakt ne. Já to vidím tak, že to řeší nějaký problémy s komfortem psaní kódu, které mám já. Jako reálně si dokážu představit hodně důvodů proč do toho nejít (používání Windows, specifické gui tooly pro vývoj, možná nějakej problematickej support pro určité jazyky a platformy), ale já jsem celkem odhodlaný.

Uvidíme, jak dlouho mi to vydrží. :)

VK, 30.7.2025
