---
title       : Homework 1
description :
attachments :
  slides_link :

--- type:NormalExercise lang:r xp:100 skills:1 key:a055bddcaf
## Einlesen von Datensätzen in R

Ihnen wurde der Preisverlauf der Henkel Aktie eines Jahres als `CSV-Datei` unter der angegebenen URL zur Verfügung gestellt.

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


Gegeben ist der Datensatz `aktien` (bereits eingelesen). Er besteht aus den Kursdaten der Henkel AG (Frankfurter Börse) und von Exxon Mobile (NYSE). 
Durch die unterschiedlichen Feiertage in Deutschland und den USA fehlen einige Werte im Datensatz. Diese Felder sind mit `NA` gekennzeichnet und sind Gegenstand der vorliegenden Aufgabe.

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



--- type:NormalExercise lang:r xp:100 skills:1 key:552b0d5936
## Missing Values (II)

Gegeben ist wieder der Datensatz `aktien` (bereits eingelesen). Ersetzen Sie nun die NAs im Datensatz durch den gleitenden Durchschnitt über 10 Tage. Nehmen Sie hierfür den Durchschnitt von den 5 vorherigen und 5 folgenden Werten. 

Nützliche R Funktionen:

- `c(1,2,3,4,9)` bildet einen Vektor mit dem Inhalt 1,2,3,4,9.
- `c(1:4,9)` bildet den gleichen Vektor.
- `which(9 == c(1:4,9) )` gibt an, an welcher Stelle der Vektor dem Wert 9 entspricht (also = 5).


*** =instructions

Gegeben ist wieder der Datensatz `aktien` (bereits eingelesen). Ersetzen Sie nun die NAs im Datensatz durch den gleitenden Durchschnitt über 10 Tage. Nehmen Sie hierfür den Durchschnitt von den 5 vorherigen und 5 folgenden Werten. 

Nützliche R Funktionen:

Die NA-Stellen finden Sie über: 
`aktien$Date[is.na(aktien$exxon)]`

Für Henkel analog.


*** =hint
NAs Henkel: "2016-10-31" "2016-10-03"

NAs Exxon: "2016-09-05" "2016-11-24"
*** =pre_exercise_code
```{r}
# Einlesen der Daten
henkel <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/henkel.csv")
exxon <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/exxon.csv")

# Zusammenführung der Daten
library(dplyr)

# Merging der Datensätze
aktienjoin <- full_join(exxon, henkel, by = "Date")

# Verkleinerung der Datensätze
aktien <- select(aktienjoin, Date, exxon = Open.x, henkel = Open.y )
remove(aktienjoin)

# class Datum setzen
aktien$Date <- as.Date(aktien$Date)

# Nach Datum sortieren
aktien <- aktien[order(aktien$Date),]

```

*** =sample_code
```{r}
# Henkel 
# Finde den Index und schreibe ihn in index1 (chronologisch erstes Datum)
index1 <- which(___ == ___)
# durch Durchschnitt ersetzen
aktien$henkel[___] <- ___(c(___[c((index1-___):(index1-___),(index1+___):(index1+___))]))
# Neuer Wert
aktien$henkel[___]


# Finde den Index und schreibe ihn in index2 (chronologisch zweites Datum)

# durch Durchschnitt ersetzen

# Neuer Wert


# Exxon 
# Finde den Index und schreibe ihn in index3 (chronologisch erstes Datum)

# durch Durchschnitt ersetzen

# Neuer Wert



# Finde den Index und schreibe ihn in index4 (chronologisch zweites Datum)

# durch Durchschnitt ersetzen

# Neuer Wert


```

