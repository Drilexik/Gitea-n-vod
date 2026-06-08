# 📦 Návod: GitHub Desktop + Synergy Gitea

Tímto systémem spravujeme soubory na serverech. Pracuješ u sebe v počítači, změny **pushneš**, a jakmile je majitel schválí, dostanou se na ostrý server.

> **Důležité pojmy ve zkratce**
> - **Repozitář (repo)** = složka projektu pod správou gitu (např. `cars`, `mlo`, `clothes`).
> - **Větev `dev`** = Dev server. Tady děláš všechno, otestuješ to a dáš commit, až toho bude víc tak PR.
> - **Větev `main`** = Main server. Přímo sem **nikdo pushovat nemůže** — změny tam jdou jen přes **Pull Request (PR)**, které musím schválit.
> - **Commit** = Dáš po každé změně (př. upravíš lokace v configu, dáš commit -m "změna lokací"). Tohle se ukládá lokálně na PC.
> - **Push** = Pošle lokální commity na server (dev).
> - **Pull / Fetch** = stažení nejnovějších změn od ostatních.
> - **Pull Request (PR)** = žádost „vezmi moje změny z `dev` do `main`".

---

## 1) Stažení a instalace GitHub Desktop

1. Otevři **https://desktop.github.com**
2. Klikni na **Download for Windows** a nainstaluj (jen poklikat Next).
3. Spusť **GitHub Desktop**.

---

## 2) První spuštění — ⚠️ NEPŘIHLAŠUJ se na GitHub.com

GitHub Desktop tě hned nabízí přihlášení na GitHub.com. **To nepotřebujeme** — náš server je vlastní (Gitea).

1. Na úvodní obrazovce klikni dole na **Skip this step** (přeskočit přihlášení).
2. Vyplň **jméno** a **email** (slouží jen jako podpis u commitů) → **Finish**.
   - Použij login co dostaneš.

---

## 3) Stažení (naklonování) repozitáře do PC — tady se přihlásíš

Každý má přístup jen ke svým repozitářům (auta / mapy / oblečení). Adresa repa je vždy:

```
https://gitea.synergy-rp.eu/synergy/<repo>.git
```

kde `<repo>` je podle toho, co děláš:
- auta → `cars`
- mapy/MLO → `mlo`
- oblečení/EUP → `clothes`

**Postup:**
1. V GitHub Desktop: **File → Clone repository…**
2. Nahoře přepni na záložku **URL**.
3. Do pole vlož adresu, např.: `https://gitea.synergy-rp.eu/synergy/cars.git`
4. **Local path** = kam se to u tebe uloží (např. `C:\Synergy\cars`). Zapamatuj si to.
5. **Clone**.
6. Objeví se okno **Authentication / přihlášení**:
   - **Username** = tvoje Gitea jméno (které ti dal majitel)
   - **Password** = tvoje Gitea heslo
   - (Pokud by heslo nešlo, viz sekci *Řešení potíží* dole — vygeneruje se token.)

Hotovo — repo máš stažené ve své složce.

---

## 4) Přepni se na větev `dev` (pracuje se vždy tady)

1. Nahoře uprostřed je tlačítko **Current Branch**.
2. Klikni a vyber **`dev`**.
3. Pracuj vždy na `dev`. (Do `main` tě to stejně nepustí — je chráněná.)

---

## 5) Jak dělat změny a odeslat je

1. **Než začneš pracovat**, klikni nahoře na **Fetch origin** → pak **Pull**, ať máš nejnovější verzi od ostatních.
2. Otevři složku repa (tu z bodu 3) v normálním Průzkumníku a edituj soubory jako obvykle (svými nástroji).
3. Vrať se do GitHub Desktop — vlevo uvidíš **seznam změn**.
4. Dole vlevo napiš krátký **popis změny** (Summary), např. „přidán Audi RS6".
5. Klikni **Commit to dev**.
6. Nahoře klikni **Push origin** → změny jsou teď na serveru ve větvi `dev`.

> Můžeš commitovat a pushovat klidně několikrát denně. `dev` je na to.

---

## 6) Jak dostat změny na ostrý server (`main`) — Pull Request

Přímo do `main` nikdo nepushuje. Když chceš změny dostat na ostrý server:

1. Otevři v prohlížeči **https://gitea.synergy-rp.eu** a přihlas se svým účtem.
2. Otevři své repo → nahoře **Pull Requests** → **New Pull Request**.
3. Nastav:
   - **base** (kam) = `main`
   - **compare** (odkud) = `dev`
4. Klikni **New Pull Request**, napiš krátce co měníš, a **Create**.
5. **Já (drilex)** to zkontroluju a buď schválím + sloučím (merge), nebo napíšu připomínky co změnit.
6. Po schválení jsou změny v `main`. (Na server se nahrají po naplánovaném restartu.)

---

## 7) Každodenní rutina (zkráceně)

```
1. Otevři GitHub Desktop
2. Fetch origin → Pull   (stáhni nejnovější)
3. Uprav soubory ve složce
4. Summary + Commit to dev
5. Push origin
6. Až je to hotové a otestované → na webu Gitea udělej Pull Request dev → main
```

---

## 🔧 Řešení potíží

**„Authentication failed" / heslo nebere při klonování nebo pushi**
Vygeneruj si přístupový token a použij ho místo hesla:
1. Web **https://gitea.synergy-rp.eu** → vpravo nahoře tvůj profil → **Settings**
2. **Applications** → sekce *Generate New Token*
3. Pojmenuj (např. „github-desktop"), zaškrtni práva **repo**, **Generate Token**.
4. Token (dlouhý řetězec) zkopíruj.
5. V GitHub Desktop při dotazu na heslo zadej **Username** = tvoje jméno, **Password** = ten **token**.
> Token si ulož, ukáže se jen jednou.

**„Nemůžu pushnout do main" / push odmítnut**
To je správně — `main` je chráněná. Pushuj do `dev` a udělej Pull Request (sekce 6).

**„Updates were rejected" / „behind" při pushi**
Někdo pushnul před tebou. Klikni **Pull origin**, vyřeš případný konflikt, a pushni znovu.

**Konflikt (conflict)**
Když dva lidé upravili stejné místo. GitHub Desktop ti řekne, který soubor — otevři ho, nech správnou verzi, ulož, commitni. Když si nevíš rady, napiš majiteli, ať to nezkazíš.

**Vidím jen některá repa**
To je v pořádku — vidíš jen to, na co máš přístup (auta / mapy / oblečení). Devs vidí vše.

---

## ❗ Pravidla

- Pracuj **vždy na `dev`**, ne na `main`.
- **Před prací** dej Pull, ať nepřepisuješ cizí práci.
- Commit popisuj stručně a jasně (co se změnilo).
- Velké soubory / hotové věci na ostrý server → jedině přes **Pull Request**.

Adresa Gitey: **https://gitea.synergy-rp.eu** · Přístup ti zařídím v DMs
