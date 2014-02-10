#### Angular Konzept
1. Create views based on what the user needs to see and do.


var guidebookConfig = function($routeProvider) {
		 	templateUrl: 'view/chapters.html'
	};
	.config(guidebookConfig);
```

#### Controller Example

```
Guidebook.controller('ChaptersController',
```

```
Guidebook.controller('AddNoteController',

#### Angular Switch Statement

```
<a href='#/chapter/{{chapter.id}}'>


Guidebook.directive('gbNoteList', function() {
	};
```

In Main Template View use 

```
<gb-note-list notes='chapter.notes'
``` 
to render the HTML.