*** =solution
```{r}
# Henkel
# Finde den Index 1 (chronologisch erstes Datum)
index1 <- which(aktien$Date == "2016-10-31")
# durch Durchschnitt ersetzen
aktien$henkel[index1] <- mean(c(aktien$henkel[c((index1-5):(index1-1),(index1+1):(index1+5))]))
# Neuer Wert
aktien$henkel[index1]

# Finde den Index 2 (chronologisch zweites Datum)
index2 <- which(aktien$Date == "2016-10-03")
# durch Durchschnitt ersetzen
aktien$henkel[index2] <- mean(c(aktien$henkel[c((index2-5):(index2-1),(index2+1):(index2+5))]))
# Neuer Wert
aktien$henkel[index2]


# Exxon 
# Finde den Index 3 (chronologisch erstes Datum)
index3 <- which(aktien$Date == "2016-09-05")
# durch Durchschnitt ersetzen
aktien$exxon[index3] <- mean(c(aktien$exxon[c((index3-5):(index3-1),(index3+1):(index3+5))]))
# Neuer Wert
aktien$exxon[index3]

# Finde den Index 4 (chronologisch zweites Datum)
index4 <- which(aktien$Date == "2016-11-24")
# durch Durchschnitt ersetzen
aktien$exxon[index4] <- mean(c(aktien$exxon[c((index4-5):(index4-1),(index4+1):(index4+5))]))
# Neuer Wert
aktien$exxon[index4]
```

*** =sct
```{r}
test_function("which", args = "x", index = 1)
test_function("which", args = "x", index = 2)
test_function("which", args = "x", index = 3)
test_function("which", args = "x", index = 4)
test_object("index1")
test_object("index2")
test_object("index3")
test_object("index4")
test_function("mean", index = 1, args = c("x"))
test_function("mean", index = 2, args = c("x"))
test_function("mean", index = 3, args = c("x"))
test_function("mean", index = 4, args = c("x"))
test_output_contains("aktien$henkel[index1]")
test_output_contains("aktien$henkel[index2]")
test_output_contains("aktien$exxon[index3]")
test_output_contains("aktien$exxon[index4]")
test_error()
success_msg("Sehr gut!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:daf78b06f4
## Missing Values (III)

Ihre Ergebnisse aus der letzten Aufgabe sollen nun zum Vergleich der Methoden geplottet werden. Benutzen Sie die Daten der Henkel Aktie. Vergleichen Sie die Methoden der Ersetzung. Der Datensatz bei dem die NAs durch den gesamten Durchschnitt ersetzt wurden ist `aktien2` (bereits eingelesen). Der, bei dem die NAs durch den gleitenden 10-er Durchschnitt ersetzt wurden, ist `aktien` (bereits eingelesen).


Schauen Sie sich beide Plots an und vergleichen Sie die ersetzten Stellen (Feiertage am "2016-10-31" und "2016-10-03"). 

Hilfe zur `plot()` bekommen Sie wie immer durch `?plot()`.

*** =instructions
- Plotten Sie die Zahlenreihen 
- Beschriften Sie die x-Achse mit "Datum"
- Beschriften Sie die y-Achse mit "Eroeffnungspreis (€)"
*** =hint

- `plot(aktien$Date, ___, type = "___", main = "___", xlab = "___", ylab = "___")`
- 
*** =pre_exercise_code
```{r}
# Einlesen der Daten
henkel <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/henkel.csv")
exxon <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/exxon.csv")

# Zusammenführung der Daten
library(dplyr)

# Merging der Datensätze
aktienjoin <- full_join(exxon, henkel, by = "Date")

# Verkleinerung der Datensätze
aktien <- select(aktienjoin, Date, exxon = Open.x, henkel = Open.y )
remove(aktienjoin)
# class Datum setzen
aktien$Date <- as.Date(aktien$Date)

# Nach Datum sortieren
aktien <- aktien[order(aktien$Date),]
aktien2 <- aktien

# Gesamten durchschnitt setzen
aktien2$henkel[is.na(aktien2$henkel)] <- mean(aktien2$henkel, na.rm = TRUE)

# Gleitender Durchschnitt setzen
index3 <- which(aktien$Date == "2016-10-31")
aktien$henkel[index3] <- mean(c(aktien$henkel[c((index3-5):(index3-1),(index3+1):(index3+5))]))
index4 <- which(aktien$Date == "2016-10-03")
aktien$henkel[index4] <- mean(c(aktien$henkel[c((index4-5):(index4-1),(index4+1):(index4+5))]))

```

*** =sample_code
```{r}
# Plot aktien (gleitender Durchschnitt)

# Plot aktien2 (gesamter Durchschnitt)
```

*** =solution
```{r}
# Plot aktien
plot(aktien$Date, aktien$henkel, type = "l", main = "Henkel Aktie 2016-2017 mit gleitendem 10er-Durchschnitt", xlab = "Datum", ylab = "Eroeffnungspreis (€)")
# Plot aktien2
plot(aktien2$Date, aktien2$henkel, type = "l", main = "Henkel Aktie 2016-2017 mit gesamt Durchschnitt", xlab = "Datum", ylab = "Eroeffnungspreis (€)")

