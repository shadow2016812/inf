class Zwierze:
    def __init__(self, imie, gatunek, wiek):
        self.imie = imie
        self.gatunek = gatunek
        self.wiek = wiek

    def wydaj_dzwiek(self):
        return f"{self.imie} wydaje dźwięk."

    def informacje(self):
        return f"{self.imie} to {self.gatunek}, ma {self.wiek} lat."

class Ssak(Zwierze):
    def __init__(self, imie, gatunek, wiek, kolor_siersci):
        super().__init__(imie, gatunek, wiek)
        self.kolor_siersci = kolor_siersci

    def wydaj_dzwiek(self):
        return f"{self.imie} mówi: 'Dźwięki ssaka!'"

    def informacje(self):
        bazowe = super().informacje()
        return f"{bazowe} Ma sierść koloru {self.kolor_siersci}."

class Ptak(Zwierze):
    def __init__(self, imie, gatunek, wiek, rozpietosc_skrzydel):
        super().__init__(imie, gatunek, wiek)
        self.rozpietosc_skrzydel = rozpietosc_skrzydel

    def wydaj_dzwiek(self):
        return f"{self.imie} ćwierka głośno."

    def informacje(self):
        bazowe = super().informacje()
        return f"{bazowe} Rozpiętość skrzydeł: {self.rozpietosc_skrzydel} metrów."

class Gad(Zwierze):
    def __init__(self, imie, gatunek, wiek, jadowity):
        super().__init__(imie, gatunek, wiek)
        self.jadowity = jadowity

    def wydaj_dzwiek(self):
        return f"{self.imie} syczy."

    def informacje(self):
        bazowe = super().informacje()
        czy_jadowity = "jadowity" if self.jadowity else "niejadowity"
        return f"{bazowe} Jest {czy_jadowity}."

class Zagroda:
    def __init__(self, nazwa):
        self.nazwa = nazwa
        self.zwierzeta = []

    def dodaj_zwierze(self, zwierze):
        if isinstance(zwierze, Zwierze):
            self.zwierzeta.append(zwierze)

    def usun_zwierze(self, zwierze):
        self.zwierzeta.remove(zwierze)

    def lista_zwierzat(self):
        return [z.informacje() for z in self.zwierzeta]

class Zoo:
    def __init__(self, nazwa):
        self.nazwa = nazwa
        self.zagrody = []
        self.pracownicy = []

    def dodaj_zagrode(self, zagroda):
        if isinstance(zagroda, Zagroda):
            self.zagrody.append(zagroda)

    def dodaj_pracownika(self, pracownik):
        if isinstance(pracownik, Pracownik):
            self.pracownicy.append(pracownik)

    def wszystkie_zwierzeta(self):
        wynik = []
        for zagroda in self.zagrody:
            wynik.extend(zagroda.lista_zwierzat())
        return wynik

    def wszyscy_pracownicy(self):
        return [str(p) for p in self.pracownicy]

class Pracownik:
    def __init__(self, imie):
        self.imie = imie

    def __str__(self):
        return f"{self.imie} jest pracownikiem."

class Opiekun(Pracownik):
    def __init__(self, imie, przydzielone_zwierzeta=None):
        super().__init__(imie)
        self.przydzielone_zwierzeta = przydzielone_zwierzeta or []

    def karm_zwierzeta(self):
        return [f"{self.imie} karmi {zw.imie}." for zw in self.przydzielone_zwierzeta]

    def __str__(self):
        zw_nazwy = [z.imie for z in self.przydzielone_zwierzeta]
        return f"Opiekun {self.imie} opiekuje się {zw_nazwy}."

class Weterynarz(Pracownik):
    def __init__(self, imie):
        super().__init__(imie)

    def sprawdz_zdrowie(self, zwierze):
        return f"{self.imie} sprawdza zdrowie {zwierze.imie}."

    def __str__(self):
        return f"Weterynarz {self.imie}"

# --- FUNKCJE TESTOWE ---

def stworz_przykladowe_zoo():
    zoo = Zoo("Miejskie Zoo")

    ssaki = Zagroda("Strefa Ssaków")
    ptaki = Zagroda("Ptasi Raj")
    gady = Zagroda("Dom Gadów")

    lew = Ssak("Leo", "lew", 5, "złoty")
    slon = Ssak("Ela", "słoń", 12, "szary")
    orzel = Ptak("Edek", "orzeł", 4, 2.5)
    waz = Gad("Syk", "pyton", 6, True)

    ssaki.dodaj_zwierze(lew)
    ssaki.dodaj_zwierze(slon)
    ptaki.dodaj_zwierze(orzel)
    gady.dodaj_zwierze(waz)

    zoo.dodaj_zagrode(ssaki)
    zoo.dodaj_zagrode(ptaki)
    zoo.dodaj_zagrode(gady)

    opiekun = Opiekun("Anna", [lew, slon])
    wet = Weterynarz("Dr. Kowalski")

    zoo.dodaj_pracownika(opiekun)
    zoo.dodaj_pracownika(wet)

    return zoo

# --- SYMULACJA ---

if __name__ == "__main__":
    zoo = stworz_przykladowe_zoo()

    print(f"Witamy w {zoo.nazwa}!\n")

    print("Zwierzęta w zoo:")
    for info in zoo.wszystkie_zwierzeta():
        print(" -", info)

    print("\nPracownicy zoo:")
    for pracownik in zoo.wszyscy_pracownicy():
        print(" -", pracownik)

    print("\nDziałania opiekuna:")
    for osoba in zoo.pracownicy:
        if isinstance(osoba, Opiekun):
            for dzialanie in osoba.karm_zwierzeta():
                print(" -", dzialanie)

    print("\nKontrole weterynarza:")
    for osoba in zoo.pracownicy:
        if isinstance(osoba, Weterynarz):
            for zagroda in zoo.zagrody:
                for zwierze in zagroda.zwierzeta:
                    print(" -", osoba.sprawdz_zdrowie(zwierze))
