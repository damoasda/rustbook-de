## Wertlisten in Vektoren ablegen

Der erste Kollektionstyp, den wir betrachten werden, ist `Vec<T>`, auch bekannt
als *Vektor*. Vektoren ermöglichen es dir, mehr als einen Wert in einer
einzigen Datenstruktur zu speichern und alle Werte nebeneinander im Speicher
abzulegen. Vektoren können nur Werte desselben Typs speichern. Sie sind nützlich,
wenn du eine Liste von Einträgen hast, z.B. die Textzeilen einer Datei oder die
Preise der Artikel in einem Einkaufswagen.

### Erstellen eines neuen Vektors

Um einen neuen, leeren Vektor zu erstellen, rufen wir die Funktion `Vec::new`
auf, wie in Codeblock 8-1 gezeigt.

```rust
let v: Vec<i32> = Vec::new();
```

<span class="caption">Codeblock 8-1: Erstellen eines neuen, leeren Vektors zur
Aufnahme von Werten des Typs `i32`</span>

Beachte, dass wir hier eine Typ-Annotation hinzugefügt haben. Da wir keine
Werte in diesen Vektor einfügen, weiß Rust nicht, welche Art von Elementen wir
zu speichern beabsichtigen. Dies ist ein wichtiger Punkt. Vektoren werden mit
Hilfe generischer Typen implementiert; wie du eigene generische Typen verwenden
kannst, wird in Kapitel 10 behandelt. Für den Moment sollst du wissen, dass der
von der Standardbibliothek bereitgestellte Typ `Vec<T>` jeden Typ enthalten
kann. Wenn wir einen Vektor zu einem bestimmten Typ erstellen, wird der Typ
in spitzen Klammern angegeben. In Codeblock 8-1 haben wir Rust gesagt, dass der
Vektor `Vec<T>` in `v` Elemente des Typs `i32` enthalten wird.

Meistens wird man ein `Vec<T>` mit Anfangswerten erstellen und Rust wird den
Typ des Wertes, den man speichern will, ableiten, sodass man diese
Typ-Annotation nur selten benötigt. Rust bietet praktischerweise das Makro
`vec!`, das einen neuen Vektor erzeugt, der die von dir angegebenen Werte
enthält. Codeblock 8-2 erzeugt einen neuen `Vec<i32>`, der die Werte `1`, `2`
und `3` enthält. Als Integer-Typ wird `i32` verwendet, weil das der
Standard-Integer-Typ ist, wie wir im Abschnitt [„Datentypen“][data-types] in
Kapitel 3 besprochen haben.

```rust
let v = vec![1, 2, 3];
```

<span class="caption">Codeblock 8-2: Erstellen eines neuen Vektors mit
Werten</span>

Da wir initiale `i32`-Werte angegeben haben, kann Rust daraus schließen, dass
`v` den Typ `Vec<i32>` hat, und die Typ-Annotation ist nicht notwendig. Als
Nächstes werden wir uns ansehen, wie man einen Vektor modifiziert.

### Aktualisieren eines Vektors

Um einen Vektor zu erstellen und ihm dann Elemente hinzuzufügen, können wir die
Methode `push` verwenden, wie in Codeblock 8-3 zu sehen ist.

```rust
let mut v = Vec::new();

v.push(5);
v.push(6);
v.push(7);
v.push(8);
```

<span class="caption">Codeblock 8-3: Verwenden der Methode `push` zum
Hinzufügen von Werten zu einem Vektor</span>

Wie bei jeder Variablen müssen wir, wenn wir ihren Wert ändern wollen, sie mit
dem Schlüsselwort `mut` als veränderbar markieren, wie in Kapitel 3
besprochen. Die Zahlen, die wir darin platzieren, sind alle vom Typ `i32`, und
Rust leitet dies aus den Daten ab, sodass wir die Annotation `Vec<i32>` nicht
benötigen.

### Elemente aus Vektoren lesen

Es gibt zwei Möglichkeiten, einen in einem Vektor gespeicherten Wert zu
referenzieren. In den Beispielen haben wir zur besseren Lesbarkeit die
Werttypen, die von den Funktionen zurückgegeben werden, mit angegeben.

