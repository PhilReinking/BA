# Final Text
Die IndexedDB ist eine HTML5 Spezifikation und befindet sich im Entwicklungsprozess des W3C in der 'Last Call Working Draft' Phase (http://www.w3.org/TR/IndexedDB/). Die Implementierung in den aktuellen Browsern ist relativ konsistent umgesetzt, d.h. es gibt keine merkbaren Unterschiede. Lediglich Apple mit Safari und iOS und Opera Mini bieten aktuell keine Unterstützung für die IndexedDB-Spezifikation.[http://caniuse.com/#feat=indexeddb]

IndexedDB macht es möglich große Datenmengen im Browser des Nutzers zu speichern. Die dort abgelegten Daten können mithilfe einer indexbasierten API durchsucht werden.

Im Gegensatz zu Cookies oder Local Storage besteht für die IndexedDB theoretisch kein Limit was die Größe des verfügbaren Datenspeichers betrifft, kann aber durch die Webbrowser begrenzt sein. Jede Applikation die notwendigerweise große Datenmengen übertragen muss, kann davon profitieren diese Daten dauerhaft in dem Browser des Clients zu speichern. Ebenso hat der Local Storage keinen Suchmechanismus, sondern nur die Möglichkeit über einen 'key' auf einen 'value' zuzugreifen. Die IndexedDB kommt dem Begriff einer "Datenbank" also sehr viel näher als der einfache Local Storage.

# Annotations

(https://developer.mozilla.org/de/docs/IndexedDB#Browser_compatibility)

(https://developer.mozilla.org/en-US/docs/IndexedDB/Basic_Concepts_Behind_IndexedDB)

(http://code.tutsplus.com/tutorials/working-with-indexeddb--net-34673)
In a nutshell, IndexedDB provides a way for you to store large amounts of data on your user's browser. Any application that needs to send a lot of data over the wire could greatly benefit from being able to store that data on the client instead. Of course storage is only part of the equation. IndexedDB also provides a powerful indexed based searching API to retrieve the data you need.

You may wonder how IndexedDB differs from other storage mechanisms?

Cookies are extremely well supported, but have legal implications and limited storage space. Also - they are sent back and forth to the server with every request, completely negating the benefits of client-side storage.

Local Storage is also very well supported, but limited in terms of the total amount of storage you can use. Local Storage doesn't provide a true "search" API as data is only retrieved via key values. Local Storage is great for "specific" things you may want to store, for example, preferences, whereas IndexedDB is better suited for Ad Hoc data (much like a database).

Before we go any further though, let's have an honest talk about the state of IndexedDB in terms of browser support. As a specification, IndexedDB is currently a Candidate Recommendation. At this point the folks behind the specification are happy with it but are now looking for feedback from the developer community. The specification may change between now and the final stage, W3C Recommendation. In general, the browsers that support IndexedDB now all do in a fairly consistent manner, but developers should be prepared to deal with prefixes and take note of updates in the future.

As for those browsers supporting IndexedDB, you've got a bit of a dilemma. Support is pretty darn good for the desktop, but virtually non-existent for mobile. Let's see what the excellent site CanIUse.com says:

Chrome for Android does support the feature, but very few people are currently using that browser on Android devices. Does the lack of mobile support imply you shouldn't use it? Of course not! Hopefully all our readers are familiar with the concept of progressive enhancement. Features like IndexedDB can be added to your application in a manner that won't break in non-supported browsers. You could use wrapper libraries to switch to WebSQL on mobile, or simply skip storing data locally on your mobile clients. Personally I believe the ability to cache large blocks of data on the client is important enough to use now even without mobile support.
