#Paskaita 1
﻿Probleminis mokymas

Mokantis administruoti Linux operacinę sistemą, geriausias mokymosi metodas yra probleminis. Pagal Barrows probleminis mokymas – tai mokymasis, kada darbo rezultatas yra problemos supratimas ir jos išsprendimas, o mokymosi procese pirmiausia apibrėžiama problema. Toks mokymasis padeda ne tik tvirtai išmokti naują kurso medžiagą, bet ir ugdo mūsų analitinį mąstymą. Kiekvieną kartą, kai skiriama užduotis, atsiranda sunkumų – programa neveikia, komanda apvalkale neatlieka to, ko norime. Sprendžiant šias problemas savarankiškai – ieškoma panašių klaidų internete, kituose šaltiniuose – informacija daug geriau įsisavinama. Pavyzdžiui, įkišus duotą USB laikmeną į kompiuterį, ji nėra aptinkama . Tokiu atveju keliami klausimai: ar tai programinės ar aparatinės įrangos klaida, ar panašios klaidos buvo aptiktos kitur, ar atsiranda klaidos pranešimas bandant prieiti prie laikmenos kelio, ar linux klaidų žurnaluose apie tai kas nors yra, kas būtų kitaip, jei šios problemos nebūtų ir ar eina ką nors pakeisti programinėje ar aparatinėje įrangoje, kad ši problema išnyktų. Eidami visą šį kelią, kad problemą  būtų išspręsta, užtrunkama tikrai ilgiau negu pasiklausus kolegos, kuris žino, kaip tai sutvarkyti.  Tačiau eidami šiuo keliu, mes atrandam daug nematytų komandų, sužinome, kurioje vietoje patalpinti įvairūs konfigūraciniai failai. Net jei nepavyksta prieiti prie USB laikmenos, įgautos žinios ieškant informacijos pasitarnaja ateityje susidūrus su panašiomis problemomis. Taigi, probleminis mokymas – efektyviausias būdas mokytis mūsų kurse, nes čia plečiamos ne tik mūsų žinios, bet ir ugdomas kritinis mąstymas, gilinami problemų sprendimo nei savarankiško mokymosi įgūdžiai, kurie pasitarnauja ne tik kurso medžiagoje, bet ir kitur.

Literatūra:
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=3&cad=rja&uact=8&ved=2ahUKEwjK-dOVg8_lAhUyzqYKHRU-AxkQFjACegQIABAC&url=http%3A%2F%2Fwww.esparama.lt%2Fes_parama_pletra%2Ffailai%2FESFproduktai%2F2012_I_problema_orientuoto_mokymo(si)_metodika.pdf&usg=AOvVaw1ilPxow3J_MXYozijfJlw3
#Paskaita 2
#Paskaita 3
#Paskaita 4
﻿Darbas su skaidinių formatais, susipažinimas su root vartotojo aplankais ir jų nauda

Visuomet turėti prieigą prie savo kompiuterio labai pravartu: turima prieiga prie rinkmenų, kurias galima redaguoti ar bet kada persiųsti kitur. Tam labai pravartus nuotolinis prisijungimas prie kompiuterio. Tačiau tam reikia, kad namų kompiuteris nuolat būtų įjungtas, o tai eikvoja elektros energiją.
 Tam į pagalbą atskuba Raspberry Pi – mažas pilnavertis kompiuteris, kuris eikvoja mažai resursų. Tačiau šiam kompiuteriui Windows operacinė sistema netinka, nes ji ryja daug resursų ir juo naudotis būtų labai sunku. Todėl į Raspberry Pi idiegsime Arch Linux.
Kad galėtume įrašyti programinę įrangą, reikia paruošti SD kortelę. Norint tai padaryti, pirmiausia reikia būti prisijungus su root vartotoju:

	$ su

Taip pat mums reikėtų pažiūrėti, ar mūsų kompiuterio sistema aptinka SD kortelę, įdėjus ją į skaitytuvą:

	$ dmesg | tail -30 | less

Turėtų pasirodyti tekstas, kad kortelė aptikta. Mano atveju:

	# [G] mmc0: new high speed SDHC card at address ... mmcblck0: mmc0:0007 SD16G 14.5 GiB

Taip pat reikėtų patikrinti kortelės vietos suskaldymą:

	$ cat /proc/partitions

Mano atveju:

	# mmcblk0 / nncblk0p1

Taigi tikėtina, kad kortelės pavadinimas mmcblk0. Ji nėra suskaldyta, visa atmintis išsaugota mmcblk0p1.

Norint tikrai įsitikinti ar prietaisas yra:

	$ ls -a /dev

Matome daugybę prietaisų. Įsitikinti ar, mūsų kortelė tikrai turi tokį pavadinimą galime išėmę kortelę vėl įvesti prieš tai esančią komandą. Kortelės pavadinimas turėjo dingti.