Codeblock 8-4 zeigt beide Zugriffsmethoden auf einen Wert in einem Vektor,
mittels Indexierungssyntax und der Methode `get`.

```rust
let v = vec![1, 2, 3, 4, 5];

let third: &i32 = &v[2];
println!("Das dritte Element ist {third}");

let third: Option<&i32> = v.get(2);
match third {
    Some(third) => println!("Das dritte Element ist {third}"),
    None => println!("Es gibt kein drittes Element."),
}
```

<span class="caption">Codeblock 8-4: Verwenden der Indexierungssyntax und der
Methode `get` für den Zugriff auf ein Element in einem Vektor</span>

Beachte hier einige Details. Wir verwenden den Indexwert `2`, um das dritte
Element zu erhalten, da Vektoren mit Zahlen beginnend bei null indiziert
werden. Mit `&` und `[]` erhalten wir eine Referenz auf das Element mit dem
Indexwert. Wenn wir die Methode `get` mit dem Index als Argument verwenden,
erhalten wir eine `Option<&T>`, die wir mit `match` verwenden können.

Der Grund, warum Rust diese beiden Möglichkeiten, auf ein Element zu
referenzieren, bietet ist, dass du wählen kannst, wie sich das Programm
verhält, wenn du versuchst, einen Indexwert außerhalb des Bereichs der
vorhandenen Elemente zu verwenden. Als Beispiel wollen wir sehen, was ein
Programm tut, wenn wir bei einem Vektor mit fünf Elementen versuchen, auf ein
Element mit Index 100 zuzugreifen, wie in Codeblock 8-5 zu sehen ist.

```rust,should_panic,panics
let v = vec![1, 2, 3, 4, 5];

let does_not_exist = &v[100];
let does_not_exist = v.get(100);
```

<span class="caption">Codeblock 8-5: Versuch, auf das Element mit Index 100 in
einem Vektor zuzugreifen, der fünf Elemente enthält</span>

Wenn wir diesen Code ausführen, wird die erste `[]` Variante das Programm abbrechen
lassen, weil es auf ein nicht existierendes Element verweist. Diese Methode
wird vorzugsweise verwendet, wenn du dein Programm abstürzen lassen möchtest,
wenn versucht wird, auf ein Element hinter dem Ende des Vektors zuzugreifen.

Wenn der Methode `get` ein Index außerhalb des Vektors übergeben wird, gibt sie
`None` zurück, ohne abzubrechen. Du würdest diese Methode verwenden, wenn der
Zugriff auf ein Element außerhalb des Bereichs des Vektors unter normalen
Umständen gelegentlich vorkommt. Dein Code wird dann eine Logik haben, die mit
`Some(&element)` und `None` umgehen kann, wie in Kapitel 6 besprochen. Der
Index könnte zum Beispiel von einer Person stammen, die eine Zahl eingibt. Wenn
sie versehentlich eine zu große Zahl eingibt und das Programm einen `None`-Wert
erhält, kannst du dem Benutzer mitteilen, wie viele Elemente sich aktuell im
Vektor befinden und ihm eine weitere Chance geben, einen gültigen Wert
einzugeben. Das wäre benutzerfreundlicher, als das Programm wegen eines
Tippfehlers abstürzen zu lassen!

Wenn das Programm über eine gültige Referenz verfügt, stellt der
Ausleihenprüfer mittels Eigentümerschafts- und Ausleihregeln (siehe Kapitel 4)
sicher, dass diese Referenz und alle anderen Referenzen auf den Inhalt des
Vektors gültig bleiben. Erinnere dich an die Regel, die besagt, dass du keine
veränderbaren und unveränderbaren Referenzen im gleichen Gültigkeitsbereich
haben kannst. Diese Regel trifft in Codeblock 8-6 zu, wo wir eine
unveränderbare Referenz auf das erste Element in einem Vektor halten und
versuchen, am Ende ein Element hinzuzufügen. Das wird nicht funktionieren, wenn
wir später in der Funktion versuchen auch auf dieses Element zuzugreifen:

```rust,does_not_compile
let mut v = vec![1, 2, 3, 4, 5];

let first = &v[0];

v.push(6);

println!("Das erste Element ist: {first}");
```