```

*** =sct
```{r}
test_function("plot", index = 1, args = c("x", "y", "type", "xlab", "ylab") )
test_function("plot", index = 2, args = c("x", "y", "type", "xlab", "ylab") )
test_error()
success_msg("Sehr gut!")

```

--- type:NormalExercise lang:r xp:100 skills:1 key:862e75aa1a
## Rendite (I)

Gegeben ist wieder der Datensatz `aktien` (bereits eingelesen). Nun sollen die diskreten Renditen für jeden Tag der Zeitreihe berechnet werden. 

Vektoren können in R einfach elementweise voneinander subtrahiert werden, solange sie die gleiche Dimension haben. 

Tipp: Durch `vektor[5:length(vektor)]` entsteht ein Vektor, der alle Elemente von `vektor` ab dem 5. Element enthält.

Die Formel zur Berechnung der Rendite finden Sie im Skript.



*** =instructions
Berechnen Sie die Rendite für Henkel für jeden Tag des gegebenen Datensatzes. `aktien$henkel` gibt Ihnen den Vektor mit den benötigten Daten.

*** =hint
- Achten Sie darauf, dass die beiden Vektoren die gleiche Länge haben müssen.
- Die Länge eines Vektors bekommen Sie durch `length(vektor)`.
*** =pre_exercise_code
```{r}
# Einlesen der Daten
aktien <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/henkel.csv")


# Zusammenführung der Daten
library(dplyr)

# Verkleinerung der Datensätze
aktien <- select(aktien, Date, henkel = Open )

# class Datum setzen
aktien$Date <- as.Date(aktien$Date)

# Nach Datum sortieren
aktien <- aktien[order(aktien$Date),]


```

*** =sample_code
```{r}
# Erstellen Sie einen vektor mit den Einträgen aus aktien$henkel. Lassen Sie den letzten Eintrag weg.
x_tminus1 <- 
# Lassen Sie den ersten Eintrag weg, da die Rendite erst ab 2. Tag berechenbar
x_t <- 
# Berechnung der Rendite
renditeH <- 
# Geben Sie hier die berechnete Rendite in der Konsole aus

```

*** =solution
```{r}
# Erstellen Sie einen vektor mit den Einträgen aus aktien$henkel. Lasse den letzten Eintrag weg.
x_tminus1 <- aktien$henkel[1:(length(aktien$henkel)-1)]

# Lassen Sie den ersten Eintrag weg, da die Rendite erst ab 2. Tag berechenbar ist.
x_t <- aktien$henkel[2:length(aktien$henkel)]

# Berechnung der Rendite
renditeH <- (x_t - x_tminus1) / x_tminus1

# Geben Sie hier die berechnete Rendite in der Konsole aus
renditeH

```

*** =sct
```{r}
test_object("x_tminus1")
test_object("x_t")
test_object("renditeH")
test_output_contains("renditeH")
test_error()
success_msg("Sehr gut!")

```



--- type:NormalExercise lang:r xp:100 skills:1 key:f014685cd8
## Berechnungen in R: Rendite (II)

Der Datensatz liegt in `aktien`. Nun sollen die stetigen (logarithmischen) Renditen für jeden Tag berechnet werden.

Tipps:

Vektoren können in R einfach elementweise voneinander subtrahiert werden, solange sie die gleiche Dimension haben. Durch `vektor[5:length(vektor)]` entsteht ein Vektor, der alle Elemente von `vektor` ab dem 5. Element enthält.

Den natürlichen Logarithmus können Sie in R einfach mit `log(x)` bestimmen. Hierbei kann `x` auch ein Vektor sein, der Logarithmus wird in dem Fall elementeweise ausgeführt.

Die Formel zur Berechnung der log-Rendite finden Sie im Skript.

*** =instructions
Berechnen Sie die log-Rendite für Henkel für jeden Tag des gegebenen Datensatzes. `aktien$henkel` gibt Ihnen den Vektor mit den benötigten Daten.

*** =hint

- Achten Sie darauf, dass die beiden Vektoren die gleiche Länge haben müssen.
- Die Länge eines Vektors bekommen Sie durch `length(vektor)`.

*** =pre_exercise_code
```{r}
# Einlesen der Daten
aktien <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/henkel.csv")

