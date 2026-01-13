---
title: "Näyttöprojektini idea"
date: 2026-01-10 11:24:00+0000
categories: [Blogging, Cyber Security, C#, Software Development]
tags: [cs, blogging, school]
description: "Ideani näyttöäni varten"
comments: true
---

# Näyttöprojektini (ainakin osittain)

## Mitä olen tekemässä näyttöä varten?
Olen luomassa projektia joka yhdistää päästä päähän salatun viestimen (C#), PHP:lla kirjoitetun API:n esim. kirjautumista sun muuta varten sekä samalla kielellä kirjoitetun WebSocket palvelimen (olen vähän laiska enkä jaksanut kirjoittaa sitä C#:lla).<br><br>
Tämän lisäksi viestin käyttää SQLite tietokantaa johon tällä hetkellä tallentuu vain käyttäjän kontaktit (yhteystiedot).<br>
Backend puolella päätin käyttää MySQLaa tai tarkemmin ottaen MariaDB:tä, sillä sen parissa minulla on eniten kokemusta. Tiedän kyllä, että suurin osa viestisovelluksista kuten Discord käyttävät MongoDB:tä, mutta en ollut täysin varma olisiko se täysin yhteensopiva tämän hetkisen setuppini kanssa.

## Missä vaiheessa projektini on?
Tällä hetkellä olen kirjoittanut valmiiksi API:n, WebSocket palvelimen viestintää varten, sekä osan tästä clientistä eli asiakasohjelmsta. Olen myös luonut tarvittavat SQL schemat jotka ovat erittäin simppeleitä.

Uskoisin, että prosentuaalisesti olen noin 80% valmis projektini kanssa.

## Miten viestien salaus toimii?
Vaikka tämä ei varmastikaan ole täysin paras ratkaisu, käytän .NET:in mukana tulevaa ```System.Security.Cryptography``` kirjastoa joka tarjoaa minulle mahdollisuuden luoda RSA keypairin helposti. Elikkä public ja private keyn.

Simppelisti selitettynä, ensimmäisellä käynnistys kerralla asiakasohjelma luo keypairin ja tallentaa sen levylle (ei todellakaan tietoturvallisin ratkaisu, mutta en usko että tälläiseltä demo projektilta sitä nyt varsinaisesti odotetaan muutenkaan).

Näitä avaimia (public ja private key) käytetään salaamiseen siten, että viestin saajan viesti salataan hänen avoimella (public key):llä, jonka hän pystyy myöhemmin purkamaan käyttämällä omaa salaista (private key) avainta.

## Asiakasohjelman UI
* Päätin luoda UI:n käyttämällä WinFormsia ja lisäsin siihen vähän tyyliä käyttämällä Krypton.Toolkit nimistä kirjastoa.
    * Syynä tähän on se, että XAML skillini eivät todellakaan ole parhaat, ja tässä haluaisinkin parantua jatkossa.

## Puuttuvat ominaisuudet
* Käyttäjä ei voi tällä hetkellä poistaa taikka estää toista käyttäjää.
* Itse viestintä ei ole vielä mahdollista.
* Varmasti kaikkea muuta mitä en ulkoa muista.

## Mahdolliset tämän hetkiset tietoturva ongelmat
* Avainten tallennus ei ole millään tavalla salattu.
* Kuka tahansa pystyy lukemaan SQLite:lla tehdyn Contacts (yhteystiedot) tietokannan ja selvittää kenen kanssa käyttäjä on jutellut.
* Käytetään HTTP:tä ja WS:ää eikä niiden salattuja versioita (koska localhostilla tämän setuppaaminen on hieman haastavampaa)

## Minkälaisia muutoksia projektiin on tullut?
* Kirjoitin koko WinForms UI:n täysin alusta. Kuten myös PHP API:n. Tähän kesti ainakin pari viikkoa extra aikaa, mutta olin silti motivoitunut saamaan projektin valmiiksi.
* Vaihdoin BouncyCastlen OpenPGP kirjastosta natiivimpaan System.Security.Cryptographyyn salausta varten (PGP -> RSA)

## Miksi päätin tehdä juuri tälläisen sovelluksen?
* Lähiaikoina on ollut paljon puhetta EU:n Chat Control laista, ja uskon että jokaisella ihmisellä on oikeus jutella vapaasti ilman pelkoa siitä, että joku tiedustelu-upseeri Ivan Ivanovich tai rikoskonstaapeli Matti Meikäläinen lukee heidän viestinstä. Oli se sitten vain "hyvää huomenta" tai "olisiko sinulla myydä minulle huumeita?". Tässä päästään siihen ongelmaan, että uskon että tälläistä projektia voi käyttää sekä hyviin tarkoituksiin, että myös huonoihin. Tästä muodostui eräänlainen moraalinen dilemma, mutta päätin silti jatkaa projektin parissa.
* Muita syitä ovat se, että halusin oppia lisää kryptografiasta sillä se on asia joka minua kiinnostaa.