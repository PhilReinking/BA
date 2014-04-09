## Technische Umsetzung Pinboard

Das Pinboard ist nach dem Dashboard die zweite Komponente und demonstriert die Funktionsweise der Datenbank und Synchronisierung.

Der Code des Controllers beinhaltet lediglich 2 Funktionsaufrufe und stellt eine Funktion zum Löschen der Beiträge bereit. Dadurch das die Logik für die Datenbank komplett im Service ppSyncService ausgelagert wurde, ist der Controller somit sehr übersichtlich.

Mit *ppSyncService.getDocuments()* werden alle Dokumente vom Typ *POST* angefragt. Der Rückgabewert dieses Aufrufs wird über die Promise Implementierung abgefangen und dann in *$scope.pins* gespeichert. Die weitere Verarbeitung kann dann in der View erfolgen.

Änderungen an der Datenbank werden mit *ppSyncService.fetchChanges()* registriert und über den Notify Promise abgefangen. Es muss dann an dieser Stelle noch überprüft werden ob das neue Dokument nicht gelöscht wurde und vom Typ *POST* ist, bevor es in das Array *$scope.pins* geschoben wird.

Um ein Dokument zu löschen, wird die Funktion *deleteDocument()* deklariert. Diese Funktion benutzt *ppSyncService.deleteDocument()* und erwartet das zu löschene Dokument als Parameter.

```javascript
angular.module('banetApp')
  .controller('PinboardCtrl', function($scope, ppSyncService) {
    ppSyncService.getDocuments(['POST']).then(function(response) {
      $scope.pins = response;
    });
    ppSyncService.fetchChanges().then(function(response) {
      console.log(response);
    }, function(error) {
      console.log(error);
    }, function(change) {
      if ($scope.pins) {
        if (!change.deleted && change.doc.type === 'POST') {
          return $scope.pins.unshift(change);
        }
      }
    });
    $scope.deleteDocument = function(pin) {
      ppSyncService.deleteDocument(pin.doc);
    };
  });
```

In der View werden einige Direktiven benutzt um die im Array *$scope.pins* gespeicherten Dokumente auszugeben.

Die Aufgabe der View ist es die Daten aus dem Array *$scope.pins* optisch ansprechend auszugeben. AngularJS bietet zur Ausgabe von Arrays die sehr nützliche Direktive *ngRepeat*, welche für jedes Element in einem Array die HTML-Vorlage einmal ausgibt. Jedes Element erhält dabei seinen eigenen Scope und kann so die anderen Elemente nicht beeinflussen.

Zusätzlich kommt in *ngRepeat* der Filter *orderBy* zum Einsatz, der es ermöglicht das Array nach dem timestamp zu sortieren.

Da der Benutzer bei der Anmeldung eine E-Mail Adresse angeben kann, wird über den Internetservice Gravatar versucht ein Avatar für den Benutzer zu finden. Die Direktive *gravatar-image* enthält das Attribut *email*, in dem über *pin.doc.user.email* die Email des Autoren ausgelesen wird. Die Umwandlung zu einem gültigen HTML Image Tag geschieht innerhalb der Direktive.

```html
<div class="pin ng-cloak" ng-repeat="pin in pins | orderBy:'doc.created':true" ng-hide="pin.deleted">
  <div class="media">
    <a class="pull-left" href="#/pinboard/show/{{pin.id}}">
      <gravatar-image email="pin.doc.user.email" />
    </a>
    <div class="media-body">
      <h5 class="media-heading">
        <span class="glyphicon glyphicon-user"></span> {{pin.doc.user.name}}
        <small>{{pin.doc.created | date:'dd. MMMM yyyy @ HH:mm:ss'}}</small>
        <button type="button" class="btn btn-xs btn-link pull-right" ng-click="deleteDocument(pin); pin.deleted = true">delete</button>
      </h5>
      <p class="lead">{{pin.doc.msg}}</p>
    </div>
  </div>
</div>
```

Zum Erstellen eines neuen Beitrags wird einen separate Seite mit dem Controller *PinboardNewCtrl* aufgerufen. Das Attribut isSubmitting wird dazu verwendet um der View mitzuteilen ob ein Beitrag gerade in die Datenbank gespeichert wird.

Die Funktion zum Speichern eines Beitrags *$scope.newPin()* erweitert das zu speichernde Objekt *pin* um einige wichtige Parameter wie den aktuellen Zeitstempel, Beitragstyp, und Benutzerdaten. 

Wenn das Speichern des Dokuments erfolgreich war, erfolgt eine Umleitung zurück auf die Pinboard View.

```javascript
angular.module('banetApp')
  .controller('PinboardNewCtrl', function($scope, $location, ppSyncService, simpleUser) {
    $scope.isSubmitting = false;
    $scope.newPin = function() {
      $scope.isSubmitting = true;
      $scope.pin.created = Date.now();
      $scope.pin.user = simpleUser.getUserData();
      $scope.pin.type = 'POST';
      ppSyncService.postDocument($scope.pin).then(function() {
        $location.path('pinboard');
      });
    };
  });
```