# Zusammenführung der Daten
library(dplyr)

# Verkleinerung der Datensätze
aktien <- select(aktien, Date, henkel = Open )

# class Datum setzen
aktien$Date <- as.Date(aktien$Date)

# Nach Datum sortieren
aktien <- aktien[order(aktien$Date),]


```

*** =sample_code
```{r}
# Erstellen Sie einen vektor mit den Einträgen aus aktien$henkel. Lassen Sie den letzten Eintrag weg.
x_tminus1 <- 
# Lassen Sie den ersten Eintrag weg, da die Rendite erst ab 2. Tag berechenbar ist.
x_t <- 
# Berechnung der Rendite
logRenditeH <- 
# Geben Sie hier die berechnete log-Rendite in der Konsole aus

```

*** =solution
```{r}
# Erstellen Sie einen vektor mit den Einträgen aus aktien$henkel. Lassen Sie den letzten Eintrag weg.
x_tminus1 <- aktien$henkel[1:(length(aktien$henkel)-1)]
# Lassen Sie den ersten Eintrag weg, da die Rendite erst ab 2. Tag berechenbar ist.
x_t <- aktien$henkel[2:length(aktien$henkel)]
# Berechnung der Rendite
logRenditeH <- log(x_t) - log(x_tminus1)
# Geben Sie hier die berechnete Rendite in der Konsole aus
logRenditeH

```

*** =sct
```{r}
test_object("x_tminus1")
test_object("x_t")
test_object("logRenditeH")
test_output_contains("logRenditeH")
test_error()
success_msg("Super!")

```





--- type:NormalExercise lang:r xp:100 skills:1 key:306bec6cc9
## Berechnung des gleitenden Durchschnitts

Die benötigten Daten sind im Objekt `aktien` bereits eingelesen.
Benutzen sie für diese Aufgabe `cumsum(vektor)`. Was der Befehlt macht können Sie durch Ausprobieren in der Konsole oder durch `?cumsum()` herausfinden.


*** =instructions

Berechnen Sie den gleitenden 10-er Durchschnitt von `aktien$henkel` und schreiben Sie ihn in `rsum`.

*** =hint

*** =pre_exercise_code
```{r}
aktien <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/henkel.csv")

# Zusammenführung der Daten
library(dplyr)

# Verkleinerung der Datensätze
aktien <- select(aktien, Date, henkel = Open )

# class Datum setzen
aktien$Date <- as.Date(aktien$Date)

# Nach Datum sortieren
aktien <- aktien[order(aktien$Date),]

```

*** =sample_code
```{r}
n <- 10
# Berechnen Sie den gleitenden Durchschnitt und speichern Sie ihr Ergebnis unter "rsum".


rsum <-


```

*** =solution
```{r}
n <- 10
cx <- cumsum(aktien$henkel)
rsum <- (cx[(n+1):length(aktien$henkel)] - cx[1:(length(aktien$henkel) - n)]) / n	

```

*** =sct
```{r}
test_object("rsum")
test_error()
```



--- type:NormalExercise lang:r xp:100 skills:1 key:1d68560355
## Analyse der Daten
Im Datensatz `aktien` (bereits eingelesen) ist der Eröffnungspreis und die jeweilige Tagesrendite der Exxon Aktie enthalten. 

Wir betrachten die Volatilität der Zeitreihe. Bei dem Plot der Eröffnungspreise (Plot 1) kann man nur grob schätzen, wo die Volatilität am stärksten ist. Im Plot 2 sehen Sie die Rendite der Zeitreihe geplottet. Hier kann man das Ergebnis schon etwas besser heraus lesen.

Wo ist die Volatilität am höchsten? Sollte die Lösung nicht eindeutig sein, können Sie auch die Ergebnisse des `var()` Befehls für die angegebenen Zeiträume vergleichen.



*** =instructions
- Anfang August 2016
- Ende September 2016

*** =hint

*** =pre_exercise_code
```{r}
library(dplyr)
# Einlesen der Daten 
aktien <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/exxon.csv")

