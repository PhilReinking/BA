## Technische Umsetzung Channel

Der Channel ist eine zusätzliche Komponente, die man als eine Art Erweiterung der Pinboard Komponente betrachten kann.

Der Benutzer hat die Möglichkeit einen beliebigen String in ein Input-Feld einzugeben um dadurch in einen Channel zu wechseln. Beiträge die in einem Channel erstellt werden, sind dann auch nur in diesem Channel sichtbar.

Zur Verwaltung des aktiven Channels wird das Objekt *channel* verwendet, welches den String mit Kleinbuchstaben und als lesbaren String mit großen Anfangsbuchstaben enthält.

Beiträge des aktiven Channels werden über den Funktionsaufruf *ppSyncService.getChannelStream()* geladen und in *$scope.entries* gespeichert.

Änderungen werden wie schon im Pinboard Controller über *ppSyncService.fetchChanges()* registriert.

Das Wechseln zu einem Channel erfolgt über die in der Applikation definierte Route für Channels */channel/:name*. *:name* ist hier ein Platzhalter für den Namen des Channels und wird dann über das Objekt *$routeParams* ausgelesen.

```javascript
$scope.channel = {
  name: $routeParams.name.toLowerCase(),
  humanizedName: Helpertools.humanize($routeParams.name.toLowerCase())
};
$scope.switchChannel = function() {
  $location.path('channel/' + $scope.newChannel.toLowerCase());
};
```

```html
<form ng-submit="switchChannel()">
  <div class="input-group input-group-lg">
    <span class="input-group-addon">#</span>
    <input type="text" class="form-control" ng-model="newChannel" placeholder="Switch Channel">
    <span class="input-group-btn">
      <button class="btn btn-default" type="submit">Submit</button>
    </span>
  </div>
</form>
```
