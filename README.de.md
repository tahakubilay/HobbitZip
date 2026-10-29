<div align="center">

# HobbitZip
### Forschung zur verlustfreien Kompression mit messbarer Leistung

<img src="./assets/hero.svg" alt="HobbitZip — Forschung zur verlustfreien Kompression" width="100%" />

**Kompressionsforschung** · **Performance Engineering** · **Reproduzierbare Evaluation**

[English](./README.md) · [Deutsch](./README.de.md) · [Türkçe](./README.tr.md)

</div>

---

## Überblick

**HobbitZip** ist ein experimentelles Engineering-Projekt für verlustfreie Datenkompression. Es untersucht, wie Datenstruktur, reversible Transformationen, Kodierungsstrategien und Rechenbeschränkungen die Kompressionseffizienz beeinflussen.

Das Ziel ist nicht die Behauptung einer generellen Überlegenheit gegenüber etablierten Codecs. Im Mittelpunkt steht die messbare Beziehung zwischen **komprimierter Größe, Kodiergeschwindigkeit, Dekodiergeschwindigkeit, Speicherbedarf und Korrektheit**.

## Zentrale technische Fragen

- Welche Datenmuster enthalten nutzbare Redundanz?
- Wann verbessert eine reversible Vorverarbeitung das Endergebnis?
- Welche CPU- und Speicherkosten verursachen bessere Kompressionsraten?
- Wie unterscheiden sich Kompressions- und Dekompressionsdurchsatz?
- Wie beeinflussen Text, Binärdaten, bereits komprimierte Dateien und kleine Eingaben das Ergebnis?
- Welche Anforderungen ergeben sich aus Streaming und begrenztem Arbeitsspeicher?

## Konzeptioneller Datenpfad

```mermaid
flowchart LR
    INPUT[Eingabebytes] --> ANALYZE[Datencharakteristik]
    ANALYZE --> TRANSFORM[Optionale reversible Transformation]
    TRANSFORM --> ENCODE[Verlustfreier Encoder]
    ENCODE --> FORMAT[Kodierter Datenstrom]
    FORMAT --> DECODE[Decoder]
    DECODE --> REVERSE[Inverse Transformation]
    REVERSE --> OUTPUT[Originalbytes]
```

Das Diagramm zeigt eine mögliche Kompressionspipeline und **behauptet nicht, dass jede Stufe bereits implementiert ist**. Jede verwendete Transformation muss reversibel sein; außerdem sind Decoder-Kompatibilität und klare Formatgrenzen entscheidend.

## Korrektheitsinvariante

Eine verlustfreie Verarbeitung muss die ursprünglichen Bytes exakt wiederherstellen:

```text
decompress(compress(input)) == input
```

Tests müssen daher leere Eingaben, kleine Dateien, Binärdaten, schwer komprimierbare Inhalte, Wiederholungsmuster, abgeschnittene Datenströme und fehlerhafte Eingaben berücksichtigen. Kompressionsraten allein beweisen keine Korrektheit.

## Benchmark-Methodik

| Metrik | Definition |
|---|---|
| **Kompressionsverhältnis** | Unkomprimierte Bytes / komprimierte Bytes |
| **Platzersparnis** | (1 − komprimierte Bytes / Originalbytes) × 100 % |
| **Kompressionsdurchsatz** | Verarbeitete Originalbytes pro Sekunde |
| **Dekompressionsdurchsatz** | Wiederhergestellte Originalbytes pro Sekunde |
| **Maximaler Speicherbedarf** | Höchster Speicherverbrauch während eines Durchlaufs |
| **Round-Trip-Integrität** | Bytegenaue Übereinstimmung nach dem Dekodieren |

Für aussagekräftige Vergleiche müssen Datensätze, Hardware, Thread-Konfiguration und Messverfahren identisch sein. Containerformat und Metadaten-Overhead gehören zur komprimierten Größe.

Je nach Workload sind etablierte Codecs wie **zlib, Zstandard, LZ4 und Brotli** geeignete Referenzen. Ergebnisse sollten pro Datensatz statt als pauschales Ranking veröffentlicht werden.

## Forschungsschwerpunkte

**Redundanzanalyse.** Datenmerkmale vor der Wahl einer Kodierungsstrategie untersuchen.

**Reversible Transformationen.** Prüfen, ob bessere Kodierung den zusätzlichen Rechen- und Speicheraufwand rechtfertigt.

**Entropiekodierung.** Zusammenhang zwischen Symbolverteilungen, Dateigröße und Decoder-Komplexität untersuchen.

**Datenstromformat.** Framing, Versionierung, Integritätsprüfung und Fehlerbehandlung berücksichtigen.

**Leistung und Sicherheit.** Durchsatz und Speicher messen sowie ungültige Längen, beschädigte Daten und übermäßige Speicheranforderungen abfangen.

---

<div align="center">

**HobbitZip** · `Lossless Compression` · `Algorithms` · `Benchmarking` · `Systems Engineering`

Entwickelt von [Taha Kubilay](https://github.com/tahakubilay)

</div>