Taigi, dabar mums reikės suskaldyti kortelės atmintį į atskirus segmentus. To mums reikia todėl, nes kortelėje saugoma operacinė sistema ir paleidimo failai turi turėti skirtingą formatą. Formatavimui naudosime programą fdisk. Taigi:

		$ fdisk /dev/<kortelės pavadinimas>
		spaudžiame <o>	# sukuriame naują DOS skaidinį
		spaudžiame <p>	# spausdiname tuščią skaidinį
		spaudžiame <n>	# sukuriame mūsų pirmą skaidinį:
		spaudžiame <Enter> # numatytas pirminis skaidinio tipas
		spaudžiame <Enter>	# numatytas skaidinio skaičius 1
		spaudžiame <Enter>	# numatyta pradžia pirmame sektoriuje
		$ +100 M		# 100 MB talpa
	
spaudžiame &lt;t&gt; ir nustatome skaidinio tipą į W95 FAT32 (to mums reikia, kad galėtume pakeisti skaidinio formatą į FAT32).


Ta patį atliekame su antruoju skaidiniu, tačiau dydžio nekeičiame. Skaidinio tipo nekeičiame.

Dabar spaudžiame &lt;p&gt;, kad patikrintume, ką padarėme bei &lt;w&gt; kad įrašytume ir išeitume iš fdisk programos.

Įvedus $ cat /proc/partitions turėtų išmesti visus mūsų sukurtus skaidinius.

Taigi, sukūrus mūsų skaidinius, juos reikia formatuoti į reikiamus formatus:

	$ mkfs.vfat /dev/<paleidimo skaidinys>
	$ mkfs.ext4 /dev/<pagrindinis skaidinys>

Savo namų kataloge sukuriame du aplankalus:

	$ mkdir boot root

Įrengiame pirmąjį skaidinį boot aplankale, o antrąjį  – root:

	$ mount <skaidinys> <aplankalas>/

Atsisiunčiame Arch Linux ARM:

	$ wget http://os.archlinuxarm.org/os/ArchLinuxARm-rpi-2-lastest.tar.gz
	
Tuomet išskleižiame jį root aplanke ir įsitikiname, kad kortelė susieta:

	$ bsdtar -xpf ArchLinux* -C root && sync

Iš kelio root/boot viską perkeliame į boot aplanką, kuriame įrengtas mūsų paleidimo skaidinys.

Prieš išimdami kortelę:

	$ umount boot root

Taigi, mūsų operacinė sistema įrašyta į SD kortelę ir parengta naudoti.

Žodynas:

- Arch Linux ARM – operacinės sistemos Arch Linux atšaka, pritaikyta ARM architektūros procesoriui.
- SD kortelė – tai kortelės formos skaitmeninių duomenų laikmena.
- root – pagrindinis vartotojas Linux operacinėje sistemoje.
- /boot – aplankalas, kuriame saugomos rinkmenos apie operacinės sistemos paleidimą.
- $ dmesg – vaizduoja su Linux branduoliu sisijusias žinutes. Dažnai naudojama aptikti laikmenoms.
- tail – rodo paskutines eilutes.
- less – rodo mažiau informacijos.
- $ cat – parodo, kas yra rinkmenos viduje.
- /proc – aplankas, kuriame saugoma informacija apie Linux operacinės sistemos atmintį, prijungtus prietaisus, aparatinės įrangos konfigūraciją.
- /dev – aplankas, kuriame saugomos visos išorinių įrenginių konfigūracinės laikmenos.
- $ ls -a – parodo visas rinkmenas bei katalogus darbiniame aplankale.
- $ fdisk – komanda, kuri leidžia naudotis fdisk programa.
- FAT32 – laikmenos skaidinio formatas, rodantis, kokiu būdu saugomi duomenys.
- ext4 – laikmenos skaidinio formatas, rodantis, kokiu būdu saugomi duomenys.
- $ mkdir <aplankalo pavadinimas> – sukuria aplanką darbiniame aplankale.
- $ mkfs.<formatas> – formatuoja disko skaidinį pasirinktu formatu.
- $ mount <skaidinys> <aplankalas>/ – prijungti įrenginį (disko skirsnis prijungiamas prie aplanko kurį pasirenkame).
- $ umount – atjungti įrenginį.
- $ bsdtar – komanda, kuri leidžia naudotis bsdtar programa, norit dirbti su *.tar.gz formato rinkmenomis.
- $ wget – leidžia gauti failą iš internetinės talpyklos.

Šaltiniai:
1. - https://www.tldp.org/LDP/Linux-Filesystem-Hierarchy/html/proc.html
2. - http://ims.mii.lt/ALK%C5%BD/l.html
3. - https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=2ahUKEwiooa3f887lAhVJ1qYKHf5HDhIQFjAAegQIABAC&url=http%3A%2F%2Fhan.vandersluys.nl%2Fpub%2FPDFs%2FArch_ARM_install.pdf&usg=AOvVaw1J87KrN-bhhkMeg19iUYQW
4. - 4a2-Failai-ir-ju-naudojimo-teises-1dalis.pdf
#Paskaita 5
#Paskaita 6