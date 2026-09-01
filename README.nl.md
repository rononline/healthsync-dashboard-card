[Original README in English](README.md) · [Русская версия](README.ru.md)

# HealthSync Dashboard Card

![Voorbeeld van de HealthSync Dashboard Card](images/preview.png)

Een compacte, responsieve dashboardkaart voor Home Assistant, bedoeld voor de
[HealthSync-integratie](https://github.com/mannotfood/healthsync). De kaart gebruikt
de sensoren van de integratie en de geschiedenis uit Recorder, zonder externe
frontend-afhankelijkheden.

> Dit is een onafhankelijk community-project en heeft geen banden met de auteur van HealthSync.

## Mogelijkheden

- Automatische herkenning van de standaard HealthSync-entiteiten
- Grafische kaarteditor van Home Assistant, met de mogelijkheid entiteiten handmatig te overschrijven
- Snel eerste beeld: de gevonden entiteiten worden gecachet en de Recorder-geschiedenis laadt pas nadat de kaart zichtbaar is
- Aparte schakelaars om elke tegel afzonderlijk te tonen of te verbergen
- Tegels zijn te herschikken met slepen en neerzetten, ook op mobiel, via de grafische editor
- Actuele stappen, actieve en rustcalorieën, hartslag, HRV en een slaapsamenvatting
- Vitale waarden uit HealthSync 0.20: hartslag in rust, bij wandelen en tijdens herstel, bloeddruk, AFib-belasting, SpO₂, ademhalingsfrequentie, temperatuur en bloedglucose
- Lichaamswaarden uit HealthSync 0.20: BMI, vetpercentage, vetvrije massa, lengte en tailleomvang
- Tegels uit HealthSync 0.12+ voor traplopen, trainingstijd, afstand wandelen en hardlopen, VO₂ max en gewicht
- Tijdstip van in slaap vallen en wakker worden
- Voortgangsbalk voor het stappendoel
- Onafhankelijke schaalverdeling voor stappen en calorieën in de activiteitsgrafiek
- Hartslaggrafiek over 24 uur van punt tot punt, op basis van exacte metingen uit `healthsync.get_readings`, met uurstatistieken en Recorder als terugval
- Slaapfasengrafiek, opgebouwd uit `deep_minutes`, `core_minutes`, `rem_minutes` en `awake_minutes`
- Apart tabblad Trainingen met de laatste training en het recente trainingslogboek
- Automatische herkenning van de afzonderlijk benoemde trainingsentiteiten en de bijbehorende pictogrammen uit HealthSync `0.11.0`–`0.20.2`
- Compacte, scrollbare lijst met recente trainingen in plaats van een kaart die helemaal uitklapt
- Optionele schakelaar `show_workouts_tab` in de grafische editor en in YAML
- Compacte, responsieve indeling voor Masonry- en Secties-dashboards
- Nederlandse, Engelse en Russische interface

## Vereisten

- Home Assistant met Recorder-geschiedenis ingeschakeld
- [mannotfood/healthsync](https://github.com/mannotfood/healthsync), minimaal één keer gesynchroniseerd
- HACS voor de aanbevolen installatiemethode

Alle mogelijkheden zijn afgestemd op HealthSync `0.20.2`. Oudere entiteiten blijven
ondersteund, inclusief de verouderde sensor `Recent workouts` van vóór HealthSync `0.11.0`.

## Installeren met HACS als aangepaste repository

[![Open je Home Assistant en voeg deze repository toe aan HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=BrainDeLook&repository=healthsync-dashboard-card&category=plugin)

1. Open **HACS** in Home Assistant.
2. Open het menu met de drie puntjes en kies **Aangepaste repositories**.
3. Voeg `https://github.com/BrainDeLook/healthsync-dashboard-card` toe.
4. Kies **Dashboard** als categorie.
5. Download **HealthSync Dashboard Card** en ververs de browser.

## De kaart toevoegen

Na de installatie staat **HealthSync Dashboard Card** in de kaartkiezer.
Minimale YAML:

```yaml
type: custom:healthsync-dashboard-card
```

De kaart neemt automatisch de taal van Home Assistant over. Staat Home Assistant op
Nederlands, dan is de kaart meteen Nederlands. Je kunt de taal ook vastzetten met
`language: nl` of via de keuzelijst **Taal** in de grafische editor.

In de grafische editor combineert het onderdeel **Meettegels** de zichtbaarheids-
schakelaars met sleepgrepen. Sleep een rij om die meting op dezelfde plek op de
kaart te zetten.

Veelgebruikte opties:

```yaml
type: custom:healthsync-dashboard-card
title: Gezondheid
language: nl # auto, nl, en of ru
device_id: 0123456789abcdef0123456789abcdef # optioneel; schakelt exacte meetgeschiedenis in
days: 7
step_goal: 10000
calorie_goal: 600
show_activity: true
show_sleep: true
show_heart_rate: true
show_workouts_tab: true

# Optionele meettegels (standaard staan ze allemaal aan)
show_steps_metric: true
show_calories_metric: true
show_sleep_metric: true
show_heart_metric: true
show_hrv_metric: true
show_sleep_onset_metric: true
show_sleep_wake_metric: true
show_flights_metric: true
show_exercise_metric: true
show_resting_energy_metric: true
show_distance_metric: true
show_vo2_max_metric: true
show_weight_metric: true
show_resting_heart_rate_metric: true
show_blood_pressure_systolic_metric: true
show_blood_pressure_diastolic_metric: true
show_walking_heart_rate_metric: true
show_heart_rate_recovery_metric: true
show_afib_burden_metric: true
show_blood_oxygen_metric: true
show_respiratory_rate_metric: true
show_body_temperature_metric: true
show_blood_glucose_metric: true
show_body_mass_index_metric: true
show_body_fat_percentage_metric: true
show_lean_body_mass_metric: true
show_height_metric: true
show_waist_circumference_metric: true

# Optionele eigen volgorde; niet genoemde metingen volgen in hun standaardvolgorde
tile_order:
  - heart_rate
  - blood_oxygen
  - steps
  - active_calories
```

De standaard entiteit-ID's van HealthSync worden automatisch gevonden. Hernoemde
entiteiten kies je in de grafische editor of geef je op in YAML:

```yaml
type: custom:healthsync-dashboard-card
entities:
  steps: sensor.healthsync_steps_today
  active_calories: sensor.healthsync_active_calories_today
  heart_rate: sensor.healthsync_heart_rate
  heart_rate_variability: sensor.healthsync_heart_rate_variability
  sleep_duration: sensor.healthsync_sleep_last_night
  sleep_onset: sensor.healthsync_fell_asleep
  sleep_wake: sensor.healthsync_woke_up
  flights_climbed: sensor.healthsync_flights_climbed_today
  exercise_time: sensor.healthsync_exercise_time_today
  resting_energy: sensor.healthsync_resting_energy_today
  distance: sensor.healthsync_walking_running_distance_today
  vo2_max: sensor.healthsync_vo2_max
  weight: sensor.healthsync_weight
  resting_heart_rate: sensor.healthsync_resting_heart_rate
  blood_pressure_systolic: sensor.healthsync_blood_pressure_systolic
  blood_pressure_diastolic: sensor.healthsync_blood_pressure_diastolic
  walking_heart_rate: sensor.healthsync_walking_heart_rate
  heart_rate_recovery: sensor.healthsync_heart_rate_recovery
  afib_burden: sensor.healthsync_afib_burden
  blood_oxygen: sensor.healthsync_blood_oxygen
  respiratory_rate: sensor.healthsync_respiratory_rate
  body_temperature: sensor.healthsync_body_temperature
  blood_glucose: sensor.healthsync_blood_glucose
  body_mass_index: sensor.healthsync_body_mass_index
  body_fat_percentage: sensor.healthsync_body_fat_percentage
  lean_body_mass: sensor.healthsync_lean_body_mass
  height: sensor.healthsync_height
  waist_circumference: sensor.healthsync_waist_circumference
  last_sync: sensor.healthsync_last_sync
  last_workout_type: sensor.healthsync_workouts_last_workout_type
  last_workout_duration: sensor.healthsync_workouts_last_workout_duration
  last_workout_distance: sensor.healthsync_workouts_last_workout_distance
  last_workout_calories: sensor.healthsync_workouts_last_workout_calories
  # Optioneel handmatig overschrijven van de geleidelijk aangemaakte trainingsplekken:
  workout_1: sensor.healthsync_workouts_running_11_08_2026_11_55
```

## Trainingen

HealthSync `0.11.0+` maakt tot tien afzonderlijk benoemde entiteiten aan voor recente
trainingen. De kaart herkent die entiteiten aan hun trainingsattributen, toont per
activiteit het eigen pictogram en opent de bijbehorende entiteit als je erop tikt. De
inmiddels verwijderde attribuutsensor `Recent workouts` van vóór `0.11.0` wordt nog
steeds als terugval geaccepteerd.

Het event-entiteit `Workout completed` van de integratie blijft beschikbaar voor
automatiseringen in Home Assistant en voor de onbeperkte geschiedenis in het Logboek.

De namen van veelvoorkomende trainingstypen worden in het Nederlands weergegeven
(bijvoorbeeld Hardlopen, Fietsen of Krachttraining). Een onbekend type wordt getoond
zoals HealthSync het aanlevert.

## Hartslaggeschiedenis

HealthSync `0.16.0+` bewaart elke oorspronkelijke meting en stelt die beschikbaar via
`healthsync.get_readings`. Kies het HealthSync-apparaat in de grafische editor (of stel
`device_id` in via YAML) om de exacte tijdstempels en waarden van Apple te gebruiken.
Kies je geen apparaat, dan probeert de kaart dit automatisch af te leiden uit de
ingestelde entiteiten. Bij oudere integraties of een niet-beschikbare service valt de
kaart terug op uurstatistieken en daarna op de gewone Recorder-geschiedenis.
Ongeldige plaatshouderwaarden buiten `25–250 bpm` worden genegeerd. Stippellijnen
markeren de delen van het venster van 24 uur vóór de eerste en ná de laatste
beschikbare meting; die tellen niet als gemeten gegevens.

## Slaapgeschiedenis

HealthSync levert de slaapfasen als attributen van `sensor.healthsync_sleep_last_night`.
De kaart vraagt de Recorder-geschiedenis inclusief attributen op en rekent de minuten
per fase om naar uren voor de gestapelde grafiek. Historische fasen zijn alleen
beschikbaar voor records die Home Assistant Recorder heeft bewaard.

## Ontwikkeling

```bash
npm test
npm run check
```

## Licentie

[MIT](LICENSE)
