## Visuelle Umsetzung
Die App setzt sich aus 3 wesentlichen Bereichen zusammen:

- Die Navigation im Kopfbereich
- Der Hauptbereich für Inhalte der aktiven Komponente
- Der Fußbereich für optionale Inhalte

Es ist sinnvoll, diese 3 Bereiche vertikal anzuordnen um so jedem Bereich die Möglichkeit zu geben sich auf die volle verfügbare Breite des Anzeigerätes zu erstrecken.

[GRAFIK Mockup01]

Der Kopfbereich beinhaltet die Navigation und optional ein Logo der Applikation. Hier soll jeder Komponente ein Link zugeordnet werden, welcher bei Bedarf auch ein weiteres Dropdown Menü öffnen kann.

Der Hauptbereich ist ausschliesslich für die Inhalte der Komponente reserviert. Das Layout wird von der Komponente selbst bestimmt und kann sich dementprechend von anderen Seiten unterscheiden. Die für diese Arbeit erstellten Komponenten setzen auf eine ebenfalls vertikale Anordnung der Elemente, um so die maximale verfügbare Breite auszunutzen.

Der Fußbereich ist ein optionaler Bereich, der beispielsweise mit ergänzenden Inhalten gefüllt werden kann.

[GRAFIK Responsive]

Mit Hilfe des Frontend Frameworks Twitter Bootstrap (Version 3.1.1) kann das geplante Layout responsiv umgesetzt werden. Die verfügbare Bildschirmbreite wird dabei in *BAnet* auf kleineren Geräten immer voll ausgenutzt, auf größeren Geräten wie z.B. einem Desktop Computer wird diese jedoch auf 700px begrenzt um die Elemente nicht zu sehr zu strecken.

Als Einstieg in die Applikation ist die erste Komponente, das Dashboard, gedacht. Das Dashboard wird standardmäßig geladen und beinhaltet das Loginformular wenn der Benutzer nicht angemeldet ist oder, falls eine Anmeldung erfolgt ist, Informationen zur Applikation mit Debugfunktionen. Diese Debugfunktionen sind während der Entwicklung der App sehr wichtig, da es beispielsweise oft notwendig ist die lokale Datenbank zu löschen.

```html
<div class="panel panel-primary" ng-hide="userIsOnline()">
  <div class="panel-heading">
    <h3 class="panel-title">Login to BAnet</h3>
  </div>
  <div class="panel-body">
    <form ng-submit="login()">
      <div class="form-group">
        <label for="userName">Username</label>
        <input type="text" class="form-control" id="userName" placeholder="Username" ng-model="user.name" required>
      </div>
      <div class="form-group">
        <label for="userMail">E-Mail</label>
        <input type="text" class="form-control" id="userMail" placeholder="E-Mail" ng-model="user.email" required>
      </div>
      <button type="submit" class="btn btn-primary" ng-class="{disabled: !user.name || !user.email}">Login {{user.name}}<span ng-show="user.email">@</span>{{user.email}}</button>
    </form>
  </div>
</div>
```

In ABBILDUNG ist der HTML-Code des finalen Login-Formulars. In diesem Beispiel werden schon einige AngularJS Funktionen benutzt, welche hier nochmals im Detail erklärt werden sollen.

Der Einsatz von Direktiven ist in AngularJS eine einfache Möglichkeit um vorhandene Elemente zu manipulieren oder funktional zu erweitern. AngularJS stellt eine Vielzahl von einsatzfähigen Direktiven, erkennbar an dem Prefix *ng-*, bereit, es lassen sich aber auch eigene Direktiven erstellen.

*ng-hide* ist eine Direktive, welche Elemente ausblendet wenn der übergebene Parameter als *wahr* ausgewertet wird. Das Login Formular nutzt diese Direktive um abzufragen ob der Nutzer schon angemeldet ist, und setzt für den div Container den CSS Stil `display: none!important` ein. Diese Direktive gibt es auch in der Variante *ng-show*, bei der ein Element nur dann gezeigt wird wenn der Parameter als *wahr* ausgewertet wird.

Das Formular selbst wird mit der Direktive *ng-submit* belegt. Wenn das Formular, durch ein Klick auf den Submit-Button oder durch Drücken der Enter-Taste, abgesendet wird, so wird die hier als Parameter hinterlegte Funktion aufgerufen.

Die 2 Input-Elemente werden mit der *ng-model* Direktive ausgestattet und bewirkt, dass die Inhalte der Input Elemente direkt in die hier deklarierten Models gespeichert werden. Der Controller erhält dann natürlich Zugriff auf diese Models und kann diese verarbeiten.

Als letzte hier verwendetete Direktive kommt *ng-class* zum Einsatz, welche ähnlich wie *ng-show* und *ng-hide* funktioniert, jedoch hier im Falle einer als wahr ausgewerteten Bedingung, die mitübergebene CSS-Klasse gesetzt wird. Der Submit-Button wird im Login-Formular mithilfe von *ng-class* erst dann aktiviert, wenn beide Input-Felder ausgefüllt wurden.
