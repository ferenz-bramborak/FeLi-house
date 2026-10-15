# FeLi house – Domeček u lesa

Hledání rodinného domu u Brna: blízko bráchy (Rosice), dědy (Střelice) a rodičů (Padochov),
u lesa, se zahradou pro psy a děti, ideálně bungalov, do 15 mil. Kč, 4+ pokoje, žádné řadovky.

**Web:** https://ferenz-bramborak.github.io/FeLi-house/

## Jak to běží

| Co | Kdo | Kdy |
|---|---|---|
| Stažení nabídek ze Sreality, iDNES Reality, Bezrealitek a Reasu, vyhodnocení lesa, terénu, dojezdů a vybavenosti, nasazení webu | GitHub Actions (`.github/workflows/nabidky-a-web.yml`) | každé ráno ~6:20 |
| Rozbor vybraných domů (Top / Možná) | Claude (naplánovaná rutina, postup v `RUTINA.md`) | každé ráno po stažení |
| Hlasy, hvězdičky „pocit“, poznámky, váhy skóre | vy dva ve webu | kdykoli |

Hlasy a poznámky se ukládají do `web/data/oznaceni.json`, váhy do `web/data/nastaveni.json`
a rozbor do `web/data/rozbor.json`. Repozitář je veřejný, takže poznámky může vidět kdokoli,
kdo ho najde. Nepište do nich nic citlivého.

## Zapnutí GitHub Pages (jednou)

*Settings → Pages → Build and deployment → Source: **GitHub Actions***. Pak v kartě *Actions*
spusťte workflow „Nabídky a web“ (*Run workflow*). Za pár minut je web na adrese výše.

## Přihlášení na telefonu (každý z vás jednou)

Web hlasy ukládá vaším GitHub tokenem. Token má právo jen na tento repozitář.

1. Na GitHubu: *Settings → Developer settings → Personal access tokens → Fine-grained tokens →
   Generate new token* (https://github.com/settings/personal-access-tokens/new).
2. *Token name*: `FeLi house – telefon`, *Expiration*: třeba 1 rok.
3. *Repository access*: **Only select repositories** → `ferenz-bramborak/FeLi-house`.
4. *Permissions → Repository permissions → **Contents: Read and write*** (nic jiného).
5. *Generate token* a zkopírujte ho (`github_pat_…`).
6. Na webu otevřete *Nastavení a váhy*, napište své jméno (Štěpán už má v datech dva hlasy,
   tak ať se přihlásí jako **Štěpán**) a vložte token. Token zůstane jen v tom telefonu.

Manželka potřebuje buď vlastní GitHub účet přidaný jako *collaborator* (Settings →
Collaborators) a vlastní token, nebo může použít stejný token jako vy, jen se přihlásí svým jménem.

## Ruční spuštění

```sh
pip install -r requirements.txt
python -m domy.run                 # stáhnout, ohodnotit, připravit web
python -m domy.run --bez-stahovani # jen přepočítat ze stavu
python -m unittest domy.test_domy
cd web && python -m http.server 8000
```

Kritéria hledání (cena, pokoje, pozemek, vzdálenost, místa rodiny, exity D1) jsou v
`domy/config.py`. Mapa lesů a reliéfu (`data/geo_mrizka.npz`) je předpočítaná z ESA WorldCover
a Copernicus DEM (`python -m domy.geo_sestav`, potřebuje navíc `pip install rasterio`).
