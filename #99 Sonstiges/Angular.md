#### Angular Konzept
1. Create views based on what the user needs to see and do.2. Define controllers to provide those views with the properties and functions that they require.3. Build models to store data requested by the controllers.
#### Router Example
```
var guidebookConfig = function($routeProvider) {	$routeProvider		.when('/', {			controller: 'ChaptersController',			templateUrl: 'view/chapters.html'		})		.when('/chapter/:chapterId', {			controller: 'ChaptersController',
		 	templateUrl: 'view/chapters.html'       	})       	.when('/addNote/:chapterId', {         	controller: 'AddNoteController',         	templateUrl: 'view/addNote.html'       	})    	.when('/deleteNote/:chapterId/:noteId', {      		controller: 'DeleteNoteController',      		templateUrl: 'view/addNote.html'    	}); 
	};var Guidebook = angular.module('Guidebook', [])
	.config(guidebookConfig);
```

#### Controller Example

```
Guidebook.controller('ChaptersController',     function ($scope, $location, $routeParams, ChapterModel,     NoteModel) {       var chapters = ChapterModel.getChapters();       for (var i=0; i<chapters.length; i++) {       chapters[i].notes =       NoteModel.getNotesForChapter(chapters[i].id);       }       $scope.chapters = chapters;       $scope.selectedChapterId = $routeParams.chapterId;       $scope.onDelete = function(noteId) {       var confirmDelete = confirm('Are you sure you want to delete       this note?');         if (confirmDelete) {           $location.path('/deleteNote/' + $routeParams.chapterId +           '/' + noteId);	} };} );
```

```
Guidebook.controller('AddNoteController',     function ($scope, $location, $routeParams, NoteModel) {       var chapterId = $routeParams.chapterId;       $scope.cancel = function() {         $location.path('/chapter/' + chapterId);       }       $scope.createNote = function() {         NoteModel.addNote(chapterId, $scope.note.content);         $location.path('/chapter/' + chapterId);	}});```

#### Angular Switch Statement

```
<a href='#/chapter/{{chapter.id}}'>     {{chapter.notes.length}}     <ng-switch on='chapter.notes.length'>       <span ng-switch-when='1'>note</span>       <span ng-switch-default>notes</span>     </ng-switch></a>```
#### Directive for loading a Sub-Template
```
Guidebook.directive('gbNoteList', function() {	return {    	restrict: 'E',       	templateUrl: 'view/directives/noteList.html',       	scope: {         	show: '=show',         	notes: '=notes',         	orderValue: '@orderBy',         	onDelete: '=deleteNoteHandler'	   	}
	};});
```

In Main Template View use 

```
<gb-note-list notes='chapter.notes'  	show='chapter.id == selectedChapterId'    order-by='id'    delete-note-handler='onDelete' />'
``` 
to render the HTML.