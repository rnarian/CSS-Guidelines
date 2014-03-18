# Translation in progress...

# Allgemeine Notizen, Ratschläge und Richtlinien zu CSS

---

Gerade bei langen und komplexen Projekten mit dutzenden von Entwicklern, ist es wichtig, dass wir alle einheitlich und nach denselben Richtlinien arbeiten um:
 
 * Unsere Stylesheets wartbar zu halten
 * Unseren Code verständlich und gut lesbar zu halten
 * Unsere Stylesheets erweiterbar zu halten

Um diese Ziele zu erfüllen, müssen wir eine Vielzahl an Techniken anwenden.

Der erste Teil dieses Dokumentes behandelt Syntax, Formatierung und Anatomie von CSS, der zweite Teil beschäftigt sich mit der Herangehensweise und Grundhaltung um CSS zu konzipieren. Aufregend, oder?

## Inhalte

* [CSS Dokument Anatomie](#css-document-anatomy)
  * [Allgemein](#general)
  * [Eine Datei vs. Viele Dateien](#one-file-vs-many-files)
  * [Inhaltsangabe](#table-of-contents)
  * [Abschnittstitel](#section-titles)
* [Ordnung innerhalb der Stylesheets](#source-order)
* [Anatomie von rulesets](#anatomy-of-rulesets)
* [Namenskonventionen](#naming-conventions)
  * [JS hooks](#js-hooks)
  * [Internationalisierung](#internationalisation)
* [Kommentare](#comments)
  * [Kommentare auf Steroiden](#comments-on-steroids)
    * [Quasi-qualified selectors](#quasi-qualified-selectors)
    * [Code Tags](#tagging-code)
    * [Objekt/Erweiterung Pointer](#objectErweiterung-pointers)
* [Writing CSS](#writing-css)
* [Building new components](#building-new-components)
* [OOCSS](#oocss)
* [Layout](#layout)
* [Sizing UIs](#sizing-uis)
  * [Font sizing](#font-sizing)
* [Shorthand](#shorthand)
* [IDs](#ids)
* [Selektoren](#selectors)
  * [Over qualified selectors](#over-qualified-selectors)
  * [Performance von Selektoren](#selector-performance)
* [CSS selector intent](#css-selector-intent)
* [`!important`](#important)
* [Magic numbers and absolutes](#magic-numbers-and-absolutes)
* [Conditional stylesheets](#conditional-stylesheets)
* [Debugging](#debugging)
* [Preprocessors](#preprocessors)

---

## Anatomie eines CSS Dokumentes

Egal um welches Dokument es sich handelt, unser Ziel ist es immer eine gleichbleibende Formatierung zu schaffen und zu erhalten. Das bedeutet einheitliche Kommentare, einheitliche Syntax und einheitliche Benennungen.

### Allgemein

Begrenze deine Stylesheets wo es möglich ist auf eine maximale Breite von 80 Zeichen. Von dieser Faustregel ausgenommen sind Gradient Syntax und URLs in Kommentaren. Das ist in Ordnung, denn es gibt nichts was wir dagegen machen können.

Ich bevorzuge Einrückungen von vier (4) Leerzeichen gegenüber Tabs und schreibe mein CSS mehrzeilig.

### Eine Datei vs. Viele Dateien

Einige bevorzugen es mit einzelnen, großen Dateien zu arbeiten. Das ist auch in Ordnung und solange du dich an die folgenden Richtlinien hältst wirst du dadurch auch keine Probleme bekommen. Seit dem Wechsel zu Sass habe ich angefangen meine Stylesheets in viele kleine Includes aufzuteilen. Das ist auch in Ordnung... Die folgenden Regeln und Richtlinien sind zutreffend, unabhängig davon, für welche Methode du dich entscheidest. Der einzige beachtenswerte Unterschied besteht hinsichtlich unserer Inhaltsangabe und unserer Abschnittstitel. Lies weiter für weitere Erläuterungen...

### Inhaltsangabe

Am Anfang eines Stylesheets pflege ich eine Inhaltsangabe, welche die Abschnitte des Dokuments beschreibt. Zum Beispiel:

    /*------------------------------------*\
       $CONTENTS
    \*------------------------------------*/
    /**
     * CONTENTS............Liest du gerade
     * RESET...............Reset 
     * FONT-FACE...........Importiert font files
     */

Das gibt dem nächsten Entwickler Aufschluss darüber, was genau er oder sie von der Datei erwarten kann. Jeder Punkt der Inhaltsangabe zeigt genau auf einen Abschnittstitel.

Wenn du mit einem großen Stylesheet arbeitest, wird der zugehörige Abschnittstitel ebenfalls in der selben Datei befinden. Wenn du mit mit mehreren Dateien arbeitest, wird jeder Punkt der Inhaltsangabe auf einen Include zeigen, der diesen Abschnitt eingliedert.

### Abschnittstitel

Eine Inhaltsangabe ohne zugehörige Abschnittstitel wäre sinnlos. Kennzeichne deshalb jeden deiner Abschnitte folgendermaßen:

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/
   
Das vorangestellte `$`-Zeichen ermöglicht uns dabei eine Suche ([Cmd|Ctrl]+F) nach `$[ABSCHNITTSTITEL]` zu starten und diese damit **auf die Abschnittstitel zu beschränken**.

Wenn du mit einem großen Stylesheet arbeitest, lasse fünf (5) Leerzeilen zwischen jedem Abschnitt:

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/
    [Unsere
    Reset
    styles]





    /*------------------------------------*\
        $FONT-FACE
    \*------------------------------------*/

Der große Leerraum fällt beim schnellen Scrollen durch große Dateien rasch auf.

Wenn du mit mehreren Dateien arbeitest, beginne jede Datei mit einem Abschnittstitel, wobei hier keine extra Leerzeilen nötig sind.

### Ordnung innerhalb der Stylesheets

Versuche die Stylesheets nach Spezifität zu ordnen. Das garantiert, dass du den Vorteil der Vererbung und CSS' erstem <i>C</i>, der Cascade, nutzt.

Ein ordentlich geordnetes Stylesheet sieht ungefähr so aus:

**Reset**
**Elemente** - `h1`, `ul`, etc. ohne Klassen
**Objekte und Abstraktionen** - allgemeine, grundlegende Design Patterns
**Komponenten** - Komplette Komponenten konstruiert aus Objekten und ihren Erweiterungen.
**Style Trümpfe** - error states etc.

Das bedeutet, dass jeder Abschnitt auf den Vorherigen aufbaut und dessen Eigenschaften erbt. Dadurch sollte weniger Überschreiben, weniger Spezifitätsprobleme und rundum besser konzipierte Stylesheets entstehen.

Wenn du dich noch ausgiebiger mit diesem Thema beschäftigen möchtest, kann ich dir Jonathan Snooks [SMACSS](http://smacss.com) wärmstens empfehlen.

## Anatomie von rulesets

    [Selektor] {
        [Eigenschaft]: [Wert];
        [<-- Deklaration -->]
    }

Ich habe einige Standards beim Strukturieren von rulesets:

* Nutze mit Bindestrich getrennte Klassennamen (Ausgenommen BEM Notation,
  [siehe unten](#naming-conventions))
* Eingerückt mit 4 Leerzeichen
* Mehrzeilig
* Deklarationen sortiert nach Relevanz (NICHT alphabetisch)
* Rücke vendor-prefix Deklarationen so ein, dass ihre Werte ausgerichtet sind
* Rücke rulesets so ein, dass sie den DOM wiederspiegeln
* Verzichte nie auf das letzte Semikolon im ruleset

Ein kleines Beispiel:

    .widget {
        padding: 10px;
        border: 1px solid #BADA55;
        background-color: #C0FFEE;
        -webkit-border-radius: 4px;
           -moz-border-radius: 4px;
                border-radius: 4px;
    }
        .widget-heading {
            font-size: 1.5rem;
            line-height: 1;
            font-weight: bold;
            color: #BADA55;
            margin-right: -10px;
            margin-left: -10px;
            padding: 0.25em;
        }

Hier sehen wir, dass `.widget-heading` ein Kind von `.widget` sein muss, weil wir `.widget-heading` eine Ebene tiefer als `.widget` eingerückt haben. Das sind nützliche Informationen für Entwickler, die nun durch einen Blick auf die Einrückung unserer rulesets erfasst werden können.

Wir sehen auch, dass die Deklarationen von `.widget-heading` nach ihrer Relevanz geordnet sind; `.widget-heading` ist ein Text-Element also beginnen wir mit unseren Text Regeln, gefolgt von allem anderen.

Eine Ausnahme zu unseren mehrzeiligen Regeln können folgende Fälle sein:

    .t10    { width: 10% }
    .t20    { width: 20% }
    .t25    { width: 25% }       /* 1/4 */
    .t30    { width: 30% }
    .t33    { width: 33.333% }   /* 1/3 */
    .t40    { width: 40% }
    .t50    { width: 50% }       /* 1/2 */
    .t60    { width: 60% }
    .t66    { width: 66.666% }   /* 2/3 */
    .t70    { width: 70% }
    .t75    { width: 75% }       /* 3/4*/
    .t80    { width: 80% }
    .t90    { width: 90% }

In diesem Beispiel (aus [inuit.css’s table grid system](https://github.com/csswizardry/inuit.css/blob/master/base/_tables.scss#L96))
macht es mehr Sinn unser CSS einzeilig zu schreiben.

## Namenskonventionen

For the most part I simply use hyphen delimited classes (e.g. `.foo-bar`, not
`.foo_bar` or `.fooBar`), however in certain circumstances I use BEM (Block,
Element, Modifier) notation.

Zum größten Teil verwende ich durch Bindestrich getrennte Klassen (z.B. `.foo-bar`, nicht `.foo_bar` oder `.fooBar`), benutze allerdings unter gewissen Umständen die BEM (Block, Element, Modifier) Notation.

<abbr title="Block, Element, Modifier">BEM</abbr> ist eine Methode um CSS-Selektoren durch ihre Benennung und Klassifizierung viel exakter, transparenter und informativer zu machen.

Die Namenskonvention folgt diesem Muster:

    .block {}
    .block__element {}
    .block--modifier {}

* `.block` repräsentiert die höhere Ebene einer Abstraktion oder eines Komponenten.
* `.block__element` repräsentiert eine Kindelement von `.block` das hilft `.block` als Ganzes zu formen.
* `.block--modifier` repräsentiert einen anderen Status oder eine andere Version von `.block`.

Eine **Analogie** zum Aufbau einer BEM Klassen könnte sein:

    .person {}
    .person--woman {}
        .person__hand {}
        .person__hand--left {}
        .person__hand--right {}

Hier sehen wir, dass das grundlegende Objekt welches wir beschreiben eine Person ist, und das ein anderer Typ der Person eine Frau sein könnte. Wir können auch sehen, dass diese Person Hände hat; diese sind weitere "Bestandteile" von Personen und es gibt verschiedene Varianten, wie 'left' und 'right'.

Nun können wir unsere Selektoren basierend auf dem grundlegendem Objekt benennen und kommunizieren zugleich welche Aufgabe der Selektor hat; handel es sich um einen Bestandteil (`__`) oder eine Variation (`--`)?

`.page-wrapper` ist also ein unabhängiger Selektor. Er ist weder Teil einer Abstraktion noch einer Komponente und ist somit korrekt benannt. Der Selektor `.widget-heading` dagegen, _ist_ einer Komponente zugehörig; Er ist ein Kind von `.widget` also sollten wir diese Klasse in `.widget__heading` umbenennen.

BEM sieht etwas hässlicher aus und ist viel ausführlicher, aber es bietet uns auch viel Übersicht, da wir die Funktionen von Elementen und deren Beziehungen zueinander allein anhand ihres Klassennamens erfassen können. Desweiteren lässt sich die BEM Syntax normalerweise - gerade durch die vielen Wiederholungen - gut komprimieren (gzip).

Unabhängig davon, ob du BEM verwendest oder nicht, solltest du immer sicherstellen, dass Klassen vernünftig benannt sind. Halte sie so kurz wie möglich, aber so lang wie nötig. Stelle sicher, das alle Objekte oder Abstraktionen sehr unbestimmt benannt sind (z.B. `.ui-list`, `.media`) um eine größere Wiederverwendbarkeit zu gewährleisten. Erweiterungen von Objekten sollten viel genauer benannt sein (z.B. `.user-avatar-link`). Mach dir keine Gedanken über die Länge von Klassennamen; gzip komprimiert ordentlich geschriebenen Code _erstaunlich_ gut.

### Klassen im HTML

Um die Lesbarkeit zu verbessern, trenne deine Klassen im HTML mit zwei (2) Leerzeichen:

    <div class="foo--bar  bar__baz">

Durch den größeren Leerraum sollte es leichter sein mehrere Klassen zu erkennen und lesen.

### JS hooks

**Nutze niemals CSS _styling_ Klassen als JavaScript hook.** Anfügen von JS Verhalten an eine styling Klassen bedeutet, dass wir das Eine nie ohne das Andere nutzen können.

Wenn du JavaScript ans Markup binden musst, nutze eine spezielle JS CSS Klasse. Das ist einfach eine Klasse mit vorangestelltem `.js-`, z.B. `.js-toggle`, `.js-drag-and-drop`. Das bedeutet, dass wir JS und CSS an Klassen hängen können, ohne mühsame Überlappungen.

    <th class="is-sortable  js-is-sortable">
    </th>

Das obige Markup beinhaltet zwei Klassen. Eine über die du Styles für sortierbare Table-Columns anhängen kannst, und eine weitere welche uns erlaubt die Sortierungsfunktionalität hinzuzufügen.

### Internationalisierung

Obwohl ich ein Britischer Entwickler bin—und mein ganzes Leben damit verbracht habe <i>colour</i> statt <i>color</i> zu schreiben-denke ich, dass es besser ist, zum Wohle der Beständigkeit, im CSS immer US-English zu nutzen. CSS, wie die meisten (wenn nicht alle) anderen Sprachen wird in US-English geschrieben, sodass es Misch-Syntax wie `color: red;` in Klassen wie `.colour-picker {}` an Beständigkeit mangelt. Ich habe bislang nahegelegt Klassennamen zweisprachig auszuformulieren, zum Beispiel:

    .color-picker,
    .colour-picker {
    }

Allerdings wurde das Beibehalten von zwei Versionen von jeder Variable beim Arbeiten an einem sehr großen Sass Project mit dutzenden Farb-Variablen (z.B. `$brand-color`, `$highlight-color` etc.) schnell lästig. Es bedeutet auch doppelt so viel arbeit bei Dingen wie "Suchen und Ersetzen".

Benenne Klassen und Variablen deshalb immer in der Sprache in der du arbeitest.

## Kommentare

Ich nutze einen docBlock-ähnlichen Kommentar Stil welchen ich in der Länge auf 80 Zeichen pro Zeile beschränke:

    /**
     * Das ist ein docBlock Kommentar
     *
     * Das ist eine längere Beschreibung des Kommentars, der den Code genauer 
     * beschreibt. Wir beschränken diese Zeilen auf maximal 80 Zeichen. 
     *
     * Wir können markup in den Kommentaren nutzen und sollten das 
     * auch tun:
     *
       <div class=foo>
           <p>Lorem</p>
       </div>
     *
     * Wir stellen Codezeilen kein Sternchen voran, um damit Copy und Paste zu 
     * ermöglichen.
     */

Du solltest deinen Code so gut wie möglich kommentieren und dokumentieren. Selbst was dir verständlich und selbsterklärend erscheint, ist womöglich für einen anderen Entwickler nicht ganz so leicht zu erkennen. Schreib einen Brocken Code und dokumentiere ihn anschließend. 

### Kommentare auf Steroiden

Es gibt eine Reihe an fortgeschrittenen Techniken die du verwenden kannst was Kommentare angeht:

* Quasi-Qualifizierte Selektoren
* Code taggen
* Objekt/Erweiterung Hinweise

#### Quasi-Qualifizierte Selektoren

Du solltest deine Selektoren niemals zusätzlich qualifizieren - also niemals `ul.nav {}` schreiben, wenn du einfach `.nav` nutzen kannst. Das Qualifizieren von Selektoren vermindert die Leistung, verhindert ein potenzielles Wiederverwenden der Klasse auf einen anderen Element-Typ und die Spezifität des Selektors nimmt zu. Das sind alles Dinge, die unter allen Umständen vermieden werden müssen.

Allerdings ist es manchmal hilfreich dem nächsten Entwickler zu zeigen wo eine Klasse genutzt werden soll. Nehmen wir `.product-page` als Beispiel; Diese Klasse hört sich so an, als würde sie auf einem Container auf hoher Ebene genutzt werden, eventuell das `html` oder `body` Element, aber mit `.product-page` alleine ist das unmöglich zu sagen.

Indem wir den Selektor quasi-qualifizieren (auskommentieren des vorangestellten Selektor Typs) können wir kommunizieren an welcher Stelle wir diese Klasse anwenden möchten:

    /*html*/.product-page {}

So sehen wir genau wo die Klasse angewendet werden soll, jedoch ohne die Nachteile der Spezifität oder an Wiederverwendbarkeit einzubüßen.

Andere Beispiele könnten sein:

    /*ol*/.breadcrumb {}
    /*p*/.intro {}
    /*ul*/.image-thumbs {}

Hier können wir sehen, wo wir beabsichtigen jede dieser Klassen zu platzieren, ohne dabei die Spezifität der Selektoren zu beeinflussen.

#### Code Tags

Wenn du eine neue Komponente schreibst, hinterlasse einige Tags in einem vorangestellten Kommentar, um ihre Funktionalität beschreiben. Zum Beispiel:

    /**
     * ^navigation ^lists
     */
    .nav {}

    /**
     * ^grids ^lists ^tables
     */
    .matrix {}

Diese Tags erlauben anderen Entwicklern Code-Snippets zu finden, indem sie nach Funktionalität suchen. Wenn ein Entwickler mit Listen arbeiten muss, kann er oder sie eine Suche nach `^lists` starten und findet die `.nav` und `.matrix` Objekte (und vermutlich noch weitere).

#### Objekt/Erweiterung Zeiger

Beim Arbeiten in einer Objektorientieren Art und Weise wird es oft vorkommen, dass du zwei Brocken von CSS hast (einer für das Skelett (das Objekt) und ein anderer für die Haut (die Erweiterung)) die sehr eng miteinander verbunden, aber an ganz unterschiedlichen Orten abgelegt sind. Verwende sog. <i>Objekt/Erweiterung Zeiger</i>, um eine feste Verbindung zwischen dem Objekt und seiner Erweiterung herzustellen. Diese sind einfach Kommentare die folgendermaßen funktionieren:

In deinem Base-Stylesheet:

    /**
     * Extend `.foo` in theme.css
     */
     .foo {}

In deinem Theme-Stylesheet:

    /**
     * Extends `.foo` in base.css
     */
     .bar {}

Hier haben wir eine feste Verbindung zwischen zwei getrennten Stücken Code hergestellt.

---

## Writing CSS

Im vorherigen Abschnitt haben wir damit beschäftigt, wie wir anhand einiger quantifizierbaren Regeln unser CSS strukturieren und formen. Der nächste Abschnitt ist hingegen etwas theoretischer und behandelt unsere Einstellung und Herangehensweise.

## Erstellen von neuen Komponenten

Wenn du eine neue Komponente erstellst, schreibe dein Markup **vor** dem CSS. So kannst du leicht erkennen welche CSS Eigenschaften automatisch vererbt werden und vermeidest dadurch das Wiederanwenden von überflüssigen Styles.

Außerdem kannst du dich dadurch voll und Ganz auf Daten, Inhalt und Semantik konzentrieren und _anschließend_ nur die relevanten Klassen und CSS verwenden.

## OOCSS

Ich arbeite auf OOCSS Art und Weise; Ich teile Komponenten in Struktur (Objekte) und Skin (Erweiterungen) auf. Als **Analogie** (nicht als Beispiel) kannst du dir folgendes vorstellen:

    .room {}

    .room--kitchen {}
    .room--bedroom {}
    .room--bathroom {}

In diesem Beispiel haben wir verschiedene Arten von Räumen, wovon alle ähnliche Eigenschaften teilen. Alle haben Flure, Dächer, Wände und Türen. Wir können diese Informationen in einer abstrahierten Klasse `.room {}` festhalten. Es gibt jedoch auch besondere Arten von Räumen, die sich von den Anderen unterscheiden. Eine Küche hat unter Umständen einen gekachelten Boden, ein Schlafzimmer eher einen Teppich, ein Bad hat nicht unbedingt ein Fenster, aber ein Schlafzimmer höchstwahrscheinlich schon. Jeder Raum hat vermutlich unterschliedlich gestrichene Wände. OOCSS lehrt uns die geteilten Eigenschaften in einem Basisobjekt zu abstrahieren und diese Information dann mit spezifischeren Klassen zu erweitern.

Also, anstatt dutzende gesonderte Komponenten zu erstellen, versuche die sich wiederholenden Muster zu erkennen und abstrahiere sie in wiederverwendbare Klassen. Erstelle diese "Skelette" als Basisobjekte, baue mit weiteren Klassen auf ihnen auf und erweitere somit ihr Styling unter anderen Gegebenheiten.

Wenn du eine neue Komponente erstellst, teile sie in Struktur und Aussehen. Erstelle die Struktur einer Komponente mit Hilfe von sehr allgemeinen Klassen, damit wir dieses Konstrukt wiederverwenden können und nutze dann spezifischere Klassen um das Aussehen zu erweitern und anzupassen.

## Layout

Alle Komponenten sollten ohne feste Breitenangaben erstellt werden. Sie sollten immer fluide bleiben und ihre Breite sollte vom jeweiligen Elterelement (z.B. Grid System) bestimmt werden.

`height` sollte **niemals** auf Elemente angewandt werden. Nur bei Elementen mit festen Maßen, wie Bildern oder Sprites, darf eine Höhe oder Breite definiert werden. Setze niemals die Höhe von `p`s, `ul`s, `div`s, und eigentlich auch allem Anderen. Oft kann der gewünschte Effekt über `line-height` erreicht werden, was darüber hinaus noch viel flexibler ist.

Grid Systeme sollte man sich wie Regale vorstellen. Sie besitzen Inhalte, sind aber nicht selbst Inhalte. Du stellst erst dein Regal auf und füllst es dann mit deinem Kram. Wenn wir also unser Grid separat von den Komponenten aufbauen, können die Komponenten viel leichter bewegt werden, als wenn wir deren Höhe oder Breite definiert hätten. Das macht unsere Front-Ends um einiges anpassungsfähiger und wir können effektiver damit arbeiten.

Du darfst Styles nie auf das Grid System selbst anwenden, dieses dient nur dazu um das Layout zu definieren. Wende Styles auf die Inhalte _innerhalb_ des Grid System an. Wende unter absolut _keinen_ Umständen box-model Eigenschaften auf ein Grid Element an.

## Größenangaben für Benutzeroberflächen

Ich verwende und kombiniere verschiedene Methoden um Größe in Benutzeroberflächen anzugeben. Prozente, Pixel, ems, rems, (und teilweise auch ohne die Angabe einer Einheit).

Die Breiten in Grid Systemen sollten idealerweise in Prozenten angegeben werden. Da ich ein Grid System nutze, um die Breiten von Spalten und Seiten zu beeinflussen, kann ich die Komponenten (wie oben beschrieben) ohne Angabe von Höhe oder Breite umsetzen.

Schriftgrößen gebe ich in rems mit Pixel-Fallback an. Dies gewährt uns alle Vorteile der Barrierefreiheit von ems (with the confidence of pixels). Hier ist ein nützliches Sass-Mixin zur Berechnung von rem und Pixel-Fallback (davon ausgehend, dass du deine Basefontsize in einer Variable setzt):

    @mixin font-size($font-size) {
        font-size: $font-size +px;
        font-size: $font-size / $base-font-size +rem;
    }

Pixel verwende ich nur bei Elementen, die schon feste Abmessungen hatten bevor sie auf die Seite kamen. Hier handelt es sich um Elemente wie Bilder oder Sprites, deren Dimensionen von Haus aus in Pixel angegeben werden.

### Anpassen von Schriftgrößen

Ich definiere eine Reihe von Klassen - ähnlich wie beim Grid System - um die Schriftgrößen anzupassen. Diese Klassen können dann genutzt werden, um Überschriften unabhängig ihrer Hierarchie zu stylen. Für weitere Informationen zu diesem Thema empfehle ich meinen Artikel [Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css).

## Shorthand

**Shorthand CSS muss immer mit Vorsicht genutzt werden.**

Deklarationen wie `background: red;` mögen erstmal verlocken, aber was du damit tatsächlich ausdrückst ist folgendes: ‘Ich möchte kein Bild das scrollt, links-oben ausgerichtet ist, sich nach X oder Y wiederholt, sondern eine rote Hintergrundfarbe’. In neun von zehn Fällen wird das auch problemlos funktionieren, aber der eine Fall, der Auswirkungen nach sich zieht ist nervig genug um komplett auf solche Shorthands zu verzichten. Nutze stattdessen einfach `background-color: red;`.

Auch Deklarationen wie `margin: 0;` sind nett und kurz, aber  verwende sie nur mit Bedacht. Wenn du eigentlich nur den Abstand nach unten beeinflussen möchtest, ist es besser `margin-bottom: 10px;` zu definieren, anstatt `margin: 0 0 10px 0`.

Sei dir im Klaren darüber, welche Eigenschaften du setzen willst und achte darauf, dass du nicht versehentlich andere Eigenschaften mit dem Shorthand überschreibst.

Shorthands sind nützlich aber auch leicht falsch zu verwenden.

## IDs

A quick note on IDs in CSS before we dive into selectors in general.

Eine kurze Bemerkung zu IDs in CSS, bevor wir uns um Selektoren im Allgemeinen kümmern

**Benutze IDs NIEMALS in CSS.**

Du kannst Sie in deinem Markup als JS Hooks oder als Fragmentbezeichner verwenden, aber du willst nicht eine einzige ID in deinen Stylesheets!

Klassen haben den Vorteil, dass sie wiederverwendbar sind (selbst wenn wir das nicht brauchen, die Möglichkeit bleibt bestehen) und kommen mit einer angenehmen, niedrigen Spezifität. Spezifität ist eine der schnellsten Wege um bei Projekten in Probleme zu geraten, daher ist es erforderlich sie zu allen Zeiten so gering wie möglich zu halten. Eine ID ist **255** mal spezifischer als eine Klasse, benutze sie deshalb _niemals_ in CSS.

## Selektoren

Halte deine Selektoren so kurz, effizient und wiederverwendbar wie möglich.

Selektoren, die stark von ihrer Position abhängig sind, sind aus mehreren Gründen unpraktisch. Nehmen wir zum Beispiel `.sidebar h3 span {}`. Dieser Selektor ist zu stark von seiner Position abhängig, weshalb wir die `span` nicht außerhalb einer `h3`, und diese wiederum nicht außerhalb von `.sidebar` verwenden, ohne dabei unser Styling zu verlieren.

Zu lange Selektoren bringen außerdem Performance Problemen mit sich: Je mehr Abgleiche in einem Selektor enthalten sind (`.sidebar h3 span` hat beispielsweise drei Abgleiche, `.content ul p a` hat vier), desto mehr Arbeit hat der Browser damit.

Stelle deshalb sicher, dass deine Styles unabhängig von ihrer Position sind, wähle deine Selektoren mit Bedacht und halte sie kurz.

Selektoren als Ganzes soltten so kurz gehalten werden wie möglich (zum Beispiel nur eine Klasse tief), die Klasse selber jedoch sollte so lange wie nötig sein. Eine Klasse `.user-avatar` ist wesentlich angenehmer als `.usr-avt`.

**Merke**: Klassen sind weder semantisch noch unsemantisch, sie sind entweder sinnig oder unsinnig! Mach dir keine Gedanken über die "Semantik" einer Klasse, suche lieber nach etwas sinnigem und zukunftstauglichen.

### Zu spezifische Selektoren

Wie zuvor bereits erwähnt bedeuten spezifische Selektoren sind meißtens nichts gutes.

Ein zu spezifischer Selektor ist beispielsweise `div.promo`. Du könntest vermutlich den selben Effekt erzielen, wenn du einfach  `.promo` verwenden würdest. Natürlich wird es manchmal den Fall geben, dass du eine Klasse _bewusst_ in Verbindung mit einem Element spezifizierst, z.b. wenn du eine allgemeine `.error` Klasse hast, die - je nachdem auf welchem Element sie angewendet wird - unterschiedlich aussehen soll (z.B.
`.error { color: red; }` `div.error { padding: 14px; }`), im Allgemeinen solltest du es allerdings wo es möglich ist vermeiden.

Ein weiteres Beispie für einen zu spezifischen Selektor wäre `ul.nav li a {}`. Wie im obigen Beispiel, können wir die `ul` ersatzlos weglassen und da wir wissen, dass `.nav` eine Liste ist und daraus schließen können, das `a` in einem `li` sein _muss_, können wir `ul.nav li a {}` auf `.nav a {}` vereinfachen.


### Performance von Selektoren

Whilst it is true that browsers will only ever keep getting faster at rendering
CSS, efficiency is something you could do to keep an eye on. Short, unnested
selectors, not using the universal (`* {}`) selector as the key selector, and
avoiding more complex CSS3 selectors should help circumvent these problems.

## CSS selector intent

Instead of using selectors to drill down the DOM to an element, it is often best
to put a class on the element you explicitly want to style. Let’s take a
specific example with a selector like `.header ul {}`…

Let’s imagine that `ul` is indeed the main navigation for our website. It lives
in the header as you might expect and is currently the only `ul` in there;
`.header ul {}` will work, but it’s not ideal or advisable. It’s not very future
proof and certainly not explicit enough. As soon as we add another `ul` to that
header it will adopt the styling of our main nav and the the chances are it
won’t want to. This means we either have to refactor a lot of code _or_ undo a
lot of styling on subsequent `ul`s in that `.header` to remove the effects of
the far reaching selector.

Your selector’s intent must match that of your reason for styling something;
ask yourself **‘am I selecting this because it’s a `ul` inside of `.header` or
because it is my site’s main nav?’**. The answer to this will determine your
selector.

Make sure your key selector is never an element/type selector or
object/abstraction class. You never really want to see selectors like
`.sidebar ul {}` or `.footer .media {}` in our theme stylesheets.

Be explicit; target the element you want to affect, not its parent. Never assume
that markup won’t change. **Write selectors that target what you want, not what
happens to be there already.**

For a full write up please see my article
[Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

## `!important`

It is okay to use `!important` on helper classes only. To add `!important`
preemptively is fine, e.g. `.error { color: red !important }`, as you know you will
**always** want this rule to take precedence.

Using `!important` reactively, e.g. to get yourself out of nasty specificity
situations, is not advised. Rework your CSS and try to combat these issues by
refactoring your selectors. Keeping your selectors short and avoiding IDs will
help out here massively.

## Magic numbers and absolutes

A magic number is a number which is used because ‘it just works’. These are bad
because they rarely work for any real reason and are not usually very
futureproof or flexible/forgiving. They tend to fix symptoms and not problems.

For example, using `.dropdown-nav li:hover ul { top: 37px; }` to move a dropdown
to the bottom of the nav on hover is bad, as 37px is a magic number. 37px only
works here because in this particular scenario the `.dropdown-nav` happens to be
37px tall.

Instead you should use `.dropdown-nav li:hover ul { top: 100%; }` which means no
matter how tall the `.dropdown-nav` gets, the dropdown will always sit 100% from
the top.

Every time you hard code a number think twice; if you can avoid it by using
keywords or ‘aliases’ (i.e. `top: 100%` to mean ‘all the way from the top’)
or&mdash;even better&mdash;no measurements at all then you probably should.

Every hard-coded measurement you set is a commitment you might not necessarily
want to keep.

## Conditional stylesheets

IE stylesheets can, by and large, be totally avoided. The only time an IE
stylesheet may be required is to circumvent blatant lack of support (e.g. PNG
fixes).

As a general rule, all layout and box-model rules can and _will_ work without an
IE stylesheet if you refactor and rework your CSS. This means you never want to
see `<!--[if IE 7]> element { margin-left: -9px; } < ![endif]-->` or other such
CSS that is clearly using arbitrary styling to just ‘make stuff work’.

## Debugging

If you run into a CSS problem **take code away before you start adding more** in
a bid to fix it. The problem exists in CSS that is already written, more CSS
isn’t the right answer!

Delete chunks of markup and CSS until your problem goes away, then you can
determine which part of the code the problem lies in.

It can be tempting to put an `overflow: hidden;` on something to hide the effects
of a layout quirk, but overflow was probably never the problem; **fix the
problem, not its symptoms.**

## Preprocessors

Sass is my preprocessor of choice. **Use it wisely.** Use Sass to make your CSS
more powerful but avoid nesting like the plague! Nest only when it would
actually be necessary in vanilla CSS, e.g.

    .header {}
    .header .site-nav {}
    .header .site-nav li {}
    .header .site-nav li a {}

Would be wholly unnecessary in normal CSS, so the following would be **bad**
Sass:

    .header {
        .site-nav {
            li {
                a {}
            }
        }
    }

If you were to Sass this up you’d write it as:

    .header {}
    .site-nav {
        li {}
        a {}
    }
