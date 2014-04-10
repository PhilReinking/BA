## Technische Umsetzung Dashboard

Controller werden mit dem in der Applikation genutzen AngularJS Modul *banetApp* erstellt. Dieses Modul besitzt die Funktion *controller()*, welche genau 2 Parameter erwartet. Der erste Parameter ist der Name des Controllers, welcher im Gültigkeitsbereich von *banetApp* eindeutig sein muss. Der zweite Parameter ist eine anonyme Funktion, die den Code für den Controller enthält. Zusätzlich können in dieser anonymen Funktion AngularJS Services übergeben werden, die dem Controller dann zur Verfügung stehen.

Das Scope-Objekt ist eines der wichtigsten Referenzen, denn darüber erhält der Controller Zugriff auf den Gültigkeitsbereich der zu steuernden View. Models und Funktionen die in der View verfügbar sind, werden in *$scope* abgelegt.

```javascript
angular.module('banetApp')
  .controller('DashboardCtrl', function($scope, ppSyncService, dummyContent, simpleUser) {
    // Controller Code
    $scope.hello = 'Hello World!';
  };
```

Der Dashboard Controller enthält insgesamt 4 Funktionen, welche zur Verwaltung des Benutzers benötigt werden.

*simpleUser* ist dabei ein einfacher Service, der die Logindaten des Benutzers ohne Verifizierung im LocalStorage speichert. Möchte man eine richtige Authentifizierung erreichen, muss man den Service simpleUser ersetzen.

In der Funktion *loadUserData()* werden die Benutzerdaten ausgelesen und in der Variablen *$scope.user* gespeichert. Die View kann dann über *user* auf diese Daten zugreifen.

*login()* speichert die Daten, die sich aktuell in der Variablen *$scope.user* befinden und loggt den Benutzer damit ein.

*logout()* löscht die Logindaten des Bneutzers.

*userIsOnline()* gibt einen booleschen Wert zurück, ob der Benutzer eingeloggt ist oder nicht.

```javascript
$scope.loadUserData = function() {
  if ($scope.userIsOnline()) {
    $scope.user = simpleUser.getUserData();
  }
};
$scope.login = function() {
  // Set the online flag to true
  $scope.user.online = true;
  simpleUser.login($scope.user);
};
$scope.logout = function() {
  $scope.user = simpleUser.logout();
};
$scope.userIsOnline = function() {
  return simpleUser.isOnline();
};
```

In Abbildung wird ersichtlich wie mithilfle von AngularJS Expression die im Controller zur Verfügung gestellten Benutzerdaten ausgegeben werden können. Alles was innerhalb einer *{{ expression }}* steht, wird von AngularJS als JavaScript interpretiert und durch das ausgewertete Ergebnis dieser Expression ersetzt.

Mit *ng-show="userIsOnline()"* wird sichergestellt, dass nur eingeloggte Benutzer diese Informationen angezeigt bekommen. 

```html
<div ng-show="userIsOnline()">
  <p>Your are logged in as <strong>{{user.name}}</strong> with the id <strong>{{user.id}}</strong>.</p>
</div>
```
