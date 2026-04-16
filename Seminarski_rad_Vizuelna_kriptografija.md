# VIZUELNA KRIPTOGRAFIJA

---

**Seminarski rad**

**Predmet:** Kriptografija

**Student:** Kristina

**Datum:** April 2026.

---

## Sadržaj

1. [Uvod](#1-uvod)
2. [Teorijske osnove](#2-teorijske-osnove)
   - 2.1. [Osnove kriptografije i dijeljenje tajni](#21-osnove-kriptografije-i-dijeljenje-tajni)
   - 2.2. [Matematički model vizuelne kriptografije](#22-matematički-model-vizuelne-kriptografije)
   - 2.3. [Formalna definicija vizuelne kriptografije](#23-formalna-definicija-vizuelne-kriptografije)
3. [Pregled metoda vizuelne kriptografije](#3-pregled-metoda-vizuelne-kriptografije)
   - 3.1. [Osnovna (2, 2) shema](#31-osnovna-2-2-shema)
   - 3.2. [Proširene (k, n) sheme](#32-proširene-k-n-sheme)
   - 3.3. [Vizuelna kriptografija u boji](#33-vizuelna-kriptografija-u-boji)
   - 3.4. [Progresivna vizuelna kriptografija](#34-progresivna-vizuelna-kriptografija)
   - 3.5. [Vizuelna kriptografija bez ekspanzije piksela](#35-vizuelna-kriptografija-bez-ekspanzije-piksela)
4. [Primjene vizuelne kriptografije](#4-primjene-vizuelne-kriptografije)
   - 4.1. [Autentifikacija i verifikacija identiteta](#41-autentifikacija-i-verifikacija-identiteta)
   - 4.2. [Zaštita medicinskih podataka](#42-zaštita-medicinskih-podataka)
   - 4.3. [Vizualni potpisi i vodeni žigovi](#43-vizualni-potpisi-i-vodeni-žigovi)
   - 4.4. [Sigurna komunikacija i bankarstvo](#44-sigurna-komunikacija-i-bankarstvo)
5. [Sigurnosni izazovi i ograničenja](#5-sigurnosni-izazovi-i-ograničenja)
   - 5.1. [Gubitak kontrastnosti i kvaliteta slike](#51-gubitak-kontrastnosti-i-kvaliteta-slike)
   - 5.2. [Napadi na vizuelnu kriptografiju](#52-napadi-na-vizuelnu-kriptografiju)
   - 5.3. [Ograničenja u praktičnoj primjeni](#53-ograničenja-u-praktičnoj-primjeni)
6. [Pregled literature i aktuelna istraživanja](#6-pregled-literature-i-aktuelna-istraživanja)
   - 6.1. [Hronološki pregled ključnih radova](#61-hronološki-pregled-ključnih-radova)
   - 6.2. [Noviji pravci istraživanja](#62-noviji-pravci-istraživanja)
   - 6.3. [Tabelarni pregled najznačajnijih doprinosa](#63-tabelarni-pregled-najznačajnijih-doprinosa)
7. [Zaključak](#7-zaključak)
8. [Literatura](#8-literatura)

---

## 1. Uvod

U savremenom digitalnom okruženju, zaštita osjetljivih informacija predstavlja jedan od ključnih izazova u oblasti informacione sigurnosti. Klasični kriptografski algoritmi, poput AES-a ili RSA, oslanjaju se na složene matematičke operacije i zahtijevaju odgovarajuću računarsku podršku kako za šifrovanje tako i za dešifrovanje podataka. Međutim, postoje scenariji u kojima je potrebno obezbijediti tajnost vizuelnog sadržaja — slika, dokumenata, dijagrama — na način koji ne zahtijeva nikakvu računarsku obradu prilikom otkrivanja skrivene poruke. Upravo u tom kontekstu nastaje pojam vizuelne kriptografije.

Vizuelna kriptografija (eng. *Visual Cryptography*, skraćeno VC) predstavlja posebnu granu kriptografije koja se bavi dijeljenjem tajne slike na dva ili više djelova, tzv. dijeljenih slika (eng. *shares*), od kojih svaka pojedinačno ne otkriva apsolutno ništa o originalnom sadržaju. Ključna karakteristika ovog pristupa jeste u tome što se rekonstrukcija tajne slike vrši isključivo vizuelnim putem — fizičkim preklapanjem prozirnih folija ili jednostavnim superponiranjem digitalnih slojeva — bez ikakve potrebe za kriptografskim izračunavanjima [1]. Ljudski vizuelni sistem služi kao jedini „dekriptor", što vizuelnu kriptografiju čini jedinstvenom među kriptografskim tehnikama.

Motivacija za istraživanje vizuelne kriptografije proizilazi iz nekoliko praktičnih potreba. Prije svega, u situacijama gdje korisnici nemaju pristup računarskim resursima ili ne posjeduju tehničko znanje za upotrebu konvencionalnih kriptografskih alata, vizuelna kriptografija nudi intuitivno i jednostavno rješenje. Osim toga, eliminacija potrebe za računarskom obradom prilikom dekriptovanja značajno smanjuje površinu napada — ne postoji softver koji se može kompromitovati, nema ključeva koji se mogu ukrasti iz memorije, niti postoji mogućnost napada bočnim kanalima tokom procesa dešifrovanja. Ove osobine čine vizuelnu kriptografiju naročito atraktivnom za primjene poput autentifikacije dokumenata, zaštite biometrijskih podataka i sigurnog prenosa vizuelnih informacija u okruženjima sa ograničenim resursima [18].

Koncept vizuelne kriptografije prvi put su formalno predstavili Moni Naor i Adi Shamir 1994. godine na konferenciji EUROCRYPT, u radu koji je objavljen u okviru serije *Lecture Notes in Computer Science* [1]. U tom radu, autori su opisali elegantnu shemu u kojoj se crno-bijela slika dijeli na dvije dijeljene slike — svaka od njih nalikuje slučajnom šumu i ne nosi nikakvu vidljivu informaciju. Tek kada se obje slike preklope na prozirnim folijama, originalna poruka postaje vidljiva golim okom. Ovaj rad se smatra temeljem čitave oblasti i inspirisao je stotine daljih istraživanja.

Prije Naora i Shamira, ideja o dijeljenju tajni već je postojala u kriptografskoj literaturi. Shamir je 1979. godine predložio shemu dijeljenja tajni zasnovanu na polinomnoj interpolaciji [2], dok je Blakley istovremeno nezavisno predstavio geometrijski pristup istom problemu [3]. Ove sheme su omogućavale podjelu tajne numeričke vrijednosti na više dijelova, ali nisu bile primjenjive na vizuelne podatke bez prethodne digitalne obrade. Vizuelna kriptografija je donijela suštinski nov pristup — umjesto matematičke rekonstrukcije, tajna se otkriva putem optičkog preklapanja, što je učinilo čitav postupak pristupačnim i korisnicima bez tehničkog predznanja.

Nakon originalnog rada, oblast se naglo razvijala. Ateniese, Blundo, De Santis i Stinson su 1996. godine proširili model na opšte pristupne strukture [4], dok su Naor i Shamir u nastavku svog istraživanja predložili metode za poboljšanje kontrasta rekonstruisane slike [5]. Tokom narednih decenija, istraživači su razvili brojne varijante — sheme za slike u boji [7], [8], halftone vizuelnu kriptografiju [9], progresivne sheme [11], kao i metode zasnovane na slučajnim mrežama (eng. *random grids*) koje eliminišu problem ekspanzije piksela [10], [13]. Danas vizuelna kriptografija predstavlja aktivno polje istraživanja sa primjenama u biometriji, medicinskoj informatici, digitalnim vodenim žigovima i sigurnosnim sistemima.

Cilj ovog seminarskog rada jeste da pruži sistematičan pregled oblasti vizuelne kriptografije. Rad je strukturiran na sljedeći način: u drugom poglavlju izložene su teorijske osnove, uključujući matematički model i formalnu definiciju. Treće poglavlje daje detaljan pregled postojećih metoda — od osnovne (2, 2) sheme do naprednih varijanti za slike u boji i shema bez ekspanzije piksela. Četvrto poglavlje razmatra praktične primjene, dok su u petom poglavlju analizirani sigurnosni izazovi i ograničenja. Šesto poglavlje nudi pregled relevantne literature i aktuelnih istraživačkih pravaca, a zaključak sumira najvažnije nalaze i daje smjernice za buduća istraživanja.

---

## 2. Teorijske osnove

Da bi se razumjeli principi na kojima počiva vizuelna kriptografija, neophodno je najprije sagledati širi kontekst kriptografskih tehnika za dijeljenje tajni, a zatim upoznati se sa specifičnim matematičkim aparatom koji se koristi u vizuelnim kriptografskim shemama.

### 2.1. Osnove kriptografije i dijeljenje tajni

Kriptografija, u najširem smislu, podrazumijeva skup tehnika za zaštitu informacija od neovlaštenog pristupa. Tradicionalni kriptografski sistemi funkcionišu po principu transformacije otvorenog teksta (eng. *plaintext*) u šifrovani oblik (eng. *ciphertext*) pomoću algoritma i tajnog ključa. Primatelj koji posjeduje odgovarajući ključ može izvršiti inverznu transformaciju i rekonstruisati originalnu poruku. Ovakvi sistemi — bilo simetrični poput AES-a, bilo asimetrični poput RSA — oslanjaju se na pretpostavku da protivnik ne raspolaže dovoljnim računarskim resursima da izvrši napade grubom silom ili iskoristi strukturalne slabosti algoritma.

Posebnu kategoriju unutar kriptografije čine sheme dijeljenja tajni (eng. *secret sharing schemes*). Osnovni problem koji ove sheme rješavaju može se formulisati na sljedeći način: kako podijeliti tajnu informaciju *S* na *n* dijelova i distribuirati ih različitim učesnicima, tako da samo određeni podskupovi učesnika mogu rekonstruisati tajnu, dok bilo koji manji podskup ne saznaje apsolutno ništa o njoj? Formalno, to je poznato kao *(k, n)* prag-shema, gdje je *k* minimalan broj dijelova potrebnih za rekonstrukciju.

Shamir je 1979. godine predložio elegantno rješenje zasnovano na Lagrangeovoj polinomnoj interpolaciji [2]. Ideja je jednostavna: tajna *S* se kodira kao slobodni član polinoma stepena *k − 1*, a svakom učesniku se dodjeljuje jedna tačka na tom polinomu. Bilo kojih *k* tačaka jednoznačno određuje polinom (i time tajnu), dok *k − 1* ili manje tačaka ne daje nikakvu informaciju. Nezavisno od Shamira, Blakley je istovremeno predložio geometrijski pristup u kojem se tajna predstavlja kao presjek *k* hiperravni u *k*-dimenzionalnom prostoru [3].

Obe navedene sheme su informaciono-teorijski sigurne, što znači da sigurnost ne zavisi od računarske moći protivnika. Međutim, one zahtijevaju aritmetičke operacije za rekonstrukciju tajne — učesnici moraju izvršiti interpolaciju ili riješiti sistem jednačina. Vizuelna kriptografija nastaje kao odgovor na pitanje: da li je moguće konstruisati shemu dijeljenja tajni za vizuelne podatke u kojoj rekonstrukcija ne zahtijeva nikakvu računarsku obradu? Naor i Shamir su pokazali da je odgovor potvrdan, čime su otvorili potpuno novi pravac istraživanja [1].

### 2.2. Matematički model vizuelne kriptografije

Matematički model vizuelne kriptografije zasniva se na konceptu dijeljenja svakog piksela tajne slike na kolekciju podpiksela, koji se zatim raspoređuju među dijeljene slike prema strogo definisanim pravilima. Razmotrimo najprije opštu *(k, n)* shemu: tajna slika se dijeli na *n* dijeljenih slika, a za rekonstrukciju je potrebno bilo kojih *k* od tih *n* slika.

Svaki piksel originalne slike, bio on crn ili bijel, zamjenjuje se blokom od *m* podpiksela u svakoj dijeljenoj slici. Vrijednost *m* naziva se ekspanzija piksela (eng. *pixel expansion*) i predstavlja faktor povećanja dimenzija rezultujućih slika. Na primjer, ako je *m* = 4, svaki piksel originala zamjenjuje se blokom 2 × 2 podpiksela u svakoj dijeljenoj slici. Ovo neizbježno dovodi do povećanja ukupne veličine slika, što predstavlja jedan od glavnih nedostataka osnovnih shema vizuelne kriptografije.

Proces kodiranja se opisuje pomoću kolekcija Boolovih matrica dimenzija *n × m*. Za bijeli piksel tajne slike, nasumično se bira jedna matrica iz kolekcije *C*₀, a za crni piksel iz kolekcije *C*₁. Red *i* odabrane matrice određuje raspored podpiksela za *i*-tu dijeljenu sliku. Konkretno, ako je element matrice na poziciji (*i*, *j*) jednak 1, odgovarajući podpiksel u *i*-toj dijeljenoj slici je crn; ako je 0, podpiksel je bijel (proziran).

Kontrast rekonstruisane slike definiše se preko Hammingove težine redova matrice. Neka *H(v)* označava Hammingovu težinu vektora *v*, odnosno broj jedinica u njemu. Kada se *k* dijeljenih slika preklopi, za svaki blok podpiksela rezultujući vektor dobija se OR operacijom nad odgovarajućim redovima matrice. Ako originalni piksel bijel, Hammingova težina rezultujućeg vektora ne smije preći prag *d* — što znači da preklopljeni blok izgleda pretežno svijetlo. Ako je piksel crn, Hammingova težina mora biti najmanje *d + α·m*, gdje je *α* parametar kontrasta (*α* > 0), tako da preklopljeni blok izgleda tamno [1], [4].

Relativni kontrast *α* određuje koliko će jasno tajna slika biti vidljiva nakon preklapanja. Veći kontrast znači bolju čitljivost, ali ga je teže postići bez povećanja ekspanzije piksela *m*. Ovo je fundamentalni kompromis u dizajnu vizuelnih kriptografskih shema — postoji inverzna zavisnost između kvaliteta rekonstruisane slike i veličine dijeljenih slika, što je formalno analizirano u više radova [5], [20].

### 2.3. Formalna definicija vizuelne kriptografije

Na osnovu prethodno opisanog modela, moguće je dati formalnu definiciju vizuelne kriptografske sheme. Prema Naoru i Shamiru [1], *(k, n)* vizuelna kriptografska shema (VCS) je definisana sa dvije kolekcije *n × m* Boolovih matrica *C*₀ i *C*₁ koje zadovoljavaju sljedeća dva uvjeta:

**Uvjet kontrasta:** Za svaku matricu *S* iz *C*₀, OR operacija nad bilo kojih *k* redova daje vektor čija Hammingova težina ne prelazi *d*. Za svaku matricu *S* iz *C*₁, OR operacija nad bilo kojih *k* redova daje vektor čija Hammingova težina iznosi najmanje *d + α·m*. Parametar *α* > 0 predstavlja relativni kontrast sheme i garantuje da ljudsko oko može razlikovati bijele od crnih piksela u rekonstruisanoj slici.

**Uvjet sigurnosti:** Za svaki podskup od manje od *k* dijeljenih slika, kolekcije matrica dobijene restrikcijom na odgovarajuće redove su identične za *C*₀ i *C*₁. Ovo znači da bilo koji skup od *k − 1* ili manje učesnika ne može razlikovati da li je originalni piksel bio crn ili bijel — informacija je savršeno skrivena u informaciono-teorijskom smislu. Ne radi se o računarskoj sigurnosti koja zavisi od pretpostavki o složenosti, već o apsolutnoj sigurnosti koja važi bez obzira na raspoloživu računarsku snagu protivnika.

Ateniese, Blundo, De Santis i Stinson su proširili ovu definiciju na opšte pristupne strukture [4]. Umjesto jednostavnog praga *k*, oni su uveli pojam kvalifikovanih i zabranjenih skupova učesnika. Kvalifikovani skup (eng. *qualified set*) je bilo koji podskup učesnika koji može rekonstruisati tajnu, dok zabranjeni skup (eng. *forbidden set*) ne smije saznati ništa o tajni. Ova generalizacija je značajna jer omogućava modelovanje složenijih scenarija — na primjer, situacije u kojoj je potrebno da barem jedan predstavnik svake od tri organizacije bude prisutan kako bi se tajna otkrila.

Blundo, De Santis i Naor su dodatno proširili formalni okvir na slike u nijansama sive (eng. *grey-level images*) [24]. Umjesto binarnih piksela, svaki piksel može imati jednu od *g* nijansi, što zahtijeva modifikaciju uvjeta kontrasta tako da se razlikuju svi parovi nijansi, a ne samo crna i bijela. Ovaj rad je postavio osnovu za kasnija proširenja na pune slike u boji.

---

## 3. Pregled metoda vizuelne kriptografije

Na osnovu teorijskog okvira izloženog u prethodnom poglavlju, u nastavku se detaljnije razmatraju konkretne metode vizuelne kriptografije — počevši od najjednostavnije (2, 2) sheme, preko proširenih prag-shema, do naprednih varijanti za slike u boji i shema bez ekspanzije piksela.

### 3.1. Osnovna (2, 2) shema

Osnovna (2, 2) vizuelna kriptografska shema je najjednostavniji i najintuitivniji oblik vizuelne kriptografije. U ovoj shemi, tajna crno-bijela slika se dijeli na tačno dvije dijeljene slike (*Share 1* i *Share 2*), a rekonstrukcija je moguća samo kada se obje slike preklope.

Princip rada može se ilustrovati na sljedeći način. Svaki piksel originalne slike zamjenjuje se blokom od 2 × 2 podpiksela. Za bijeli piksel, obje dijeljene slike dobijaju identičan raspored podpiksela — nasumično odabran iz skupa mogućih konfiguracija u kojima su tačno dva od četiri podpiksela crna. Kada se takvi identični blokovi preklope, rezultat je blok u kojem su i dalje samo dva podpiksela crna, što ljudsko oko percipira kao sivu (svijetlu) površinu. Za crni piksel, dijeljene slike dobijaju komplementarne rasporede — jedan blok je „negativ" drugog. Kada se ovi komplementarni blokovi preklope OR operacijom, sva četiri podpiksela postaju crna, što oko percipira kao tamnu površinu [1].

Formalno, kolekcije matrica za (2, 2) shemu definišu se na sljedeći način. Matrice za bijeli piksel su oblika:

```
C₀: sve permutacije kolona matrice  [1 0 1 0]
                                      [1 0 1 0]
```

dok su matrice za crni piksel oblika:

```
C₁: sve permutacije kolona matrice  [1 0 1 0]
                                      [0 1 0 1]
```

U prvom slučaju, OR operacija nad dva reda daje vektor sa Hammingovom težinom 2. U drugom slučaju, OR daje vektor sa težinom 4 (svi podpikseli crni). Razlika u težinama omogućava vizuelnu distinkciju između bijelih i crnih piksela. Ekspanzija piksela iznosi *m* = 4, a relativni kontrast *α* = 1/2, jer je razlika u proporciji crnih podpiksela između bijelih i crnih piksela jednaka 2/4 = 1/2.

Važno je naglasiti da svaka pojedinačna dijeljena slika izgleda kao potpuno slučajan šum. Budući da se za svaki piksel nasumično bira permutacija kolona, svaki blok u dijeljenoj slici ima tačno dva crna i dva bijela podpiksela, bez obzira na to da li je originalni piksel bio crn ili bijel. To znači da posmatranjem samo jedne dijeljene slike nije moguće zaključiti apsolutno ništa o tajnoj slici — sigurnost je savršena u informaciono-teorijskom smislu.

### 3.2. Proširene (k, n) sheme

Generalizacija osnovne sheme na proizvoljne pragove *(k, n)* predstavlja značajno složeniji problem. U opštoj *(k, n)* shemi, tajna slika se dijeli na *n* dijeljenih slika, a za rekonstrukciju je potrebno bilo kojih *k* od tih *n* slika. Bilo koji podskup od *k − 1* ili manje slika ne smije otkriti nikakvu informaciju o tajnoj slici.

Naor i Shamir su u svom originalnom radu [1] dali konstrukcije za *(2, n)*, *(n, n)* i opšte *(k, n)* sheme. Za *(2, n)* shemu, ekspanzija piksela iznosi *m* = 2^(*n*−1), što znači da veličina dijeljenih slika eksponencijalno raste sa brojem učesnika. Za *(n, n)* shemu, ekspanzija je *m* = 2^(*n*−1), a kontrast opada sa porastom *n*. Ovo eksponencijalno ponašanje predstavlja ozbiljno ograničenje za praktičnu primjenu sa većim brojem učesnika.

Ateniese i saradnici [4] su pristupili problemu sistematičnije uvodeći pojam baznih matrica (eng. *basis matrices*). Bazne matrice *S*⁰ i *S*¹ dimenzija *n × m* služe kao osnova iz koje se generišu kolekcije *C*₀ i *C*₁ permutacijom kolona. Dizajn baznih matrica svodi se na kombinatorni optimizacioni problem — cilj je minimizovati ekspanziju piksela *m* uz istovremeno maksimizovanje kontrasta *α*, poštujući uvjete sigurnosti i kontrastnosti za sve kvalifikovane i zabranjene podskupove učesnika.

Liu, Wu i Lin su 2010. godine predložili tzv. koračnu konstrukciju (eng. *step construction*) koja sistematizuje proces dizajna baznih matrica [21]. Njihov pristup omogućava postepenu izgradnju matrica za složenije pristupne strukture polazeći od jednostavnijih shema. Ovim je postignuto značajno smanjenje ekspanzije piksela u poređenju sa ranijim konstrukcijama za određene klase pristupnih struktura.

Treba napomenuti da za neke pristupne strukture optimalna ekspanzija piksela nije poznata — problem pronalaženja donje granice za *m* ostaje otvoren za mnoge konfiguracije. Ovo pitanje je tesno povezano sa kombinatorikom i teorijom dizajna, i predstavlja jedan od aktivnih istraživačkih problema u oblasti vizuelne kriptografije.

### 3.3. Vizuelna kriptografija u boji

Originalna shema Naora i Shamira bila je ograničena na crno-bijele slike, što je značajno sužavalo njenu praktičnu upotrebljivost. Proširenje vizuelne kriptografije na slike u boji pokazalo se kao netrivijalan problem jer boje zahtijevaju značajno složenije kodiranje od binarnih piksela.

Yang i Laih su među prvima predložili shemu za slike u boji [7]. Njihov pristup se zasniva na dekompoziciji boje na komponente subtraktivnog modela CMY (cijan, magenta, žuta). Svaka komponenta boje tretira se zasebno kao crno-bijela slika i kodira standardnom vizuelnom kriptografskom shemom. Rekonstrukcija se vrši preklapanjem dijeljenih slika štampanih na prozirnim folijama u odgovarajućim bojama. Ključni problem ovog pristupa jeste značajan gubitak kvaliteta boja — rekonstruisana slika često ima zamutan i bljedunjav izgled jer se kontrast svake pojedinačne komponente smanjuje prilikom preklapanja.

Hou je 2003. godine predložio drugačiji pristup zasnovan na halftone tehnici i paleti od ograničenog broja boja [8]. Umjesto rada sa kontinualnim bojama, slika se najprije konvertuje u halftone verziju koristeći tehnike distribucije greške (eng. *error diffusion*) ili uređenog raspršivanja (eng. *ordered dithering*). Svaki piksel halftone slike može poprimiti samo jednu od unaprijed definisanih boja iz palete, čime se problem svodi na kodiranje konačnog broja diskretnih simbola. Ovaj pristup je dao bolje vizuelne rezultate od prethodnih metoda, ali uz veću ekspanziju piksela.

Zhou, Arce i Di Crescenzo su 2006. godine formalizovali pojam halftone vizuelne kriptografije [9]. Njihov ključni doprinos bio je integracija procesa halftone-ovanja i vizuelnog kriptografskog kodiranja u jedinstven postupak — umjesto da se slika najprije konvertuje u halftone pa zatim kodira, oba procesa se obavljaju simultano. Rezultat su dijeljene slike koje same po sebi izgledaju kao smislene halftone fotografije (a ne kao slučajni šum), dok njihovo preklapanje otkriva tajnu sliku. Ova osobina, poznata kao *meaningful shares*, dodatno poboljšava praktičnu upotrebljivost jer dijeljene slike ne privlače pažnju kao sumnjivi kriptografski artefakti.

Lin i Tsai su 2003. godine predložili metodu za slike u nijansama sive koja koristi tehnike raspršivanja (eng. *dithering*) kako bi se sivi pikseli konvertovali u crno-bijele obrasce koji se zatim mogu kodirati standardnim shemama [12]. Nakajima i Yamaguchi su pak radili na proširenju vizuelne kriptografije za prirodne slike u punoj boji, razmatrajući RGB model i odvojeno kodiranje svake komponente [19]. Cimato, De Prisco i De Santis su analizirali optimalne obojene prag-sheme i dali donje granice za ekspanziju piksela u zavisnosti od broja boja u paleti [20].

### 3.4. Progresivna vizuelna kriptografija

U klasičnim vizuelnim kriptografskim shemama, tajna slika je ili potpuno skrivena (ako je prisutno manje od *k* dijeljenih slika) ili potpuno vidljiva (kada se preklopi *k* ili više slika). Ne postoji nikakav prelazni stadijum — situacija je binarna. Progresivna vizuelna kriptografija uvodi ideju postepenog otkrivanja tajne slike, gdje se sa svakom dodatnom dijeljenom slikom kvalitet rekonstrukcije poboljšava.

Jin, Yan i Kankanhalli su 2005. godine predložili progresivnu shemu za slike u boji [11]. U njihovom pristupu, svaka nova dijeljena slika koja se doda u proces preklapanja postepeno povećava kontrast i jasnoću rekonstruisane slike. Sa dvije slike, tajna je jedva vidljiva; sa tri, konture postaju jasnije; sa četiri ili pet, slika postaje čitljiva. Ovo je korisno u scenarijima gdje postoji hijerarhija pristupa — učesnici sa više dijeljenih slika dobijaju detaljniju verziju tajne informacije.

Progresivni pristup ima praktične prednosti u distribuiranim okruženjima. Na primjer, u vojnoj komunikaciji, različiti nivoi komandne strukture mogu imati pristup različitom broju dijeljenih slika, čime se implicitno ostvaruje kontrola pristupa zasnovana na rangu. Slično, u medicinskim aplikacijama, opšti ljekar može vidjeti samo grubu konturu dijagnostičke slike, dok specijalista koji posjeduje više dijeljenih slika dobija punu rezoluciju.

Treba napomenuti da progresivne sheme zahtijevaju pažljivije projektovanje baznih matrica u poređenju sa klasičnim prag-shemama. Potrebno je obezbijediti da kontrast monotono raste sa brojem preklopljenih slika, što nameće dodatna ograničenja na strukturu matrica i po pravilu dovodi do veće ekspanzije piksela.

### 3.5. Vizuelna kriptografija bez ekspanzije piksela

Ekspanzija piksela je jedan od najčešće kritikovanih aspekata vizuelne kriptografije. U osnovnoj (2, 2) shemi, svaki piksel se zamjenjuje blokom od 2 × 2 podpiksela, čime se površina slike učetvorostručuje. Za složenije sheme, ekspanzija može biti znatno veća. Ovo ne samo da povećava zahtjeve za skladištenjem i prenosom, već i mijenja proporcije slike, što je nepoželjno u mnogim primjenama.

Ito, Kuwakado i Tanaka su 1999. godine predložili pristup koji zadržava originalnu veličinu slike [6]. Njihova metoda koristi probabilistički pristup — umjesto determinističkog mapiranja piksela u blokove podpiksela, svaki piksel dijeljene slike je jednostavno crn ili bijel, bez proširenja. Cijena ovog pristupa jeste smanjen kontrast i povećan nivo šuma u rekonstruisanoj slici, ali su dimenzije dijeljenih slika identične dimenzijama originala.

Značajan napredak u ovom pravcu ostvaren je metodama zasnovanim na slučajnim mrežama (eng. *random grids*). Kafri i Keren su još 1987. godine uveli koncept slučajnih mreža za preklapanje slika, ali je tek Shyu 2007. godine sistematski razvio vizuelnu kriptografiju zasnovanu na ovom pristupu [10]. U metodi slučajnih mreža, prva dijeljena slika se generiše kao potpuno slučajna binarna slika — svaki piksel je nezavisno crn ili bijel sa jednakom vjerovatnoćom. Druga dijeljena slika se zatim generiše tako da njeno preklapanje sa prvom reprodukuje tajnu sliku. Za bijeli piksel tajne slike, odgovarajući piksel druge dijeljene slike je identičan pikselu prve; za crni piksel, odgovarajući piksel je komplementaran.

Chen i Tsao su 2011. godine proširili metodu slučajnih mreža na opšte *(k, n)* prag-sheme [13]. Njihov pristup omogućava dijeljenje tajne slike na *n* slika iste veličine kao original, uz mogućnost rekonstrukcije sa bilo kojih *k* slika. Tuyls i saradnici su pak razvili pristup zasnovan na XOR operaciji umjesto OR operacije [23]. XOR-bazirana vizuelna kriptografija postiže savršen kontrast (rekonstruisani bijeli pikseli su potpuno bijeli), ali zahtijeva elektronsku opremu za izvođenje XOR operacije — folije na svjetlu prirodno realizuju OR, a ne XOR. De Prisco i De Santis su 2014. godine detaljno analizirali odnos između pristupa zasnovanog na slučajnim mrežama i determinističke vizuelne kriptografije, pokazujući formalne veze između ova dva okvira [16].

---

## 4. Primjene vizuelne kriptografije

Razvoj raznovrsnih metoda vizuelne kriptografije, opisanih u prethodnom poglavlju, omogućio je njihovu primjenu u širokom spektru praktičnih scenarija. U nastavku su predstavljene najznačajnije oblasti u kojima vizuelna kriptografija nalazi konkretnu upotrebu.

### 4.1. Autentifikacija i verifikacija identiteta

Jedna od najprirodnijih primjena vizuelne kriptografije jeste autentifikacija dokumenata i verifikacija identiteta. Princip je jednostavan: povjerljiva vizuelna informacija — potpis, pečat, fotografija lica — šifruje se vizuelnom kriptografskom shemom i pohranjuje kao dvije dijeljene slike. Jedna slika se štampa na dokument ili identifikacionu karticu, a druga se čuva kod ovlaštene institucije. Prilikom verifikacije, korisnik preklapa svoj dio sa dijelom koji posjeduje institucija, i autentičnost se potvrđuje vizuelnom inspekcijom rekonstruisane slike.

Ross i Othman su 2011. godine predložili primjenu vizuelne kriptografije za zaštitu biometrijskih podataka [18]. U njihovom pristupu, biometrijska slika lica se dijeli na dvije dijeljene slike korištenjem proširene vizuelne kriptografske sheme. Jedna dijeljena slika se pohranjuje na pametnu karticu korisnika, a druga u bazu podataka sistema. Pri autentifikaciji, obje slike se digitalno preklapaju i rekonstruisana slika se poredi sa licem korisnika. Ključna prednost ovog pristupa jeste da čak i u slučaju kompromitovanja baze podataka, napadač ne može rekonstruisati biometrijsku sliku jer posjeduje samo jednu dijeljenu sliku. Ovo je značajna prednost u poređenju sa klasičnim pristupima gdje se biometrijski šabloni čuvaju u obliku koji je potencijalno reverzibilan.

Vizuelna autentifikacija je naročito korisna u okruženjima sa ograničenom infrastrukturom — graničnim prelazima u udaljenim područjima, humanitarnim operacijama i sličnim kontekstima gdje nije uvijek moguće osigurati pouzdanu mrežnu konekciju za online verifikaciju.

### 4.2. Zaštita medicinskih podataka

Medicinska informatika predstavlja oblast u kojoj je povjerljivost podataka od izuzetnog značaja. Medicinske slike — rendgenski snimci, MRI skenovi, ultrazvučni snimci — sadrže osjetljive informacije o pacijentu i podliježu strogim regulativama o zaštiti privatnosti. Vizuelna kriptografija nudi elegantno rješenje za scenarije u kojima je potrebno dijeliti medicinske slike između više institucija bez rizika od neovlaštenog pristupa.

U tipičnom scenariju, dijagnostička slika se dijeli na *n* dijeljenih slika koje se distribuiraju različitim medicinskim ustanovama ili odjeljenjima. Nijedna pojedinačna institucija ne može vidjeti originalnu sliku — za rekonstrukciju je potrebna saradnja najmanje *k* institucija. Ovo je korisno, na primjer, kada pacijent traži drugo mišljenje od specijaliste u drugoj bolnici: obje strane moraju sarađivati da bi pristupile kompletnom snimku, čime se sprečava neovlaštena distribucija medicinskih slika.

Halftone vizuelna kriptografija [9] je naročito pogodna za medicinske primjene jer dijeljene slike mogu izgledati kao smislene medicinske fotografije niskog kvaliteta, čime se izbjegava sumnja da se radi o šifrovanom sadržaju. Ovo svojstvo smislenih dijeljenih slika (eng. *meaningful shares*) pruža dodatni sloj zaštite kroz nejasnoću — potencijalni napadač ne može ni prepoznati da datoteka sadrži kriptografski materijal.

### 4.3. Vizualni potpisi i vodeni žigovi

Zaštita autorskih prava nad digitalnim slikama predstavlja još jednu važnu oblast primjene vizuelne kriptografije. Koncept je sljedeći: vlasnik slike kreira vodeni žig (eng. *watermark*) — obično logotip ili potpis — i kodira ga vizuelnom kriptografskom shemom. Jedna dijeljena slika se ugrađuje u originalnu sliku na neprimjetan način, a druga se čuva kod vlasnika. U slučaju spora o autorskim pravima, vlasnik može dokazati vlasništvo preklapanjem svoje dijeljene slike sa onom koja je ugrađena u spornu sliku, čime se rekonstruiše vodeni žig.

Fu i Au su 2004. godine predložili integrisani pristup koji kombinuje vizuelnu kriptografiju i digitalno vodeno žigosanje [17]. Njihova metoda ugrađuje jednu dijeljenu sliku vodenog žiga direktno u koeficijente diskretne kosinusne transformacije (DCT) originalne slike, čime vodeni žig postaje otporan na uobičajene manipulacije poput kompresije, skaliranja i izrezivanja. Druga dijeljena slika ostaje kod vlasnika kao „ključ" za verifikaciju. Prednost ovog pristupa u odnosu na klasično vodeno žigosanje jeste u tome što ekstrakcija vodenog žiga ne zahtijeva originalnu sliku — dovoljna je samo vlasnikova dijeljena slika.

Yan, Jin i Kankanhalli su razmatrali primjenu vizuelne kriptografije za zaštitu štampanih dokumenata [25]. U njihovom radu, vizuelna kriptografska shema je dizajnirana tako da bude otporna na degradaciju kvaliteta koju unosi proces štampanja i skeniranja (eng. *print-and-scan*). Ovo je praktično važno jer se u stvarnom svijetu dijeljene slike često štampaju na papiru, a zatim skeniraju za digitalnu verifikaciju, pri čemu dolazi do gubitka informacija uslijed nesavršenosti štampača i skenera.

### 4.4. Sigurna komunikacija i bankarstvo

Vizuelna kriptografija nalazi primjenu i u sistemima za sigurnu komunikaciju, posebno u kontekstu internet bankarstva i online autentifikacije. Jedan od najzanimljivijih primjera je primjena u CAPTCHA sistemima — testovima koji razlikuju ljudske korisnike od automatizovanih programa (botova).

U CAPTCHA sistemu zasnovanom na vizuelnoj kriptografiji, server generiše tajnu sliku koja sadrži tekst ili kod koji korisnik treba da pročita. Slika se dijeli na dvije dijeljene slike: jedna se šalje korisniku putem interneta, a druga se prikazuje na već autentifikovanom uređaju (npr. pametnom telefonu). Korisnik preklapa obje slike i čita poruku. Prednost u odnosu na klasične CAPTCHA sisteme jeste u tome što je dekodiranje praktično nemoguće bez posjedovanja obje dijeljene slike, dok istovremeno ne zahtijeva složene algoritme strojnog učenja za generisanje.

U bankarskim aplikacijama, vizuelna kriptografija se koristi za sigurno dostavljanje jednokratnih lozinki (eng. *one-time passwords*, OTP) i transakcionih autorizacionih kodova. Banka izdaje korisniku prozirnu karticu sa jednom dijeljenom slikom. Prilikom svake transakcije, banka na ekranu prikazuje drugu dijeljenu sliku. Korisnik prislanja svoju prozirnu karticu na ekran i čita autorizacioni kod koji postaje vidljiv preklapanjem. Čak i ako napadač presretne sliku sa ekrana, bez fizičke kartice korisnika ne može rekonstruisati kod. Ovaj pristup pruža dvofaktorsku autentifikaciju — nešto što korisnik ima (karticu) i nešto što korisnik vidi (sliku na ekranu) — bez potrebe za elektronskim uređajima poput tokena ili generatora kodova.

---

## 5. Sigurnosni izazovi i ograničenja

Iako prethodna poglavlja ukazuju na brojne prednosti i mogućnosti primjene vizuelne kriptografije, važno je sagledati i njene nedostatke. Ovaj dio rada posvećen je analizi sigurnosnih izazova, poznatih napada i praktičnih ograničenja koja utiču na širu upotrebu vizuelnih kriptografskih shema.

### 5.1. Gubitak kontrastnosti i kvaliteta slike

Fundamentalno ograničenje vizuelne kriptografije zasnovane na OR operaciji jeste neizbježan gubitak kontrasta u rekonstruisanoj slici. Bijeli pikseli originalne slike nakon preklapanja ne postaju potpuno bijeli — uvijek postoji određeni broj crnih podpiksela koji unose šum. U osnovnoj (2, 2) shemi, bijeli pikseli se rekonstruišu sa 50% crnih podpiksela (siva boja), dok se crni pikseli rekonstruišu sa 100% crnih podpiksela. Relativni kontrast od 1/2 je maksimalan za ovu shemu, ali je subjektivno slika značajno tamnija i manje oštra od originala [1].

Situacija se dodatno pogoršava sa porastom broja dijeljenih slika. U *(k, n)* shemama, kontrast opada sa povećanjem *k* i *n*. Za veće vrijednosti *n*, rekonstruisana slika može biti toliko tamna i zašumljena da postaje teško čitljiva. Naor i Shamir su u svom drugom radu predložili metode za poboljšanje kontrasta korištenjem tzv. pokrivajućih baznih matrica (eng. *cover base*), ali poboljšanja su bila ograničena [5].

Ekspanzija piksela donosi dodatne probleme. Povećanje dimenzija slike ne narušava samo efikasnost skladištenja i prenosa, već i fizičku upotrebljivost sheme. Kada se dijeljene slike štampaju na prozirnim folijama, ekspanzija znači da su folije veće od originalne slike, što komplikuje precizno poravnanje prilikom preklapanja. Čak i mala nepreciznost u poravnanju može značajno degradirati kvalitet rekonstruisane slike, posebno za velike ekspanzije piksela.

XOR-bazirana vizuelna kriptografija [23] rješava problem kontrasta jer XOR operacija daje savršenu rekonstrukciju — bijeli pikseli su potpuno bijeli. Međutim, ova prednost dolazi uz cijenu gubitka jednostavnosti: XOR operacija se ne može ostvariti fizičkim preklapanjem prozirnih folija, već zahtijeva elektronski uređaj, čime se gubi jedna od ključnih prednosti vizuelne kriptografije.

### 5.2. Napadi na vizuelnu kriptografiju

Iako vizuelna kriptografija pruža informaciono-teorijsku sigurnost protiv pasivnih protivnika — onih koji samo posmatraju dijeljene slike — postoje napadi koji iskorištavaju aktivno ponašanje nepoštenih učesnika. Najznačajniju kategoriju čine tzv. varljive sheme (eng. *cheating attacks*), u kojima jedan ili više nepoštenih učesnika pokušavaju da prevare poštene učesnike tako da im prikažu lažnu tajnu sliku.

Horng, Chen i Tsai su 2006. godine detaljno analizirali ovaj tip napada [14]. Scenarij je sljedeći: nepošteni učesnik koji posjeduje jednu dijeljenu sliku konstruiše lažnu dijeljenu sliku koja, kada se preklopi sa dijeljenom slikom poštenog učesnika, otkriva drugačiju poruku od originalne tajne. Pošteni učesnik nema način da utvrdi da je prevaren jer rekonstruisana slika izgleda potpuno validno. Ovaj napad je posebno opasan u aplikacijama poput vizuelne autentifikacije, gdje bi lažna rekonstrukcija mogla navesti korisnika na pogrešnu odluku.

Hu i Tzeng su 2007. godine predložili mehanizme za prevenciju varanja u vizuelnim kriptografskim shemama [15]. Njihov pristup uvodi verifikacione piksele — dodatne obrasce ugrađene u dijeljene slike koji omogućavaju učesnicima da provjere autentičnost dijeljenih slika prije nego što im vjeruju. Ova tehnika dodaje određenu redundancu i povećava ekspanziju piksela, ali pruža zaštitu od aktivnih napadača. Tsai, Chen i Horng su predložili sličan pristup specifično za binarne vizuelne kriptografske sheme [22].

Pored varljivih napada, postoje i kolusioni napadi u kojima grupa nepoštenih učesnika dijeli svoje dijeljene slike kako bi pokušala rekonstruisati tajnu bez dovoljnog broja legitimnih dijelova. U pravilno dizajniranoj *(k, n)* shemi, ovo je nemoguće dok god broj koluzionih učesnika ostaje manji od *k*. Međutim, u praksi, nepažljivo čuvanje dijeljenih slika ili curenje informacija putem bočnih kanala (npr. analiza veličine datoteke ili metapodataka) može oslabiti sigurnosne garancije.

### 5.3. Ograničenja u praktičnoj primjeni

Uprkos elegantnosti koncepta, vizuelna kriptografija se suočava sa nizom praktičnih ograničenja koja usporavaju njenu širu primjenu. Prvo i najočiglednije ograničenje tiče se fizičkog preklapanja. Da bi se tajna slika rekonstruisala optičkim putem, dijeljene slike moraju biti odštampane na prozirnim folijama i precizno poravnane. Ovo zahtijeva odgovarajući materijal, kvalitetan štampač sposoban za štampu na prozirnom mediju i vješte ruke korisnika — uslovi koji nisu uvijek ispunjeni u realnim scenarijima.

Digitalno preklapanje eliminiše potrebu za folijama, ali uvodi novi problem: potreban je računarski uređaj za prikaz i kombinovanje slika. Time se gubi osnovna prednost vizuelne kriptografije — nezavisnost od računarske opreme. U tom slučaju, postavlja se legitimno pitanje zašto ne koristiti konvencionalne kriptografske algoritme koji su efikasniji i pružaju veći nivo kontrole.

Skalabilnost predstavlja dodatno ograničenje. Za sheme sa velikim brojem učesnika, ekspanzija piksela može postati neprihvatljivo velika. U *(n, n)* shemi, ekspanzija raste eksponencijalno sa *n*, što za *n* > 10 rezultira dijeljenim slikama koje su toliko velike da postaju praktično neupotrebljive. Metode zasnovane na slučajnim mrežama [10], [13] ublažavaju ovaj problem, ali uz smanjenje kontrasta.

Konačno, vizuelna kriptografija je inherentno ograničena na statičke slike. Primjena na video sadržaj je teorijski moguća — obradom svake sličice (eng. *frame*) zasebno — ali je izuzetno nepraktična zbog količine podataka i nemogućnosti preklapanja folija u realnom vremenu. Također, vizuelna kriptografija ne pruža zaštitu integriteta podataka — ne postoji ugrađeni mehanizam koji bi spriječio modifikaciju dijeljenih slika bez detekcije, osim ako se ne kombinuje sa dodatnim verifikacionim tehnikama [15].

---

## 6. Pregled literature i aktuelna istraživanja

Nakon detaljnog pregleda metoda, primjena i sigurnosnih izazova, u ovom poglavlju se daje hronološki pregled najvažnijih naučnih doprinosa u oblasti vizuelne kriptografije, kao i osvrt na aktuelne pravce istraživanja.

### 6.1. Hronološki pregled ključnih radova

Razvoj vizuelne kriptografije može se pratiti kroz nekoliko jasno definisanih faza. Prva faza (1994–1996) obuhvata postavljanje temelja: Naor i Shamir uvode koncept i osnovne sheme [1], a Ateniese i saradnici generalizuju model na opšte pristupne strukture [4]. Ovaj period je obilježen pretežno teorijskim doprinosima, sa fokusom na formalne definicije i dokazivanje sigurnosti.

Druga faza (1997–2003) karakteriše se intenzivnim radom na proširenju vizuelne kriptografije izvan crno-bijelih slika. Ito i saradnici predlažu prvu shemu bez ekspanzije piksela [6], Yang i Laih uvode sheme za slike u boji [7], Blundo i saradnici razvijaju model za nijanse sive [24], a Hou predlaže halftone pristup za pune boje [8]. Lin i Tsai rade na tehnikama raspršivanja za slike u nijansama sive [12]. U ovom periodu se pojavljuju i prve primjene — Nakajima i Yamaguchi razmatraju prirodne slike [19], dok Fu i Au kombinuju vizuelnu kriptografiju sa vodenim žigovima [17].

Treća faza (2004–2011) donosi diversifikaciju istraživačkih pravaca. Jin i saradnici uvode progresivnu vizuelnu kriptografiju [11], Horng i saradnici analiziraju napade varanjem [14], a Hu i Tzeng predlažu mehanizme za prevenciju varanja [15]. Zhou i saradnici formalizuju halftone vizuelnu kriptografiju [9], Shyu razvija metode slučajnih mreža [10], a Chen i Tsao ih proširuju na opšte prag-sheme [13]. Ross i Othman demonstriraju primjenu u biometrijskim sistemima [18]. Liu i saradnici predlažu koračnu konstrukciju baznih matrica [21].

Četvrta faza (2012–danas) obilježena je konsolidacijom rezultata i novim pravcima. De Prisco i De Santis formalizuju vezu između slučajnih mreža i determinističke vizuelne kriptografije [16], dok se istražuju primjene u cloud okruženjima i integracija sa tehnikama dubokog učenja za poboljšanje kvaliteta rekonstruisanih slika.

### 6.2. Noviji pravci istraživanja

Među aktuelnim istraživačkim pravcima ističu se tri područja. Prvo, XOR-bazirana vizuelna kriptografija [23] dobija na značaju jer postiže savršen kontrast, a sve veća rasprostranjenost pametnih telefona i tableta čini elektronsko preklapanje praktičnijim nego ikada. Drugo, metode slučajnih mreža nastavljaju da se razvijaju ka shemama sa boljim kontrastom i manjom složenošću, uz zadržavanje nulte ekspanzije piksela. Treće, pojavljuju se hibridni pristupi koji kombinuju vizuelnu kriptografiju sa steganografijom, homomorfnim šifrovanjem ili blockchain tehnologijom za postizanje dodatnih sigurnosnih svojstava.

### 6.3. Tabelarni pregled najznačajnijih doprinosa

| Godina | Autori | Doprinos | Referenca |
|--------|--------|----------|-----------|
| 1979 | Shamir | Shema dijeljenja tajni zasnovana na polinomima | [2] |
| 1979 | Blakley | Geometrijska shema dijeljenja tajni | [3] |
| 1994 | Naor, Shamir | Uvođenje vizuelne kriptografije, (k,n) sheme | [1] |
| 1996 | Ateniese et al. | Generalizacija na opšte pristupne strukture | [4] |
| 1999 | Ito et al. | Vizuelna kriptografija bez ekspanzije piksela | [6] |
| 2000 | Yang, Laih | Vizuelna kriptografija za slike u boji | [7] |
| 2000 | Blundo et al. | Proširenje na nijanse sive | [24] |
| 2003 | Hou | Halftone pristup za slike u boji | [8] |
| 2003 | Lin, Tsai | Tehnike raspršivanja za nijanse sive | [12] |
| 2005 | Jin et al. | Progresivna vizuelna kriptografija u boji | [11] |
| 2005 | Tuyls et al. | XOR-bazirana vizuelna kriptografija | [23] |
| 2005 | Cimato et al. | Optimalne obojene prag-sheme | [20] |
| 2006 | Zhou et al. | Formalizacija halftone vizuelne kriptografije | [9] |
| 2006 | Horng et al. | Analiza napada varanjem | [14] |
| 2007 | Shyu | Vizuelna kriptografija sa slučajnim mrežama | [10] |
| 2007 | Hu, Tzeng | Prevencija varanja u VC shemama | [15] |
| 2010 | Liu et al. | Koračna konstrukcija baznih matrica | [21] |
| 2011 | Chen, Tsao | Prag-sheme zasnovane na slučajnim mrežama | [13] |
| 2011 | Ross, Othman | Primjena u zaštiti biometrijskih podataka | [18] |
| 2014 | De Prisco, De Santis | Formalna veza random grid i determinističke VC | [16] |

---

## 7. Zaključak

Vizuelna kriptografija, od svog nastanka 1994. godine, izrasla je u bogatu i raznovrsnu oblast istraživanja koja spaja kriptografiju, teoriju informacija, kombinatoriku i obradu slike. Ovaj rad je pružio sistematičan pregled te oblasti — od teorijskih osnova i formalnih definicija, preko pregleda metoda i praktičnih primjena, do analize sigurnosnih izazova i aktuelnih istraživačkih pravaca.

Glavne prednosti vizuelne kriptografije ostaju nepromijenjene od njenog nastanka: jednostavnost dekodiranja bez računarske opreme, informaciono-teorijska sigurnost i intuitivnost koncepta koja ne zahtijeva tehničko predznanje korisnika. Ove osobine je čine posebno pogodnom za specifične primjene poput autentifikacije dokumenata, zaštite biometrijskih podataka i sigurnog prenosa vizuelnih informacija u okruženjima sa ograničenom infrastrukturom.

S druge strane, ograničenja poput ekspanzije piksela, gubitka kontrasta, ranjivosti na napade varanjem i poteškoća sa fizičkim preklapanjem sprečavaju širu primjenu u svakodnevnoj praksi. Istraživači su tokom protekle tri decenije uspjeli značajno ublažiti mnoga od ovih ograničenja — metode slučajnih mreža eliminišu ekspanziju piksela, XOR-bazirana vizuelna kriptografija postiže savršen kontrast, halftone tehnike omogućavaju smislene dijeljene slike, a verifikacioni mehanizmi pružaju zaštitu od varanja. Međutim, savršeno rješenje koje istovremeno eliminiše sva ograničenja još uvijek ne postoji, i vjerovatno nikada neće postojati jer su neki od kompromisa inherentni prirodi problema.

Perspektive budućeg razvoja su obećavajuće. Rastuća rasprostranjenost pametnih uređaja sa kamerama i ekranima čini digitalno preklapanje praktičnijim, čime se otvara prostor za širu primjenu XOR-baziranih shema i interaktivnih vizuelno-kriptografskih protokola. Integracija sa blockchain tehnologijom mogla bi riješiti problem povjerenja u distribuciji dijeljenih slika, dok napredak u oblasti strojnog učenja nudi mogućnosti za poboljšanje kvaliteta rekonstruisanih slika. Vizuelna kriptografija, premda neće zamijeniti konvencionalne kriptografske sisteme, zauzima svoju jedinstvenu nišu u širokom spektru alata za zaštitu informacija — nišu u kojoj je jednostavnost jednako važna kao i sigurnost.

---

## 8. Literatura

[1] M. Naor and A. Shamir, "Visual cryptography," in *Advances in Cryptology — EUROCRYPT '94*, Lecture Notes in Computer Science, vol. 950, Berlin, Germany: Springer, 1995, pp. 1–12.

[2] A. Shamir, "How to share a secret," *Communications of the ACM*, vol. 22, no. 11, pp. 612–613, Nov. 1979.

[3] G. R. Blakley, "Safeguarding cryptographic keys," in *Proc. AFIPS National Computer Conference*, vol. 48, 1979, pp. 313–317.

[4] G. Ateniese, C. Blundo, A. De Santis, and D. R. Stinson, "Visual cryptography for general access structures," *Information and Computation*, vol. 129, no. 2, pp. 86–106, Sep. 1996.

[5] M. Naor and A. Shamir, "Visual cryptography II: Improving the contrast via the cover base," in *Security Protocols*, Lecture Notes in Computer Science, vol. 1189, Berlin, Germany: Springer, 1997, pp. 197–202.

[6] R. Ito, H. Kuwakado, and H. Tanaka, "Image size invariant visual cryptography," *IEICE Transactions on Fundamentals of Electronics, Communications and Computer Sciences*, vol. E82-A, no. 10, pp. 2172–2177, Oct. 1999.

[7] C. N. Yang and C. S. Laih, "New colored visual secret sharing schemes," *Designs, Codes and Cryptography*, vol. 20, no. 3, pp. 325–336, Jul. 2000.

[8] Y. C. Hou, "Visual cryptography for color images," *Pattern Recognition*, vol. 36, no. 7, pp. 1619–1629, Jul. 2003.

[9] Z. Zhou, G. R. Arce, and G. Di Crescenzo, "Halftone visual cryptography," *IEEE Transactions on Image Processing*, vol. 15, no. 8, pp. 2441–2453, Aug. 2006.

[10] S. J. Shyu, "Image encryption by random grids," *Pattern Recognition*, vol. 40, no. 3, pp. 1014–1031, Mar. 2007.

[11] D. Jin, W. Q. Yan, and M. S. Kankanhalli, "Progressive color visual cryptography," *Journal of Electronic Imaging*, vol. 14, no. 3, art. 033019, Jul. 2005.

[12] C. C. Lin and W. H. Tsai, "Visual cryptography for gray-level images by dithering techniques," *Pattern Recognition Letters*, vol. 24, no. 1–3, pp. 349–358, Jan. 2003.

[13] T. H. Chen and K. H. Tsao, "Threshold visual secret sharing by random grids," *Journal of Systems and Software*, vol. 84, no. 7, pp. 1197–1208, Jul. 2011.

[14] G. Horng, T. Chen, and D. S. Tsai, "Cheating in visual cryptography," *Designs, Codes and Cryptography*, vol. 38, no. 2, pp. 219–236, Feb. 2006.

[15] C. M. Hu and W. G. Tzeng, "Cheating prevention in visual cryptography," *IEEE Transactions on Image Processing*, vol. 16, no. 1, pp. 36–45, Jan. 2007.

[16] R. De Prisco and A. De Santis, "On the relation of random grid and deterministic visual cryptography," *IEEE Transactions on Information Forensics and Security*, vol. 9, no. 4, pp. 653–665, Apr. 2014.

[17] M. S. Fu and O. C. Au, "Joint visual cryptography and watermarking," in *Proc. IEEE International Conference on Multimedia and Expo (ICME)*, 2004, pp. 975–978.

[18] A. Ross and A. A. Othman, "Visual cryptography for biometric privacy," *IEEE Transactions on Information Forensics and Security*, vol. 6, no. 1, pp. 70–81, Mar. 2011.

[19] M. Nakajima and Y. Yamaguchi, "Extended visual cryptography for natural images," *Journal of WSCG*, vol. 10, no. 2, pp. 303–310, 2002.

[20] S. Cimato, R. De Prisco, and A. De Santis, "Optimal colored threshold visual cryptography schemes," *Designs, Codes and Cryptography*, vol. 35, no. 3, pp. 311–335, Jun. 2005.

[21] F. Liu, C. Wu, and X. Lin, "Step construction of visual cryptography schemes," *IEEE Transactions on Information Forensics and Security*, vol. 5, no. 1, pp. 27–38, Mar. 2010.

[22] D. S. Tsai, T. H. Chen, and G. Horng, "A cheating prevention scheme for binary visual cryptography," *Journal of Imaging Science and Technology*, vol. 51, no. 6, pp. 534–541, 2007.

[23] P. Tuyls, H. D. L. Hollmann, J. H. van Lint, and L. Tolhuizen, "XOR-based visual cryptography schemes," *Designs, Codes and Cryptography*, vol. 37, no. 1, pp. 169–186, Oct. 2005.

[24] C. Blundo, A. De Santis, and M. Naor, "Visual cryptography for grey level images," *Information Processing Letters*, vol. 75, no. 6, pp. 255–259, Oct. 2000.

[25] W. Q. Yan, D. Jin, and M. S. Kankanhalli, "Visual cryptography for print and scan applications," in *Proc. IEEE International Symposium on Circuits and Systems (ISCAS)*, 2004, pp. 572–575.

---

