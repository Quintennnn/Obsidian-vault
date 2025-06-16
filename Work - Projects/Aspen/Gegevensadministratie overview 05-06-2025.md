
# Gegevensadministratie – Verzekeringsproduct

Dit document bevat een overzicht en checklist voor de verwerking van gegevens bij het maken van een nieuw verzekeringsproduct. Het proces is opgedeeld in twee hoofdfases:

1. Verwerking van offertegegevens
2. Verwerking van persoonsgegevens na akkoord offerte

---

## Fase 1: Offerte – verwerking van aangeleverde Excel-bestand

### Doel
Verwerken van een gestandaardiseerde Excel met meerdere sheets waarin premiedata staat, vóórdat er een offerte wordt gemaakt.

### Input
- Excelbestand (gestandaardiseerd formaat)
- Meerdere sheets met specifieke datastructuur

### Output
- JSON-object met gestructureerde, geschoonde en gevalideerde data
- Eventuele foutmeldingen (sanity checks)

### Acties
- [ ] Ontvangst en opslag van Excelbestand
- [ ] Inlezen van meerdere sheets met behulp van een Lambda functie
- [ ] Mapping van sheetnamen en kolomnamen naar interne datastructuur
- [ ] Sanity checks:
  - [ ] Verplichte velden
  - [ ] Typevalidatie (bijv. getallen, datums)
  - [ ] Waardevalidatie (bijv. premie mag niet negatief zijn)
  - [ ] Duplicatencontrole
  - [ ] Controleren of verwachte sheetnamen aanwezig zijn

### Openstaande vragen
- [ ] Welke velden zijn verplicht?
- [ ] Welke velden mogen leeg zijn?
- [ ] Zijn er defaultwaarden of validatieregels per veld?
- [ ] Moeten we per sheet iets specifieks doen?
- [ ] Welke foutmeldingen moeten zichtbaar worden?

### Aandachtspunten
- Excel kan onjuist ingevuld of corrupt zijn – robuuste foutafhandeling vereist
- Versiebeheer van templates kan nodig zijn bij wijzigingen

---

## Fase 2: Na offerte – verwerking persoonsgegevens

### Doel
Verwerken van persoonsgegevens nadat een offerte is geaccepteerd, via een door ons opgestelde Excel-template.

### Input
- Excelbestand (template door ons gedefinieerd)
- Eén sheet met persoonsgegevens (bijv. verzekerden, begunstigden, contactinformatie)

### Output
- JSON-object met gestructureerde persoonsgegevens
- Foutmeldingen bij ontbrekende of ongeldige waarden

### Acties
- [ ] Template specificeren:
  - [ ] Benodigde velden bepalen
  - [ ] Kolomnamen vastleggen
  - [ ] Format en validatieregels definiëren
- [ ] Implementatie sanity checks:
  - [ ] Verplichte velden ingevuld?
  - [ ] Typevalidatie (bijv. geboortedatum als geldige datum)
  - [ ] Validatie op BSN, e-mailadres, telefoonnummer
  - [ ] Controle op uniciteit van records
- [ ] Inlezen met Lambda functie
- [ ] JSON-output genereren

### Openstaande vragen
- [ ] Welke persoonsgegevens zijn exact nodig?
- [ ] Moeten meerdere verzekerden in één bestand verwerkt kunnen worden?
- [ ] Is koppeling met een bestaand systeem nodig?
- [ ] Moet encryptie of versleuteling worden toegepast?
- [ ] Wat zijn de vereisten vanuit de AVG?

### Aandachtspunten
- Privacygevoelige data vereist goede beveiliging en logging
- Template moet duidelijk, foutbestendig en herbruikbaar zijn
- Versiebeheer van templates en validatieregels noodzakelijk

---

## Overkoepelend – Techniek en documentatie

### Techniek
- [ ] Lambda functies geschreven in [taal/tooling]
- [ ] Inlezen Excel via bijvoorbeeld `openpyxl`, `pandas`, of AWS SDK
- [ ] Sanity check helpermodule/framework opzetten
- [ ] Output structureren als JSON volgens afgesproken schema

### Documentatie
- [ ] Specificaties van templates (offerte en persoonsgegevens)
- [ ] Validatieregels per veld
- [ ] Voorbeeldinput en -output
- [ ] Logging- en foutafhandelingsbeleid
- [ ] Toekomstige uitbreiding: integratie met backend of opslag in database