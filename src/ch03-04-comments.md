## Kommentare

Alle Programmierer bemühen sich, ihren Code leicht verständlich zu machen, aber
manchmal sind zusätzliche Erklärungen angebracht. In solchen Fällen versehen
Entwickler den Quellcode mit _Kommentaren_, welche der Compiler ignoriert
und für andere Entwickler nützlich sein können.

Dies ist ein einfacher Kommentar:

```rust
// Hallo Welt
```

In Rust beginnt ein gewöhnlicher Kommentar mit zwei Schrägstrichen; der
Kommentar reicht dann bis zum Ende der Zeile. Für Kommentare, die über eine
einzelne Zeile hinausgehen, musst du bei jedem Zeilenanfang `//` angeben:

```rust
// Hier passiert etwas kompliziertes, so komplex dass wir
// mehrere Kommentarzeilen brauchen! Puh! Hoffentlich erklärt
// dieser Kommentar, was hier passiert.
```

Kommentare können auch am Ende einer Codezeile stehen:

<span class="filename">Dateiname: src/main.rs</span>

```rust
fn main() {
    let lucky_number = 7; // Heute habe ich Glück
}
```

Gängiger ist jedoch die Schreibweise mit dem Kommentar über der Codezeile, die
er beschreibt:

<span class="filename">Dateiname: src/main.rs</span>

```rust
fn main() {
    // Heute habe ich Glück
    let lucky_number = 7;
}
```

Rust kennt noch eine weitere Kommentarart, nämlich Dokumentationskommentare,
die wir im Abschnitt [„Kisten (crate) auf crates.io
veröffentlichen“][publishing] in Kapitel 14 besprechen werden.

[publishing]: ch14-02-publishing-to-crates-io.html
