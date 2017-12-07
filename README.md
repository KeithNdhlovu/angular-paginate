# angular-paginate
Dead simple pagination for AngularJS on static data. Thanks to https://github.com/svileng/ng-simplePagination

# simplePagination

This is an Angular 1.x Module created by [svileng](https://github.com/svileng/) which i liked alot, to a point that i have made adaptations to fit my needs.

## Quick start

```bash
just include this script on you page https://raw.githubusercontent.com/KeithNdhlovu/angular-paginate/master/angular-paginate.js
```
or alternatively download and include `simplePagination.js` after `angular.min.js`.

Add the `simplePagination` module as a dependency when creating your app, e.g.

```js
var app = angular.module('myApp', ['simplePagination']);`
```

Inject the `Pagination` service to the controller containing the data which you want to paginate, and set it on the $scope:

```js
app.controller('MyCtrl', ['$scope', 'Pagination',
function($scope, Pagination) {
  $scope.pagination = Pagination.getNew();
}]);
```

This defaults to 5 items per page. You can pass an optional parameter with the number of items you want per page:

```js
$scope.pagination = Pagination.getNew(10);
```

Finally, calculate and set the number of pages depending on your data. Here's an example with a pre-defined `$scope.posts` array for a blog application:

```js
$scope.pagination.numPages = Math.ceil($scope.posts.length/$scope.pagination.perPage);
```

Replace `$scope.posts` with whatever data you have initialised.

## Rendering

There is a custom filter called `startFrom` to help you rendering items per page.

```html
<div ng-repeat="post in posts | startFrom: pagination.page * pagination.perPage | limitTo: pagination.perPage">
	<!-- stuff -->
</div>
```

Again, replace `post in posts` with your data.

### Previous / Next page buttons
```html
<button ng-click="pagination.prevPage()">Previous</button>
<button ng-click="pagination.nextPage()">Next</button>
```
Optionally you can add some logic to hide/disable the buttons using the `pagination.page` and `pagination.numPages` attributes; here's an example:

```html
ng-hide="pagination.page == 0" ng-click="pagination.prevPage()"
ng-hide="pagination.page + 1 >= pagination.numPages" ng-click="pagination.nextPage()"
```

### Page Change

After initalisation, you can hook onto the page change function like so.

This is extremely usefull when you need to get the filtered data from the current page

It is also more important for showing one set of data per page (pagination.perPage = 1), while needing to show details per filtered set.

Heres an example usage.

```js
// show one post at a time
$scope.perPage = 1;
$scope.pagination = Pagination.getNew($scope.perPage);
$scope.pagination.numPages = 0;

...
// AJAX get posts and initialise the number pages
$scope.pagination.numPages = Math.ceil($scope.posts.length / $scope.pagination.perPage);
...

// As page changes, use the current page to get a single post
$scope.pagination.onPageChange(function(page) {
    var filtered = $filter('limitTo')($scope.posts, $scope.pagination.perPage, page * $scope.pagination.perPage));
    
    // Because when know we are always going to show one, then our filtered results will return an array with one item always
    // then we just get the first item on the list
    $scope.currentPost = filtered[0];
    
    // Do something with it ...
});
```

### Page numbers
Using another built-in filter called `range`:
```html
<ul class="pagination">
<li><a href="" ng-click="pagination.prevPage()">&laquo;</a></li>
  <li ng-repeat="n in [] | range: pagination.numPages" ng-class="{active: n == pagination.page}">
  <a href="" ng-click="pagination.toPageId(n)">{{n + 1}}</a>
  </li>
  <li><a href="" ng-click="pagination.nextPage()">&raquo;</a></li>
</ul>
```

If you use, bootstrap.css, Above given list HTML coding give good appearance. Note that the first page is actually __0__ hence the {{n + 1}}.

## Contributions
For problems/suggestions please create an issue on Github.

## Contributors

* [keithndhlovu](https://www.instagram.com/keithndhlovu/)

## Credits
* [@svileng](https://twitter.com/svileng)
* ng-simplePagination: [https://github.com/svileng/ng-simplePagination](https://github.com/svileng/ng-simplePagination)
