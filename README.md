## Salesforce integration with Rules

### Installation
- Create a rule
  - add an action: create Salesforce submission entity
  - set Salesforce submission field(*X)
  - submit Salesforce submission


Version "7.x-0.1"

## Rules Salesforce briefing

A set of Rules actions that handles user registration information of the following format:

- Contact
  - name
  - lastname
  - ...
- Opleidingsvoorkeur
  - ...
- CampaignMember
  - ...

(see Salesforce structure)

### Specifics:

- Salesforce keys (gebruikerssleutel)
- mapping van data aan salesforce objecten, zie puntje **Mapping**
- bijhouden in een entity (bvb: registration)
- generic module met hooks: VUB specifics via hooks of configuratie toevoegen
- na in/upsert van de data in Salesforce wordt de Salesforce id toegevoegd aan de data set in de entity (bvb: registration)
- gebruik de salesforce API module

### Logica

- een Rules event wordt getriggered. 
- data wordt via het event beschikbaar gesteld
- de Rules action pikt de data op
  - in de rules configuratie is gedefinieerd welke relaties er bestaan tussen de data en de salesforce data
  
### Mapping

- … 

  
####  Use case 1

*gebruiker A registreert voor een campagne X met status Y*

We configureren als volgt:

- via de mapping tabel wordt een nieuw veld aangemaakt, dat wordt gekoppeld aan een veld van een ContactPerson uit de SalesForce API. We krijgen een rij te zien met een Data selector veld (selecteer uit de gecapteerde data) (DATA_SELECTOR_1), een select list (kies een veld in de Salesforce API) (SELECT_1) en een checkbox (BOOL_1) (dit veld is een Salesforce key).
- alle velden die gewenst zijn kunnen aangemaakt worden en gekoppeld worden, inclusief campagne en status
- er kunnen sleutels (keys) toegevoegd worden
- voor de keys kunnen in de data selector (adhv tokens? php evaluation?) berekeningen gemaakt worden: voornaam&naam&email bvb
- er moet geen data validatie gebeuren, die dient in de toeleverende source gebeuren die het event triggered.

**neen: nieuwe logic:**

- entity aanmaken,
- fields in entity vullen met juiste data
  - create entity action
  - Set a data value
  - te ontwikkelen action: send entity to Salesforce


####  Use case 2

*gebruiker B registreert voor een campagne C met status D, voorkeursopleiding E gelinkt aan campagne C en gebruiker B*

We gebruiken alles in Use case 2, maar met de volgende extra requirements:

- een afzonderlijk onbject moet gedefinieerd kunnen worden (generiek, adhv de API)
- de select list uit Use Case 1 (SELECT_1) wordt uitgebreid met een tweede (SELECT_2) die alle objecten oplijst: de default *Contactperson* en het bijgecreeerde object (bvb voorkeursopleiding, bedrijf,...)

#### Referenties

- voor referenties, kijk naar de webform_salesforce.module.
- zie ook Rules data type vub_events_data in vub_events.rules.inc (hook_rules_data_info)
- https://drupal.org/project/salesforce