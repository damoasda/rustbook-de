# Allgemeine Kollektionen

Die Standardbibliothek von Rust enthält eine Reihe sehr nützlicher
Datenstrukturen, die _Kollektionen_ (collections) genannt werden. Die meisten
anderen Datentypen repräsentieren einen bestimmten Wert, aber Kollektionen
können mehrere Werte enthalten. Im Gegensatz zu den eingebauten Array- und
Tupel-Typen werden die Daten, auf die diese Kollektionen zeigen, im dynamischen
Speicher abgelegt. Somit muss die Datenmenge zum Kompilierzeitpunkt nicht
bekannt sein und kann während der Programmausführung wachsen oder schrumpfen.
Jede Kollektionsart hat unterschiedliche Fähigkeiten und Kosten, und die
Auswahl einer für deine aktuelle Situation geeigneten Kollektion ist eine
Fähigkeit, die du im Laufe der Zeit entwickeln wirst. In diesem Kapitel
besprechen wir drei Kollektionen, die sehr häufig in Rust-Programmen verwendet
werden:

- Ein _Vektor_ erlaubt es dir, eine variable Anzahl von Werten nebeneinander zu
  speichern.
- Eine _Zeichenkette_ ist eine Kollektion von Zeichen. Wir haben den Typ
  `String` bereits kennengelernt, aber in diesem Kapitel werden wir ausführlich
  darauf eingehen.
- Eine _Hashtabelle_ (hash map) erlaubt es dir, einen Wert mit einem
  bestimmten Schlüssel zu assoziieren. Es ist eine spezielle Implementierung
  der allgemeineren Datenstruktur, die _assoziatives Datenfeld_ (map) genannt
  wird.

Informationen über weitere Kollektionsarten, die von der Standardbibliothek
bereitgestellt werden, findest du in [der Dokumentation][collections].

Wir werden erörtern, wie Vektoren, Zeichenketten und Hashtabellen erstellt und
aktualisiert werden und was jede einzelne besonders macht.

[collections]: https://doc.rust-lang.org/std/collections/index.html
