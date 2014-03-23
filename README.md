# Allgemeine Notizen, Ratschläge und Richtlinien zu CSS

Gerade bei langen und komplexen Projekten mit dutzenden von Entwicklern, ist es wichtig, dass wir alle einheitlich und nach denselben Richtlinien arbeiten, um:
 
* unsere Stylesheets wartbar zu halten.
* unseren Code verständlich und gut lesbar zu halten.
* unsere Stylesheets erweiterbar zu halten.

Um diese Ziele zu erfüllen, müssen wir eine Vielzahl von Techniken anwenden.

Der erste Teil dieses Dokumentes behandelt Syntax, Formatierung und Anatomie von CSS. Der zweite Teil beschäftigt sich mit der Herangehensweise und Grundhaltung um skalierbares CSS zu konzipieren. Aufregend, oder?

## Inhalte

* [Anatomie eines CSS Dokumentes](#anatomie-eines-css-dokumentes)
  * [Allgemein](#allgemein)
  * [Eine Datei vs. Viele Dateien](#eine-datei-vs-viele-dateien)
  * [Inhaltsangabe](#inhaltsangabe)
  * [Abschnittstitel](#abschnittstitel)
* [Ordnung innerhalb der Stylesheets](#ordnung-innerhalb-der-stylesheets)
* [Anatomie von rulesets](#anatomie-von-rulesets)
* [Namenskonventionen](#namenskonventionen)
  * [JS hooks](#js-hooks)
  * [Internationalisierung](#internationalisierung)
* [Kommentare](#kommentare)
  * [Kommentare auf Steroiden](#kommentare-auf-steroiden)
    * [Quasi-qualifizierte Selektoren](#quasi-qualifizierte-selektoren)
    * [Code Tags](#code-tags)
    * [Objekt/Erweiterung Zeiger](#objekterweiterung-zeiger)
* [CSS schreiben](#css-schreiben)
* [Erstellen neuer Komponenten](#erstellen-neuer-komponenten)
* [OOCSS](#oocss)
* [Layout](#layout)
* [Größenangaben für Benutzeroberflächen](#gr%C3%B6%C3%9Fenangaben-f%C3%BCr-benutzeroberfl%C3%A4chen)
  * [Anpassen von Schriftgrößen](#anpassen-von-schriftgr%C3%B6%C3%9Fen)
* [Shorthand](#shorthand)
* [IDs](#ids)
* [Selektoren](#selektoren)
  * [Zu spezifische Selektoren](#zu-spezifische-selektoren)
  * [Performance von Selektoren](#performance-von-selektoren)
* [Absicht eines CSS Selektors](#absicht-eines-css-selektors)
* [`!important`](#important)
* [Magische Zahlen und Konstanten](#magische-zahlen-und-konstanten)
* [Conditional Stylesheets](#conditional-stylesheets)
* [Debugging](#debugging)
* [Preprocessors](#preprocessors)

---

## Anatomie eines CSS Dokumentes

Egal um welches Dokument es sich handelt, unser Ziel ist es immer eine gleichbleibende Formatierung zu schaffen und zu erhalten. Das bedeutet einheitliche Kommentare, einheitliche Syntax und einheitliche Benennungen.

### Allgemein

Begrenze deine Stylesheets, wo es möglich ist, auf eine maximale Breite von 80 Zeichen. Von dieser Faustregel ausgenommen sind Gradient Syntax und URLs in Kommentaren. Das ist in Ordnung, denn es gibt nichts was wir dagegen machen können.

Ich bevorzuge Einrückungen über vier (4) Leerzeichen gegenüber von Tabs und schreibe mein CSS mehrzeilig.

### Eine Datei vs. Viele Dateien

Einige bevorzugen es mit einzelnen, großen Dateien zu arbeiten. Das ist in Ordnung und solange du dich an die folgenden Richtlinien hältst, wirst du dadurch auch keine Probleme bekommen. Seit dem Wechsel zu Sass habe ich angefangen meine Stylesheets in viele kleine Includes aufzuteilen. Auch das ist in Ordnung... Die folgenden Regeln und Richtlinien gelten unabhängig davon, für welche Methode du dich entscheidest. Der einzige beachtenswerte Unterschied besteht hinsichtlich unserer Inhaltsangabe und unserer Abschnittstitel. Lies weiter für zusätzliche Erläuterungen...

### Inhaltsangabe

Am Anfang eines Stylesheets pflege ich eine Inhaltsangabe, welche die Abschnitte des Dokumentes beschreibt. Zum Beispiel:

    /*------------------------------------*\
       $CONTENTS
    \*------------------------------------*/
    /**
     * CONTENTS............Liest du gerade
     * RESET...............Reset 
     * FONT-FACE...........Importiert font files
     */

Das gibt dem nächsten Entwickler Aufschluss darüber, was genau er oder sie von der Datei erwarten kann. Jeder Punkt der Inhaltsangabe zeigt genau auf einen Abschnittstitel.

Wenn du mit einem großen Stylesheet arbeitest, wird sich der zugehörige Abschnittstitel ebenfalls in der selben Datei befinden. Wenn du mit mehreren Dateien arbeitest, wird jeder Punkt der Inhaltsangabe auf einen Include zeigen, der diesen Abschnitt enthält.

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
**Objekte und Abstraktionen** - allgemeine, grundlegende Design Pattern
**Komponenten** - komplette Komponenten bestehend aus Objekten und ihren Erweiterungen
**Style Trümpfe** - error states etc.

Das bedeutet, dass jeder Abschnitt auf den Vorherigen aufbaut und deren Eigenschaften erbt. Dadurch sollte zum Einen weniger Überschreiben, zum Anderen weniger Spezifitätsprobleme und rundum besser konzipierte Stylesheets entstehen.

Wenn du dich noch ausgiebiger mit diesem Thema beschäftigen möchtest, kann ich dir Jonathan Snooks [SMACSS](http://smacss.com) wärmstens empfehlen.

## Anatomie von rulesets

    [Selektor] {
        [Eigenschaft]: [Wert];
        [<-- Deklaration -->]
    }

Ich habe einige Standards beim Strukturieren von rulesets:

* Verwende mit Bindestrich getrennte Klassennamen (Ausgenommen BEM Notation,
  [siehe unten](#namenskonventionen))
* Rücke mit 4 Leerzeichen ein
* Schreibe Mehrzeilig
* Sortiere Deklarationen nach Relevanz (NICHT alphabetisch)
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

Wir sehen auch, dass die Deklarationen von `.widget-heading` nach ihrer Relevanz geordnet sind: `.widget-heading` ist ein Text-Element, also beginnen wir mit unseren Text Regeln, gefolgt von allem Anderen.

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

Zum größten Teil verwende ich durch Bindestrich getrennte Klassen (z.B. `.foo-bar`, nicht `.foo_bar` oder `.fooBar`). Unter gewissen Umständen allerdings die BEM (Block, Element, Modifier) Notation.

<abbr title="Block, Element, Modifier">BEM</abbr> ist eine Methode um CSS-Selektoren durch ihre Benennung und Klassifizierung viel exakter, transparenter und informativer zu machen.

Die Namenskonvention folgt diesem Muster:

    .block {}
    .block__element {}
    .block--modifier {}

* `.block` repräsentiert die höhere Ebene einer Abstraktion oder einer Komponente.
* `.block__element` repräsentiert ein Kindelement von `.block`, das hilft `.block` als Ganzes zu formen.
* `.block--modifier` repräsentiert einen anderen Status oder eine andere Version von `.block`.

Eine **Analogie** zum Aufbau einer BEM Klassen könnte sein:

    .person {}
    .person--woman {}
        .person__hand {}
        .person__hand--left {}
        .person__hand--right {}

Hier sehen wir, dass das grundlegende Objekt welches wir beschreiben eine Person ist und das ein anderer Typ der Person eine Frau sein könnte. Wir können auch sehen, dass die Person Hände hat. Dies sind weitere "Bestandteile" von Personen und es gibt verschiedene Varianten, wie 'left' und 'right'.

Nun können wir unsere Selektoren basierend auf dem grundlegendem Objekt benennen und kommunizieren zugleich welche Aufgabe der Selektor hat. Handelt es sich um einen Bestandteil (`__`) oder eine Variation (`--`)?

`.page-wrapper` ist also ein unabhängiger Selektor. Er ist weder Teil einer Abstraktion noch einer Komponente und ist somit korrekt benannt. Der Selektor `.widget-heading` dagegen, _ist_ einer Komponente zugehörig. Er ist ein Kind von `.widget`, also sollten wir diese Klasse in `.widget__heading` umbenennen.

BEM sieht etwas hässlicher aus und ist viel ausführlicher, aber es bietet uns auch viel Übersicht, da wir die Funktionen von Elementen und deren Beziehungen zueinander alleine anhand ihres Klassennamens erfassen können. Desweiteren lässt sich die BEM Syntax normalerweise - gerade durch die vielen Wiederholungen - gut komprimieren (gzip).

Unabhängig davon, ob du BEM verwendest oder nicht, solltest du immer sicherstellen, dass Klassen vernünftig benannt sind. Halte sie so kurz wie möglich, aber so lang wie nötig. Stelle sicher, das alle Objekte oder Abstraktionen sehr unbestimmt benannt sind (z.B. `.ui-list`, `.media`) um eine größere Wiederverwendbarkeit zu gewährleisten. Erweiterungen von Objekten sollten viel genauer benannt sein (z.B. `.user-avatar-link`). Mach dir keine Gedanken über die Länge von Klassennamen; gzip komprimiert ordentlich geschriebenen Code _erstaunlich_ gut.

### Klassen im HTML

Um die Lesbarkeit zu verbessern, trenne deine Klassen im HTML mit zwei (2) Leerzeichen:

    <div class="foo--bar  bar__baz">

Durch den größeren Leerraum sollte es leichter sein, mehrere Klassen zu erkennen und zu lesen.

### JS hooks

**Nutze niemals CSS _styling_ Klassen als JavaScript hook.** Anfügen von JS Verhalten an eine styling-Klasse bedeutet, dass wir das Eine nie ohne das Andere nutzen können.

Wenn du JavaScript ans Markup binden musst, nutze eine spezielle JS CSS Klasse. Das ist eine Klasse mit vorangestelltem `.js-`, z.B. `.js-toggle`, `.js-drag-and-drop`. Das bedeutet, dass wir JS und CSS Klassen ohne etwaige Überschneidungen an unser Markup binden können.

    <th class="is-sortable  js-is-sortable">
    </th>

Das obige Markup beinhaltet zwei Klassen. Eine über die du Styles für sortierbare Tabellenspalten anhängen kannst, und eine weitere um die Sortierungsfunktionalität zu gewährleisten.

### Internationalisierung

Obwohl ich ein Britischer Entwickler bin — und mein ganzes Leben damit verbracht habe <i>colour</i> statt <i>color</i> zu schreiben - denke ich, dass es - aus Gründen der Beständigkeit - besser ist, im CSS immer US-English zu nutzen. CSS wird, wie die meisten (wenn nicht alle) anderen Sprachen in US-English geschrieben, sodass es Misch-Syntax wie `color: red;` in Klassen wie `.colour-picker {}` an Beständigkeit mangelt. Ich habe bisher empfohlen Klassennamen zweisprachig auszuformulieren, zum Beispiel:

    .color-picker,
    .colour-picker {
    }

Allerdings wurde das Beibehalten von zwei Versionen von jeder Variable beim Arbeiten an einem sehr großen Sass Projekt mit dutzenden Farb-Variablen (z.B. `$brand-color`, `$highlight-color` etc.) schnell lästig. Es bedeutet auch doppelt so viel Arbeit bei Aufgaben wie "Suchen und Ersetzen".

Benenne Klassen und Variablen deshalb immer in der Sprache in der du arbeitest.

## Kommentare

Ich nutze einen docBlock-ähnlichen Kommentar Stil, den ich in der Länge auf 80 Zeichen pro Zeile beschränke:

    /**
     * Das ist ein docBlock Kommentar
     *
     * Das ist eine längere Beschreibung des Kommentars, der den Code genauer 
     * beschreibt. Wir beschränken diese Zeilen auf maximal 80 Zeichen. 
     *
     * Wir können Markup in den Kommentaren nutzen und sollten das 
     * auch tun:
     *
       <div class=foo>
           <p>Lorem</p>
       </div>
     *
     * Wir stellen Codezeilen kein Sternchen voran, um damit Copy und Paste zu 
     * ermöglichen.
     */

Du solltest deinen Code so gut wie möglich kommentieren und dokumentieren. Selbst was dir verständlich und selbsterklärend erscheint, ist womöglich für einen anderen Entwickler nicht ganz so leicht zu verstehen. Schreibe einen Chunk Code und dokumentiere ihn anschließend.

### Kommentare auf Steroiden

Was Kommentare angeht gibt es eine Reihe an fortgeschritteneren Techniken die du verwenden kannst:

* Quasi-Qualifizierte Selektoren
* Code taggen
* Objekt/Erweiterung Hinweise

#### Quasi-qualifizierte Selektoren

Du solltest deine Selektoren niemals zusätzlich qualifizieren - also niemals `ul.nav {}` schreiben, wenn du einfach `.nav` nutzen kannst. Das Qualifizieren von Selektoren vermindert die Leistung und verhindert ein potenzielles Wiederverwenden der Klasse auf einen anderen Element-Typ. Außerdem nimmt die Spezifität des Selektors zu. Das sind alles Dinge, die unter allen Umständen vermieden werden müssen.

Allerdings ist es manchmal hilfreich dem nächsten Entwickler zu zeigen wo eine Klasse genutzt werden soll. Nehmen wir `.product-page` als Beispiel; Diese Klasse sieht so aus, als würde sie auf einem Container auf hoher Ebene genutzt werden, eventuell das `html` oder `body` Element, aber mit `.product-page` alleine ist das unmöglich zu bestimmen.

Indem wir den Selektor quasi-qualifizieren (auskommentieren des vorangestellten Selektor Typs) können wir kommunizieren an welcher Stelle wir diese Klasse anwenden möchten:

    /*html*/.product-page {}

So sehen wir genau wo die Klasse angewendet werden soll, jedoch ohne den Nachteil der Spezifität und ohne dabei an Wiederverwendbarkeit einzubüßen.

Andere Beispiele könnten sein:

    /*ol*/.breadcrumb {}
    /*p*/.intro {}
    /*ul*/.image-thumbs {}

Hier können wir sehen, wo wir diese Klassen platzieren möchten, ohne dabei die Spezifität der Selektoren zu beeinflussen.

#### Code Tags

Wenn du eine neue Komponente entwickelst, hinterlasse einige Tags in einem vorangestellten Kommentar, um ihre Funktionalität beschreiben. Zum Beispiel:

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

Wenn du nach objekt-orientieren Prinzipien arbeitest, wird es oft vorkommen, dass du zwei Chunks an CSS hast (einer für das Skelett (das Objekt) und ein anderer für die Haut (die Erweiterung)) die sehr eng miteinander verbunden, aber an ganz unterschiedlichen Orten abgelegt sind. Verwende sog. <i>Objekt/Erweiterung Zeiger</i>, um eine feste Verbindung zwischen dem Objekt und seiner Erweiterung herzustellen. Dies sind einfach Kommentare die folgendermaßen funktionieren:

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

## CSS schreiben

Im vorherigen Abschnitt haben wir uns damit beschäftigt, wie wir anhand einiger quantifizierbaren Regeln unser CSS strukturieren und formen können. Der nächste Abschnitt ist hingegen etwas theoretischer und behandelt unsere Einstellung und Herangehensweise beim Schreiben von CSS.

## Erstellen neuer Komponenten

Wenn du eine neue Komponente erstellst, schreibe dein Markup **bevor** du dein CSS schreibst. So kannst du leicht erkennen, welche CSS Eigenschaften automatisch vererbt werden und vermeidest dadurch das Wiederanwenden von überflüssigen Styles.

Außerdem kannst du dich dadurch voll und Ganz auf Daten, Inhalt und Semantik konzentrieren und _anschließend_ nur die relevanten Klassen und CSS verwenden.

## OOCSS

Ich arbeite nach OOCSS Manier. Ich teile Komponenten in Struktur (Objekte) und Skin (Erweiterungen) auf. Als **Analogie** (nicht als Beispiel) kannst du dir folgendes vorstellen:

    .room {}

    .room--kitchen {}
    .room--bedroom {}
    .room--bathroom {}

In diesem Beispiel haben wir verschiedene Arten von Räumen, wovon alle ähnliche Eigenschaften teilen. Alle haben Flure, Dächer, Wände und Türen. Diese Informationen können wir in einer abstrahierten Klasse `.room {}` festhalten. Es gibt jedoch auch besondere Arten von Räumen, die sich von den Anderen unterscheiden. Eine Küche hat unter Umständen einen gekachelten Boden, ein Schlafzimmer eher einen Teppich, ein Bad hat nicht unbedingt ein Fenster, aber ein Schlafzimmer höchstwahrscheinlich schon. Jeder Raum hat vermutlich unterschliedlich gestrichene Wände. OOCSS lehrt uns die geteilten Eigenschaften in einem Basisobjekt zu abstrahieren und diese Information dann mit spezifischeren Klassen zu erweitern.

Versuche also, anstatt dutzende gesonderte Komponenten zu erstellen, die sich wiederholenden Muster zu erkennen und in wiederverwendbaren Klassen zu abstrahieren. Erstelle diese "Skelette" als Basisobjekte, baue mit weiteren Klassen auf ihnen auf und erweitere somit ihr Styling unter anderen Gegebenheiten.

Wenn du eine neue Komponente erstellst, teile sie in Struktur und Aussehen. Erstelle die Struktur einer Komponente mit Hilfe von sehr allgemeinen Klassen, damit wir dieses Konstrukt wiederverwenden können und nutze dann spezifischere Klassen um das Aussehen zu erweitern und anzupassen.

## Layout

Alle Komponenten sollten ohne feste Breitenangaben erstellt werden. Sie sollten immer fluide bleiben und ihre Breite sollte vom jeweiligen Elternelement (z.B. Grid System) bestimmt werden.

`height` sollte **niemals** auf Elemente angewandt werden. Nur bei Elementen mit festen Abmessungen, wie Bildern oder Sprites, darf eine Höhe oder Breite definiert werden. Setze niemals die Höhe von `p`s, `ul`s, `div`s, und eigentlich auch allem Anderen. Oft kann der gewünschte Effekt auch über `line-height` erreicht werden, was darüber hinaus noch viel flexibler ist.

Grid Systeme sollte man sich wie Regale vorstellen. Sie besitzen Inhalte, sind aber nicht selbst Inhalte. Du stellst erst dein Regal auf und füllst es dann mit deinem Kram. Wenn wir also unser Grid separat von den Komponenten aufbauen, können die Komponenten viel leichter bewegt werden, als wenn wir deren Höhe oder Breite fest definiert hätten. Das macht unsere Front-Ends um einiges anpassungsfähiger und wir können effektiver daran arbeiten.

Du darfst Styles nie auf das Grid System selbst anwenden, dieses dient nur dazu um das Layout zu definieren. Wende Styles auf die Inhalte _innerhalb_ des Grid Systems an. Wende unter absolut _keinen_ Umständen box-model Eigenschaften auf ein Grid Element an.

## Größenangaben für Benutzeroberflächen

Ich verwende und kombiniere verschiedene Methoden um Größen in Benutzeroberflächen anzugeben. Prozente, Pixel, ems, rems, (und teilweise auch ohne die Angabe einer Einheit).

Die Breiten in Grid Systemen sollten idealerweise in Prozenten angegeben werden. Da ich ein Grid System nutze, um die Breiten von Spalten und Seiten zu beeinflussen, kann ich die Komponenten (wie oben beschrieben) ohne Angabe von Höhe oder Breite umsetzen.

Schriftgrößen gebe ich in rems mit Pixel-Fallback an. Das gewährleistet uns alle Vorteile der Barrierefreiheit von ems (with the confidence of pixels). Hier ist ein nützliches Sass-Mixin zur Berechnung von rem und Pixel-Fallback (davon ausgehend, dass du deine Basefontsize in einer Variable setzt):

    @mixin font-size($font-size) {
        font-size: $font-size +px;
        font-size: $font-size / $base-font-size +rem;
    }

Pixel verwende ich nur bei Elementen, die schon feste Abmessungen hatten bevor sie auf die Seite kamen. Hier handelt es sich um Elemente wie Bilder oder Sprites, deren Dimensionen von Haus aus in Pixel angegeben werden.

### Anpassen von Schriftgrößen

Ich definiere eine Reihe von Klassen - ähnlich wie beim Grid System - um die Schriftgrößen anzupassen. Diese Klassen können dann genutzt werden, um Überschriften unabhängig von ihrer Hierarchie zu stylen. Für weitere Informationen zu diesem Thema empfehle ich meinen Artikel [Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css).

## Shorthand

**Shorthand CSS muss immer mit Vorsicht genutzt werden.**

Deklarationen wie `background: red;` mögen erstmal verlocken, aber was du damit tatsächlich ausdrückst ist folgendes: ‘Ich möchte kein Bild das scrollt, links-oben ausgerichtet ist, sich nach X oder Y wiederholt, sondern eine rote Hintergrundfarbe’. In neun von zehn Fällen wird das auch problemlos funktionieren, aber der eine Fall, der Auswirkungen nach sich zieht ist nervig genug, um komplett auf solche Shorthands zu verzichten. Nutze stattdessen einfach `background-color: red;`.

Auch Deklarationen wie `margin: 0;` sind nett und kurz, aber verwende sie nur mit Bedacht. Wenn du eigentlich nur den Abstand nach unten beeinflussen möchtest, ist es besser `margin-bottom: 10px;` zu definieren, anstatt `margin: 0 0 10px 0`.

Sei dir im Klaren darüber, welche Eigenschaften du setzen willst und achte darauf, dass du nicht versehentlich andere Eigenschaften mit dem Shorthand überschreibst.

Shorthands sind nützlich aber auch leicht falsch zu verwenden.

## IDs

Eine kurze Bemerkung zu IDs in CSS, bevor wir uns um Selektoren im Allgemeinen kümmern:

**Benutze IDs NIEMALS in CSS.**

Du kannst Sie in deinem Markup als JS Hooks oder als Fragmentbezeichner verwenden, aber in deinen Stylesheets will ich niemals IDs sehen.

Klassen haben den Vorteil, dass sie wiederverwendbar sind (selbst wenn wir das nicht brauchen, die Möglichkeit bleibt bestehen) und kommen mit einer angenehmen, niedrigen Spezifität. Spezifität ist einer der schnellsten Wege um bei Projekten in Probleme zu geraten, daher ist es erforderlich sie zu allen Zeiten so gering wie möglich zu halten. Eine ID ist **255** mal spezifischer als eine Klasse, benutze sie deshalb _niemals_ in CSS.

## Selektoren

Halte deine Selektoren so kurz, effizient und wiederverwendbar wie möglich.

Selektoren, die stark von ihrer Position abhängig sind, sind aus mehreren Gründen unpraktisch. Nehmen wir zum Beispiel `.sidebar h3 span {}`. Dieser Selektor ist zu stark von seiner Position abhängig, weshalb wir die `span` nicht außerhalb einer `h3`, und diese wiederum nicht außerhalb von `.sidebar` verwenden können, ohne dabei unser Styling zu verlieren.

Zu lange Selektoren bringen außerdem Performance Problemen mit sich: Je mehr Abgleiche in einem Selektor enthalten sind (`.sidebar h3 span` hat beispielsweise drei Abgleiche, `.content ul p a` hat vier), desto mehr Arbeit hat der Browser damit.

Stelle deshalb sicher, dass deine Styles nicht abhängig von ihrer Position sind, wähle deine Selektoren mit Bedacht und halte sie kurz.

Selektoren als Ganzes sollten so kurz wie möglich gehalten werden (zum Beispiel nur eine Klasse tief), die Klasse selber jedoch sollte so lange wie nötig sein. Eine Klasse `.user-avatar` ist wesentlich angenehmer als `.usr-avt`.

**Merke**: Klassen sind weder semantisch noch unsemantisch, sie sind entweder sinnig oder unsinnig! Mach dir keine Gedanken über die "Semantik" einer Klasse, suche lieber nach etwas sinnigem und zukunftstauglichen.

### Zu spezifische Selektoren

Wie zuvor bereits erwähnt bedeuten spezifische Selektoren meißt nichts gutes.

Ein zu spezifischer Selektor ist beispielsweise `div.promo`. Du könntest vermutlich den selben Effekt erzielen, wenn du einfach  `.promo` verwenden würdest. Natürlich wird es manchmal Fälle geben, in denen du eine Klasse _bewusst_ in Verbindung mit einem Element spezifizierst, z.b. wenn du eine allgemeine `.error` Klasse hast, die - je nachdem auf welchem Element sie angewendet wird - unterschiedlich aussehen soll (z.B.
`.error { color: red; }` `div.error { padding: 14px; }`). Im Allgemeinen solltest du es allerdings, wo es möglich ist, vermeiden.

Ein weiteres Beispiel für einen zu spezifischen Selektor wäre `ul.nav li a {}`. Auch hier können wir die `ul` ersatzlos streichen und da wir wissen, dass `.nav` eine Liste ist und daraus schließen können, das `a` in einem `li` sein _muss_, können wir `ul.nav li a {}` auf `.nav a {}` vereinfachen.

### Performance von Selektoren

Obwohl es richtig ist, dass Browser in der Zukunft nur noch schneller werden was das Rendern von CSS betrifft, solltest du die Effizienz deines CSS trotzdem im Auge behalten.

Das Verwenden von kurzen, unverschachtelten Selektoren, das Verzichten auf den Universal-Selektor (`* {}`) als Schlüssel-Selektor, sowie das Vermeiden von komplexeren CSS3-Selektoren sollten dabei helfen, diese Probleme zu umgehen.

## Absicht eines CSS Selektors

Anstatt sich mit Selektoren am DOM entlang zu einem Element zu hangeln, ist es meistens ratsamer, dem Element, welches du stylen möchtest, eine Klasse zu geben. Lass uns das an einem konkreten Beispiel, mit einem Selektor wie `.header ul {}` veranschaulichen ...

Nehmen wir an, dass diese `ul` tatsächlich die Hauptnavigation unserer Webseite darstellt. Sie befindet sich - wie erwartet - im Header und ist dort momentan die einzige `ul`. `.header ul {}` funktioniert zwar, ist aber in diesem Fall weder ideal noch ratsam.

Es ist nicht zukunftsorientiert und definitiv nicht spezifisch genug. Sobald wir in diesem Header eine weitere `ul` einfügen, wird diese das Styling unserer Hauptnavigation annehmen, doch es besteht die hohe Wahrscheinlichkeit, dass wir das gar nicht möchten. Das bedeutet, dass wir entweder große Teile unseres Codes überarbeiten _oder_ viele Style-Anweisungen rückgängig machen müssen, um die Effekte des weitreichenden Selektors wieder zu entfernen.

Die Absicht deines Selektors sollte dem Grund entsprechen, aus dem du etwas stylen möchstes. Frag dich **‘wähle ich diese Selektoren, weil es eine `ul` innerhalb von `.header`, oder weil es die Hauptnavigation meiner Seite ist?‘**. Die Antwort darauf hilft dir dabei, deinen Selektor richtig zu wählen.

Stelle sicher, dass dein Schlüssel-Selektor niemals ein Element-/Typen-Selektor oder eine Objekt-/Abstraktions-Klasse ist. Selektoren wie `.sidebar ul {}` oder `.footer .media {}` möchtest du besser nicht in deinem Stylesheet haben.

Greife explizit auf das zu verändernde Element zu, nicht auf sein Elternelement. Gehe niemals davon aus, dass sich dein Markup nicht verändern wird. **Verwende Selektoren, die auf das zugreifen, was du verändern möchtest, nicht auf das was bereits vorhanden ist.**

Für eine ausführliche Beschreibung und Zusammenfassung, schau dir meinen Artikel [Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/) an.

## `!important`

Verwende `!important` nur auf Helper-Klassen. Es ist ok, wenn du `!important` vorsorglich verwendest, wie beispielsweise bei `.error { color: red !important }`, wenn du weißt, dass diese Anweisung **immer** Vorrang haben sollte.

`!important` nachträglich zu verwenden - zum Beispiel um Spezifitäts-Problemen abzuwenden - ist nicht empfohlen. 

Verändere deine CSS und versuche diese Probleme zu lösen, indem du deine Selektoren überarbeitest. Wenn du deine Selektoren kurz hältst und IDs vermeidest, wird dir das massiv helfen.

## Magische Zahlen und Konstanten

Eine magische Zahl ist eine Zahl, die verwendet wird, weil es _einfach funktioniert_. Diese sind schlecht, weil sie in den wenigsten Fällen aus einem bestimmten Grund funktionieren und in der Regel weder zukunftstauglich noch flexibel sind. Sie neigen dazu Symptome zu bekämpfen, keine Probleme.

Beispiel: `.dropdown-nav li:hover ul { top: 37px; }` dazu zu verwenden um ein Dropdown bei Hover unter die Navigation zu schieben ist schlecht, da 37px eine magische Nummer ist. 37px funktioniert hier nur, weil in diesem speziellen Szenario die `.dropdown-nav` 37px hoch ist.

Stattdessen solltest du `.dropdown-nav li:hover ul { top: 100%; }` verwenden, da das Dropdown dadurch immer mit einem Abstand von 100% ausgerichtet sein wir, egal wie hoch die `.dropdown-nav` wird.

Überlege dir genau, ob du eine feste Nummer vergibst. Wenn du es mit Hilfe von Keywords oder ‘Aliasen’ (z.B. `top: 100%` was soviel bedeutet wie ‘den kompletten Abstand von oben’) oder - noch besser - ohne Angabe von Abmessungen vermeiden kannst, solltest du das tun.

Jeder Wert, den du von Hand vergibst, ist ein Zugeständnis, das du möglicherweise nicht einräumen willst.

## Conditional Stylesheets

Spezielle IE Stylesheets können in den meisten Fällen komplett vermieden werden, es sei denn, du musst eine fehlende Browserunterstützung ausgleichen (z.B. PNG Fix).

Im Allgemeinen können und werden alle Layout- und box-model-Anweisungen auch ohne ein IE Stylesheet funktionieren, wenn du dein CSS refactorst und überarbeitest. Das bedeutet, dass du niemals `<!--[if IE 7]> element { margin-left: -9px; } < ![endif]-->` oder ähnliches CSS verwenden willst, dass offensichtlich grundloses Styling verwendet, nur damit Sachen wieder ‘funktionieren‘.

## Debugging

Wenn du ein CSS Problem entdeckst, **entferne zuallererst Code, bevor du damit anfängst noch mehr hinzuzufügen** um es zu lösen. Da das Problem in dem CSS besteht, das du schon geschrieben hast, ist mehr CSS nicht die richtige Antwort!

Lösche solange Markup und CSS, bis dein Problem verschwindet. Anschließend kannst du bestimmen in welchem Teil deines Codes das Problem liegt.

Es ist sehr verlockend einem Element `overflow: hidden;` oder ähnliches zu geben um damit die Effekte eines Layout Problems zu verstecken, aber overflow war möglicherweise nie das Problem - **Behandle das Problem, nicht seine Symptome**.

## Preprocessors

Sass ist der Preprocessor meiner Wahl. **Verwende ihn weise**. Benutze Sass um dein CSS mächtiger zu machen aber vermeide Verschachtelungen unter allen Umständen! Verschachtele nur, wenn es auch in vanilla CSS notwendig wäre, z.B.

    .header {}
    .header .site-nav {}
    .header .site-nav li {}
    .header .site-nav li a {}

wäre in normalem CSS absolut unnötig und daher wäre das Folgende **schlecht**

Sass:

    .header {
        .site-nav {
            li {
                a {}
            }
        }
    }

In Sass würdest du es so schreiben:

    .header {}
    .site-nav {
        li {}
        a {}
    }