<span class="caption">Codeblock 8-6: Versuch, ein Element zu einem Vektor
hinzuzufügen, während eine Referenz auf ein Element gehalten wird</span>

Das Kompilieren dieses Codes führt zu folgendem Fehler:

```console
$ cargo run
   Compiling collections v0.1.0 (file:///projects/collections)
error[E0502]: cannot borrow `v` as mutable because it is also borrowed as immutable
 --> src/main.rs:6:5
  |
4 |     let first = &v[0];
  |                  - immutable borrow occurs here
5 | 
6 |     v.push(6);
  |     ^^^^^^^^^ mutable borrow occurs here
7 | 
8 |     println!("Das erste Element ist: {first}");
  |                                      ------- immutable borrow later used here

For more information about this error, try `rustc --explain E0502`.
error: could not compile `collections` (bin "collections") due to 1 previous error
```

Der Code in Codeblock 8-6 sieht so aus, als könnte er funktionieren: Warum
sollte sich eine Referenz auf das erste Element darum kümmern, was sich am
Ende des Vektors ändert? Dieser Fehler ist in der Funktionsweise von Vektoren
begründet: Weil Vektoren die Werte nebeneinander im Speicher ablegen, könnte
das Hinzufügen eines neuen Elements am Ende des Vektors die Allokation neuen
Speichers und das Kopieren der alten Elemente an die neue Stelle erfordern,
wenn nicht genügend Platz vorhanden ist, um alle Elemente nebeneinander an der
aktuellen Stelle des Vektors zu platzieren. In diesem Fall würde die Referenz
auf das erste Element auf einen freigegebenen Speicherplatz verweisen. Die
Ausleihregeln verhindern, dass Programme in diese Situation geraten.

> Anmerkung: Weitere Einzelheiten zu den Implementierungsdetails des Typs
> `Vec<T>` findest du in [„Das Rustonomicon“][nomicon1].

### Iterieren über die Werte in einem Vektor

Um auf die Elemente eines Vektors der Reihe nach zuzugreifen, können wir über
alle Elemente iterieren, anstatt Indizes zu verwenden, um auf jeweils ein
Element zur gleichen Zeit zuzugreifen. Codeblock 8-7 zeigt, wie man eine
`for`-Schleife verwendet, um unveränderbare Referenzen auf die Elemente eines
Vektors von `i32`-Werten zu erhalten und diese auszugeben.

```rust
let v = vec![100, 32, 57];
for i in &v {
    println!("{i}");
}
```

<span class="caption">Codeblock 8-7: Ausgeben aller Elemente eines Vektors
durch Iterieren über die Elemente mittels `for`-Schleife</span>

Wir können auch über veränderbare Referenzen der Elemente eines veränderbaren
Vektors iterieren, um Änderungen an allen Elementen vorzunehmen. Die
`for`-Schleife in Codeblock 8-8 addiert zu jedem Element `50`.

```rust
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;
}
```

<span class="caption">Codeblock 8-8: Iterieren über veränderbare Referenzen
der Elemente eines Vektors</span>

Um den Wert, auf den sich die veränderbare Referenz bezieht, zu ändern, müssen
wir den Dereferenzierungsoperator (`*`) verwenden, um an den Wert in `i` zu
kommen, bevor wir den Operator `+=` verwenden können. Wir werden mehr über den
Dereferenzierungsoperator im Abschnitt [„Dem Zeiger zum Wert folgen“][deref] in
Kapitel 15 sprechen.

Die Iteration über einen Vektor, ob unveränderbar oder veränderbar, ist
aufgrund der Regeln des Ausleihenprüfers sicher. Wenn wir versuchen würden,
Elemente in den `for`-Schleifenrümpfen in Codeblock 8-7 und Codeblock 8-8
einzufügen oder zu entfernen, würden wir einen Compilerfehler erhalten, ähnlich
dem, den wir mit dem Code in Codeblock 8-6 erhalten haben. Die Referenz auf den
Vektor, den die `for`-Schleife enthält, verhindert eine gleichzeitige Änderung
des gesamten Vektors.

### Verwenden einer Aufzählung zum Speichern mehrerer Typen

