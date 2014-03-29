## IndexedDB

Die IndexedDB ist eine HTML5 Spezifikation und befindet sich im Entwicklungsprozess des W3C in der 'Last Call Working Draft' Phase (http://www.w3.org/TR/IndexedDB/). Die Implementierung in den aktuellen Browsern ist relativ konsistent umgesetzt, d.h. es gibt keine merkbaren Unterschiede. Lediglich Apple mit Safari und iOS und Opera Mini bieten aktuell keine Unterstützung für die IndexedDB-Spezifikation.[http://caniuse.com/#feat=indexeddb]

IndexedDB macht es möglich große Datenmengen im Browser des Nutzers zu speichern. Die dort abgelegten Daten können mithilfe einer API durchsucht werden.

Im Gegensatz zu Cookies oder Webstorage besteht für die IndexedDB theoretisch kein Limit was die Größe des verfügbaren Datenspeichers betrifft, kann aber durch den Browser begrenzt sein. Jede Applikation die notwendigerweise große Datenmengen übertragen muss, kann davon profitieren diese Daten dauerhaft in dem Browser des Nutzers zu speichern.(https://developer.mozilla.org/en-US/docs/IndexedDB/Basic_Concepts_Behind_IndexedDB) Ebenso hat der Webstorage keinen Suchmechanismus, sondern nur die Möglichkeit über einen *Key* auf einen *Value*
zuzugreifen. Die IndexedDB kommt dem Begriff einer "Datenbank" also sehr viel näher als der einfache Webstorage.