# Datum als class Date initialisieren
aktien$Date <- as.Date(aktien$Date)

# sortieren Datensatz nach Datum
aktien <- aktien[order(aktien$Date),]

# Verkleinerung der Datensätze
aktien <- select(aktien, Date, Open)
plot(aktien$Date, aktien$Open, type = "l", main = "Exxon Aktie 2016-2017", xlab = "Datum", ylab = "Eröffnungspreis ($)")

# Funktion zur Berechnung der Rendite
rendite <- function(zeitreihe){ 
  r <- zeitreihe[1:(length(zeitreihe)-1)];  
  ren <- zeitreihe[2:length(zeitreihe)];
  ren <- (ren - r) / r;
  return(ren)     
}

# Rendite für Eröffnungspreise berechnen
exRen <- rendite(aktien$Open) 

# Rendite zum Datensatz hizufügen, vorne 0
exRen <- c(0,exRen)
aktien[ , "Rendite"] <- exRen

plot(aktien$Date, aktien$Rendite, type = "l", main = "Exxon Aktie 2016-2017", xlab = "Datum", ylab = "Rendite")

```

*** =sct
```{r}
msg_bad <- "Das stimmt nicht! Nutze die Konsole und berechne das Datum ganz genau."
msg_success <- "Richtig!"
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_bad,  msg_success))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:72c5438fee
## Histogramm

Der Datensatz `aktien` mit den berechneten Renditen für die  Exxon Aktie ist bereits eingelesen. Erstellen Sie ein Histogramm über die Verteilung der Renditen. 

Nutzen Sie die Funktion zum Erstellen eines Histogramms ist `hist(x,...)`.


*** =instructions
Nutzen Sie `?hist()` um mehr über die Anwendung der Funktion zu erfahren.
Das Histogramm sollte enthalten: 

- `breaks` um die dicke der Balken anzupassen.
- die Überschrift "Verteilung der Renditen Exxon Aktie".
- `xlab` und `ylab` zur Beschriftung der Achsen, mit "Renditen" und "Haeufigkeit".


*** =hint

- An die x-Werte kommen Sie über `aktien$Rendite`
- Denken Sie daran, die Beschriftungen in Anführungszeichen zu setzen.


*** =pre_exercise_code
```{r}
# Einlesen der Daten
aktien <- read.csv("https://www.uni-duesseldorf.de/redaktion/fileadmin/redaktion/Fakultaeten/Wirtschaftswissenschaftliche_Fakultaet/Statistik/Kurse/BW_09/exxon.csv")

# Datum als class Date initialisieren
aktien$Date <- as.Date(aktien$Date)

# sortieren Datensatz nach Datum
aktien <- aktien[order(aktien$Date),]

# Funktion zur Berechnung der Rendite
rendite <- function(zeitreihe){ 
  r <- zeitreihe[1:(length(zeitreihe)-1)];  
  ren <- zeitreihe[2:length(zeitreihe)];
  ren <- (ren - r) / r;
  return(ren)     
}

# Rendite von Exxon Aktien
renAktien <- rendite(aktien$Open)

# Rendite zum Datensatz hinzufügen
renAktien <- c(0,renAktien)
aktien[ , "Rendite"] <- renAktien

```

*** =sample_code
```{r}
# Erstellen Sie ein Histogramm und setzen Sie breaks = 25.

# Erstellen Sie das Histogramm mit breaks = 50.

```

*** =solution
```{r}
# Erstellen Sie ein Histogramm und setzen Sie breaks = 25.
hist(aktien$Rendite, breaks = 25, main = "Verteilung der Renditen Exxon Aktie", xlab = "Renditen", ylab = "Haeufigkeit")

# Erstellen Sie das Histogramm mit breaks = 50.
hist(aktien$Rendite, breaks = 50, main = "Verteilung der Renditen Exxon Aktie", xlab = "Renditen", ylab = "Haeufigkeit")

```

*** =sct
```{r}
test_function("hist", args = c("x", "breaks", "main", "xlab", "ylab"), index = 1) 
test_function("hist", args = c("x", "breaks", "main", "xlab", "ylab"), index = 2) 
test_error()
success_msg("Gratulation! Sie haben die letzte Aufgabe gemeistert.")
```