Vektoren können nur Werte desselben Typs speichern. Das kann unbequem sein; es
gibt definitiv Anwendungsfälle, in denen es notwendig ist, eine Liste von
Einträgen unterschiedlicher Typen zu speichern. Glücklicherweise werden die
Varianten einer Aufzählung unter dem gleichen Aufzählungstyp definiert. Wenn
wir also Elemente eines anderen Typs in einem Vektor speichern wollen, können
wir eine Aufzählung definieren und verwenden!

Angenommen, wir möchten Werte aus einer Zeile einer Tabellenkalkulationstabelle
erhalten, in der einige Spalten der Zeile ganze Zahlen, Fließkommazahlen und
Zeichenketten enthalten. Wir können eine Aufzählung definieren, deren Varianten
die verschiedenen Werttypen enthalten, und alle Aufzählungsvarianten werden als
derselbe Typ angesehen: Der Typ der Aufzählung. Dann können wir einen Vektor
erstellen, der diese Aufzählung und damit letztlich verschiedene Typen enthält.
Wir haben dies in Codeblock 8-9 demonstriert.

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blau")),
    SpreadsheetCell::Float(10.12),
];
```

<span class="caption">Codeblock 8-9: Definieren eines `enum`, um Werte
verschiedener Typen in einem Vektor zu speichern</span>

Rust muss wissen, welche Typen zur Kompilierzeit im Vektor enthalten sein
werden, damit es genau weiß, wie viel Speicherplatz im Haldenspeicher
benötigt wird, um alle Elemente zu speichern. Wir müssen auch eindeutig
festlegen, welche Typen in diesem Vektor zulässig sind. Wenn Rust einen Vektor
mit beliebigen Typen zuließe, bestünde die Möglichkeit, dass einer oder mehrere
Typen Fehler bei den an den Elementen des Vektors durchgeführten Operationen
verursachen würden. Das Verwenden einer Aufzählung zusammen mit einem
`match`-Ausdruck bedeutet, dass Rust zur Kompilierzeit sicherstellt, dass jeder
mögliche Fall behandelt wird, wie in Kapitel 6 besprochen.

Wenn du nicht weißt, welche Typen ein Programm zur Laufzeit in einem Vektor
speichern kann, funktioniert der Aufzählungsansatz nicht. Stattdessen kannst du
ein Merkmalsobjekt (trait object) verwenden, das wir in Kapitel 17 behandeln
werden.

Nachdem wir nun einige der gängigsten Methoden zur Verwendung von Vektoren
besprochen haben, solltest du dir unbedingt die [API-Dokumentation][vec-api] zu
den vielen nützlichen Methoden ansehen, die die Standardbibliothek für `Vec<T>`
mitbringt. Zum Beispiel gibt es zusätzlich zu `push` die Methode `pop`, die das
letzte Element entfernt und zurückgibt.

### Beim Aufräumen eines Vektors werden seine Elemente aufgeräumt

Wie bei jeder anderen Struktur wird ein Vektor freigegeben, wenn er den
Gültigkeitsbereich verlässt, wie in Codeblock 8-10 kommentiert wird.

```rust
{
    let v = vec![1, 2, 3, 4];

    // mache etwas mit v
} // <- v verlässt den Gültigkeitsbereich und wird hier freigegeben
```

<span class="caption">Codeblock 8-10: Zeigt, wo der Vektor und seine Elemente
aufgeräumt werden</span>

Wenn der Vektor aufgeräumt wird, wird auch sein gesamter Inhalt aufgeräumt,
d.h. die ganzen Zahlen, die er enthält, werden beseitigt. Der Ausleihenprüfer
stellt sicher, dass alle Referenzen auf den Inhalt eines Vektors nur verwendet
werden, solange der Vektor selbst gültig ist.

Lass uns zum nächsten Kollektionstyp übergehen: `String`!

[data-types]: ch03-02-data-types.html
[nomicon1]: https://doc.rust-lang.org/nomicon/vec.html
[vec-api]: https://doc.rust-lang.org/std/vec/struct.Vec.html
[deref]: ch15-02-deref.html#dem-zeiger-zum-wert-folgen
