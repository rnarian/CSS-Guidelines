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
    * [Objekt/Extension Pointer](#objectextension-pointers)
* [Writing CSS](#writing-css)
* [Building new components](#building-new-components)
* [OOCSS](#oocss)
* [Layout](#layout)
* [Sizing UIs](#sizing-uis)
  * [Font sizing](#font-sizing)
* [Shorthand](#shorthand)
* [IDs](#ids)
* [Selectors](#selectors)
  * [Over qualified selectors](#over-qualified-selectors)
  * [Selector performance](#selector-performance)
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
**Komponenten** - Komplette Komponenten konstruiert aus Objekten und ihren Extensions.
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

Zum größten Teil verwende ich durch Bindestrich getrennte Klassen (z.B. `.foo-bar`, nicht `.foo_bar` oder `.fooBar`), allerdings nutze ich unter gewissen Umständen die BEM (Block, Element, Modifier) Notation.

<abbr title="Block, Element, Modifier">BEM</abbr> ist eine Methode um CSS Selektoren durch ihre Benennung und Klassifizierung viel exakter, transparent und informativ zu machen.

Die Namenskonvention folgt diesem Muster:

    .block {}
    .block__element {}
    .block--modifier {}

* `.block` repräsentiert die höhere Ebene einer Abstraktion oder einer Komponente.
* `.block__element` repräsentiert eine Kindelement von `.block` das hilft `.block` als ein Ganzes zu formen.
* `.block--modifier` repräsentiert einen anderen Status oder eine andere Version von `.block`.

Eine **Analogie** vom Aufbau mit BEM Klassen könnte sein:

    .person {}
    .person--woman {}
        .person__hand {}
        .person__hand--left {}
        .person__hand--right {}

Hier sehen wir, dass das grundlegende Objekt das wir beschreiben eine Person ist, und das eine anderer Typ der Person eine Frau sein könnte. Wir können auch sehen, dass diese Person Hände hat; Das sind weitere "Unterteile" von Person, und es gibt verschiedene Varianten, wie 'left' und 'right'.

Nun können wir unsere Selektoren basierend auf ihrerem Basis Objekt benennen und kommunizieren welche Aufgabe der Selektor hat; Ist er ein eine Unter-Komponente (`__`) oder eine Variation (`--`)?

Also, `.page-wrapper` ist ein unabhängiger Selektor; Er form keinen Teil einer Abstraktion oder einer Komponente und ist somit korrekt benannt. Der Selektor `.widget-heading` dagegen, _ist_ einer Komponente zugehörig; Er ist ein Kind von `.widget` also würden wir diese Klasse umbenennen in `.widget__heading`.

BEM sieht etwas hässlicher aus und ist viel ausführlicher, aber es garantiert uns viel Macht, da wir die Funktionen und Beziehungen der Elemente von ihrem Klassennamen allein erfassen können. Ausserdem wird die BEM Syntax normalerweise ziemlich gut komprimiert (gzip), sodass die vielen Wiederholungen nicht sonderlich ins Gewicht fallen.

Unabhängig davon, ob du BEM nutzt oder nicht, stelle immer sicher, dass Klassen vernünftig benannt sind; Halte sie so kurz wie möglich, aber so lang wie nötig. Stelle sicher, das alle Objekte oder Abstraktionen sehr unbestimmt benannt sind (z.B. `.ui-list`, `.media`) um größere Wiederverwendbarkeit zu gewährleisten. Extensions von Objekten sollten viel genauer benannt sein (z.B. `.user-avatar-link`). Mach dir keine Gedanken über die Länge von Klassennamen; gzip wird ordentlich geschriebenen Code _erstaunlich_ gut komprimieren.

### Klassen im HTML

Um Dinge lesbar zu halten, trenne Klassen im HTML mit zwei (2) Leerzeichen:

    <div class="foo--bar  bar__baz">

Der erweiterte Leerraum sollte das Ausmachen von mehreren Klassen erleichtern.

### JS hooks

**Nutze niemals CSS _styling_ Klassen als JavaScript hook.** Anfügen von JS Verhalten an eine styling Klassen bedeutet, dass wir das Eine nie ohne das Andere nutzen können.

Wenn du JavaScript ans Markup binden musst, nutze eine spezielle JS CSS Klasse. Das ist einfach eine Klasse mit vorangestelltem `.js-`, z.B. `.js-toggle`, `.js-drag-and-drop`. Das bedeutet, dass wir JS und CSS an Klassen hängen können, ohne mühsame Überlappungen.

    <th class="is-sortable  js-is-sortable">
    </th>

Das obige Markup hält zwei Klassen; Eine über die du Styles für sortierbare Table-Columns anhängen kannst, und eine Andere welche uns erlaubt Sortierungsfunktionalität hinzuzufügen.

### Internationalisierung

Obwohl ich ein Britischer Entwickler bin—und mein ganzes Leben <i>colour</i> statt <i>color</i> schreibe—denke ich, dass es besser ist, zum Wohle der Beständigkeit, im CSS immer US-English zu nutzen. CSS, wie die meisten (wenn nicht alle) anderen Sprachen wird in US-English geschrieben, sodass es Misch-Syntax wie `color: red;` in Klassen wie `.colour-picker {}` an Beständigkeit mangelt. Ich habe bislang nahegelegt Klassennamen zweisprachig auszuformulieren, zum Beispiel:

    .color-picker,
    .colour-picker {
    }

Allerdings wurde das Beibehalten von zwei Versionen von jeder Variable beim Arbeiten an einem sehr großen Sass Project mit duzenden Farb-Variablen (z.B. `$brand-color`, `$highlight-color` etc.) schnell lästig. Es bedeutet auch doppelt so viel arbeit bei Dingen wie "Finden und Ersetzen".

Benenne Klassen und Variablen deshalb immer in der Sprache in der du arbeitest.

## Kommentare

Ich nutze einen docBlock-ähnlichen Kommentar Stil welchen ich auf 80 Zeichen beschränke:

    /**
     * Das ist ein docBlock Kommentar
     *
     * Das ist eine längere Beschreibung des Kommentars, der den Code genauer 
     * beschreibt. Wir limitieren diese Zeilen auf maximal 80 Zeichen. 
     *
     * Wir können markup in Kommentaren nutzen und sind ermutigt das 
     * folgendermaßen zu tun:
     *
       <div class=foo>
           <p>Lorem</p>
       </div>
     *
     * Wir stellen Codezeilen kein Sternchen vor, um Copy und Paste zu 
     * ermöglichen.
     */

Du solltest deinen Code so gut wie möglich kommentieren und dokumentieren. Was für dich transparent oder selbsterklärend wirkt, ist es womöglich nicht für einen anderen Entwickler. Schreib einen Brocken Code und dann schreib darüber. 

### Kommentare auf Steroiden

Es gibt eine Reihe von fortgeschritteneren Techniken die du verwenden kannst, und zwar:

* Quasi-Qualifizierte Selektoren
* Code markieren
* Objekt/Extension Hinweise

#### Quasi-Qualifizierte Selektoren

Du solltest deine Selektoren niemals einschränken; also niemals `ul.nav {}` schreiben, wenn du einfach `.nav` nutzen kannst. Das einschränken von Selektoren mindert die Leistung, verhindert das Potenzial die Klasse auf einem anderen Element-Typ wiederzuverwenden und die Spezifität des Selektors nimmt zu. Das sind alles Dinge, die unter allen Umständen vermieden werden sollten.

Jedoch ist es manchmal nützlich dem nächsten Entwickler zu zeigen wo eine Klasse genutzt werden soll. Nehmen wie `.product-page` als Beispiel; Diese Klasse hört sich so an, als würde sie auf einem Container auf hoher Ebene genutzt werden, eventuell das `html` oder `body` Element, aber mit `.product-page` alleine ist das unmöglich zu sagen.

Indem wir den Selektor quasi-qualifizieren (auskommentieren des vorangestellten Selektor Typs) können wir kommunizieren wo wir die Klasse nutzen möchten:

    /*html*/.product-page {}

So sehen wir genau wo die Klasse angewand werden soll, ohne die Nachteile der Spezifität oder nichtvorhandenen Wiederverwendbarkeit.

Andere Beispiele können sein:

    /*ol*/.breadcrumb {}
    /*p*/.intro {}
    /*ul*/.image-thumbs {}

Hier können wir sehen, wo wir jede dieser Klassen zu platzieren beabsichtigen, ohne jemals die Spezifität der Selektoren zu beeinflussen.

#### Code Tags

Wenn du eine neue Komponente schreibst, hinterlasse einige Tags die ihre Funktionalität beschreiben in einem Kommentar oberhalb, zum Beispiel:

    /**
     * ^navigation ^lists
     */
    .nav {}

    /**
     * ^grids ^lists ^tables
     */
    .matrix {}

Diese Tags erlauben anderen Entwicklern Code-Snippets zu finden indem sie nach Funktionalität suchen; Wenn ein Entwickler mit Listen arbeiten muss, kann er oder sie eine Suche nach `^lists` starten und findet die `.nav` und `.matrix` Objekte (und vermutlich noch mehr).

#### Objekt/Extension Pointer

Beim Arbeiten in einer Objektorientieren Art und Weise wirst du oft zwei Brocken von CSS haben (einer das Skelett (das Objekt) und einer die Haut (die Extension)) die sehr eng miteinander verbunden, aber an ganz unterschiedlichen Orten abgelegt sind. Um eine feste Verbindung zwischen dem Objekt und seiner Extension herzustellen nutze <i>Objekt/Extension Pointer</i>. Diese sind einfach Kommentare die folgendermaßen funktionieren:

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

Der vorherige Abschnitt behandelte wie wir unser CSS strukturieren und formen; Sie waren quantifizierbare Regeln. Der nächste Abschnitt ist etwas mehr theoretisch und behandelt unsere Einstellung und Herangehensweise.

## Erstellen von neuen Komponenten

Wenn du eine neue Komponente erstellst, schreibe dein Markup **vor** dem CSS. So kannst du sehen welche CSS Eigenschaften natürlich vererbt werden und vermeidest dadurch das wiederanwenden von überflüssigen Styles.

Durch das vorherige niederschreiben vom Markup kannst du dich auf Daten, Inhalt und Semantik konzentrieren und _danach_ nur die relevanten Classen und CSS verwenden.

## OOCSS

Ich arbeite auf OOCSS Art und Weise; Ich teile Komponenten in Struktur (Objekte) und Skin (Extensions). Als **Analogie** stell dir folgendes vor: 

    .room {}

    .room--kitchen {}
    .room--bedroom {}
    .room--bathroom {}

In diesem Beispiel haben wir verschiedene Arten von Räumen, wovon alle ähnliche Eigenschaften teilen; Alle haben Flure, Dächer, Wände und Türen. Wir können diese Informationen in einer abstrahierten Klasse `.room {}` festhalten. Jedoch haben wir auch besondere Arten von Räumen, die sich von den Anderen unterscheiden; Eine Küche hat unter Umständen einen gekachelten Boden, ein Schlafzimmer jedoch eher einen Teppich, ein Bad hat nicht unbedingt ein Fenster, aber ein Schlafzimmer höchstwahrscheinlich schon. Jeder Raum hat vermutlich unterschliedlich gestrichene Wände. OOCSS lehrt uns die geteilten Eigenschaften in einem Basisobjekt zu abstrahieren und diese Information dann mit spezifischeren Klassen zu erweitern.

Also, statt duzende gesonderte Komponenten zu erstellen, versuche die sich wiederholenden Muster zu erkennen und abstrahiere sie in wiederverwendbare Klassen; Erstelle diese "Skelette" als Basisobjekte und baue mit weiteren Klassen auf ihnen auf und erweitere somit ihr Styling unter anderen Gegebenheiten.

Wenn du eine neue Komponente erstellst, teile sie in Struktur und Aussehen; Erstelle die Struktur einer Komponente mit Hilfe von allgemeineren Klassen und nutze dann spezifischere Klassen um das Aussehen zu erweitern und anzupassen.

## Layout

Alle Komponenten sollten ohne Angabe von `width` erstellt werden; Sie sollten immer fluide bleiben und die Breite sollte vom Elterelement (z.B. Grid System) bestimmt werden.

`height` sollte auf Elemente **niemals** angewandt werden. Nur bei Elementen mit festen Maßen, wie Bildern oder Sprites, darf die Höhe oder Breite definiert werden. Setze niemals die Höhe von `p`s, `ul`s, `div`s, und eigentlich auch allem Anderen. Oft kann der gewünschte Effekt über `line-height` erreicht werden, was darüber hinaus noch viel flexibler ist.

Grid Systeme sollte man sich wie Regale vorstellen. Sie enthalten Inhalte aber sind aber nicht selbst Inhalte. Du stellst erst dein Regal auf und füllst es dann mit deinem Kram. Wenn wir also unser Grid seperat von den Komponenten aufbauen, können die Komponenten viel leichter bewegt werden, als wenn wir die Höhe oder Breite definiert hätten; Das macht unser Front-End viel anpassungsfähiger und wir können effektiver daran arbeiten.

Du darfst Styles nie auf das Grid System selbst anwenden, sie dienen nur dazu das Layout zu definieren. Wende Styles auf die Inhalte _im_ Grid System an. Wende niemals, unter _keinen_ Umständen, box-model Eigenschaften auf Grid System Elemente an.

## Sizing UIs

I use a combination of methods for sizing UIs. Percentages, pixels, ems, rems
and nothing at all.

Grid systems should, ideally, be set in percentages. Because I use grid systems
to govern widths of columns and pages, I can leave components totally free of
any dimensions (as discussed above).

Font sizes I set in rems with a pixel fallback. This gives the accessibility
benefits of ems with the confidence of pixels. Here is a handy Sass mixin to
work out a rem and pixel fallback for you (assuming you set your base font
size in a variable somewhere):

    @mixin font-size($font-size) {
        font-size: $font-size +px;
        font-size: $font-size / $base-font-size +rem;
    }

I only use pixels for items whose dimensions were defined before the came into
the site. This includes things like images and sprites whose dimensions are
inherently set absolutely in pixels.

### Font sizing

I define a series of classes akin to a grid system for sizing fonts. These
classes can be used to style type in a double stranded heading hierarchy. For a
full explanation of how this works please refer to my article
[Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)

## Shorthand

**Shorthand CSS needs to be used with caution.**

It might be tempting to use declarations like `background: red;` but in doing so
what you are actually saying is ‘I want no image to scroll, aligned top-left,
repeating X and Y, and a background colour of red’. Nine times out of ten this
won’t cause any issues but that one time it does is annoying enough to warrant
not using such shorthand. Instead use `background-color: red;`.

Similarly, declarations like `margin: 0;` are nice and short, but
**be explicit**. If you actually only really want to affect the margin on
the bottom of an element then it is more appropriate to use `margin-bottom: 0;`.

Be explicit in which properties you set and take care to not inadvertently unset
others with shorthand. E.g. if you only want to remove the bottom margin on an
element then there is no sense in setting all margins to zero with `margin: 0;`.

Shorthand is good, but easily misused.

## IDs

A quick note on IDs in CSS before we dive into selectors in general.

**NEVER use IDs in CSS.**

They can be used in your markup for JS and fragment identifiers but use only
classes for styling. You don’t want to see a single ID in any stylesheets!

Classes come with the benefit of being reusable (even if we don’t want to, we
can) and they have a nice, low specificity. Specificity is one of the quickest
ways to run into difficulties in projects and keeping it low at all times is
imperative. An ID is **255** times more specific than a class, so never ever use
them in CSS _ever_.

## Selectors

Keep selectors short, efficient and portable.

Heavily location-based selectors are bad for a number of reasons. For example,
take `.sidebar h3 span {}`. This selector is too location-based and thus we
cannot move that `span` outside of a `h3` outside of `.sidebar` and maintain
styling.

Selectors which are too long also introduce performance issues; the more checks
in a selector (e.g. `.sidebar h3 span` has three checks, `.content ul p a` has
four), the more work the browser has to do.

Make sure styles aren’t dependent on location where possible, and make sure
selectors are nice and short.

Selectors as a whole should be kept short (e.g. one class deep) but the class
names themselves should be as long as they need to be. A class of `.user-avatar`
is far nicer than `.usr-avt`.

**Remember:** classes are neither semantic or insemantic; they are sensible or
insensible! Stop stressing about ‘semantic’ class names and pick something
sensible and futureproof.

### Over-qualified selectors

As discussed above, qualified selectors are bad news.

An over-qualified selector is one like `div.promo`. You could probably get the
same effect from just using `.promo`. Of course sometimes you will _want_ to
qualify a class with an element (e.g. if you have a generic `.error` class that
needs to look different when applied to different elements (e.g.
`.error { color: red; }` `div.error { padding: 14px; }`)), but generally avoid it
where possible.

Another example of an over-qualified selector might be `ul.nav li a {}`. As
above, we can instantly drop the `ul` and because we know `.nav` is a list, we
therefore know that any `a` _must_ be in an `li`, so we can get `ul.nav li a {}`
down to just `.nav a {}`.

### Selector performance

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
