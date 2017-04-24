---
title       : Homework 1
description :
attachments :
  slides_link :

--- type:NormalExercise lang:r xp:100 skills:1 key:a055bddcaf
## 1. Einlesen von Datensätzen


Ihnen wurde der Preisverlauf der Henkel Aktie eines Jahres unter der angegebenen URL hinterlegt. Lesen Sie den Datensatz als `CSV-Datei` ein. 

(Quelle: de.finance.yahoo.com)


*** =instructions

- Lesen Sie die Daten ein und speichern Sie diese in `henkel`.
- Schauen Sie sich die Daten in der Konsole an.

*** =hint
- Nutzen Sie die `read.csv()` Funktion.
- Setzen Sie den Pfad als Argument der Funktion `read.csv()` in "Anführungszeichen".
- Zum ausgeben in der Konsole reicht `henkel`.

*** =pre_exercise_code
```{r}
```

*** =sample_code
```{r}

# die Datei liegt in https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/henkel.csv


# Geben Sie die eingelesenen Daten in der Konsole aus

```

*** =solution
```{r}
# der Pfad der Datei ist 
henkel <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/henkel.csv")

# Schauen Sie sich die Daten in der Konsole an
henkel

```

*** =sct
```{r}
test_function("read.csv", args = c("file"))
test_object("henkel")
test_output_contains("henkel")
test_error()
success_msg("Sehr gut!")

```



--- type:NormalExercise lang:r xp:100 skills:1 key:122133c624
## Missing Values (I)

Gegeben ist der Datensatz `aktien`. Er besteht aus den Daten von Henkel (Frankfurter Börse) und von Exxon Mobile (NYSE). Länderspezifische Feiertage sind mit `NA` gekennzeichnet. Gesucht ist eine geeiegnete Möglichkeit, zur Ersetzung diese Felder durch passende Werte.

(Quelle der Daten: de.finance.yahoo.com)

*** =instructions
Ersetzen Sie die NA Felder in dem Datensatz durch:

- den Durchschnitt der gesamten Zeitreihe. Hierfür können Sie die `mean()`-Funktion nutzen.
- in den [ ]-Klammern hinter der Variable stehen die Auswahlbedingungen. Beispielsweise: `spalte[is.na(spalte)]` gibt nur die Felder aus `spalte` zurück, in denen NA steht.

Nehmen Sie hierbei jeweils die Spalten henkel und exxon.
*** =hint
- `na.rm = TRUE` entfernt die NA Felder, zur Berechnung des Durchschnitts.
- `is.na(daten)` findet die NAs in den daten.
-  Zur Ausgabe in der Konsole können Sie auch `print(Objekt)` nutzen

*** =pre_exercise_code
```{r}
# Einlesen der Daten
exxon <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/exxon.csv")
henkel <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/henkel.csv")

# Zusammenführung der Daten
library(dplyr)
# Merging der Datensätze
aktienjoin <- full_join(exxon, henkel, by = "Date")

# Verkleinerung der Datensätze
aktien <- select(aktienjoin, Date, exxon = Open.x, henkel = Open.y )

# class Datum setzen
aktien$Date <- as.Date(aktien$Date)
remove(aktienjoin)

# Nach Datum sortieren
aktien <- aktien[order(aktien$Date),]

# class Datum setzen
aktien$Date <- as.Date(aktien$Date)

```

*** =sample_code
```{r}
# Schaue, an welchen Tagen sich NAs befinden
aktien$Date[is.na(aktien$exxon)]
aktien$Date[is.na(aktien$henkel)]

# Ersetzen Sie NA in der Spalte 'henkel' durch den Durchschnitt der Spalte
aktien$___[is.na(___)] <- ___(___, na.rm = TRUE)

# Ersetzen Sie NA in der Spalte 'exxon' durch den Durchschnitt der Spalte


# Geben Sie die geänderten Spalten in der Konsole aus


```

*** =solution
```{r}
# Schaue, an welchen Tagen sich NAs befinden
aktien$Date[is.na(aktien$exxon)]
aktien$Date[is.na(aktien$henkel)]

# Ersetzen Sie NA in der Spalte 'henkel' durch den Durchschnitt der Spalte
aktien$henkel[is.na(aktien$henkel)] <- mean(aktien$henkel, na.rm = TRUE)

# Ersetzen Sie NA in der Spalte 'exxon' durch den Durchschnitt der Spalte
aktien$exxon[is.na(aktien$exxon)] <- mean(aktien$exxon, na.rm = TRUE)

# Geben Sie die geänderten Spalten in der Konsole aus
aktien$henkel
aktien$exxon

```

*** =sct
```{r}

test_function("mean", index = 1, args = c("x", "na.rm"))
test_function("mean", index = 2, args = c("x", "na.rm"))
test_object("aktien")
test_output_contains("aktien$henkel")
test_output_contains("aktien$exxon")
test_error()
success_msg("Sehr gut!")

```
