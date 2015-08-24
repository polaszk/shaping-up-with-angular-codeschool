// shaping up with angular.js 

// 1.2 Creating a Store Module
// app.js
var app = angular.module('gemStore', []);

// index.html
<!DOCTYPE html>
<html ng-app="gemStore">
  <head>
    <link rel="stylesheet" type="text/css" href="bootstrap.min.css" />
    <script type="text/javascript" src="angular.min.js"></script>
    <script type="text/javascript" src="app.js"></script>
  </head>
  <body>
    <h1>{{ 'Hello, Angular!' }}</h1>
  </body>
</html>

// 1.4 Our First Controller
// app.js
(function(){
  var gem = { name: 'Azurite', price: 2.95 };
  var app = angular.module('gemStore', []);
  app.controller('StoreController', function(){
    this.product = gem;
  });
})();

// index.html
<body ng-controller="StoreController as store">
  <div class="product row">
    <h3>
      {{ store.product.name }}
      <em class="pull-right">{{ store.product.price }}</em>
    </h3>
  </div>
</body>

// 1.6 Not For Sale
// app.js
(function() {
  var app = angular.module('gemStore', []);

  app.controller('StoreController', function(){
    this.product = gem;
  });

  var gem = {
    name: 'Azurite',
    price: 110.50,
    canPurchase: false,
    soldOut: true
  };
})();

// index.thml
<body class="container" ng-controller="StoreController as store">
  <div ng-hide="store.product.soldOut" class="product row">
    <h3>
      {{store.product.name}}
      <em class="pull-right">${{store.product.price}}</em>
    </h3>
    <button ng-show="store.product.canPurchase">Add to Cart</button>
  </div>
</body>

// 1.7 Look, More Gems!
// app.js
(function() {
  var app = angular.module('gemStore', []);

  app.controller('StoreController', function(){
    this.products = gems;
  });

  var gems = [
    { name: 'Azurite', price: 2.95 },
    { name: 'Bloodstone', price: 5.95 },
    { name: 'Zircon', price: 3.95 }
  ];
})();

// index.thml
<body class="container" ng-controller="StoreController as store">
   <div class="product row" ng-repeat="product in store.products">
     <h3>
       {{product.name}}
       <em class="pull-right">${{product.price}}</em>
     </h3>
   </div>
 </body>

// 2.2 Using filters
// index.thml
<body class="container" ng-controller="StoreController as store">
  <div class="product row" ng-repeat="product in store.products">
    <h3>
      {{product.name}}
      <em class="pull-right">{{product.price | currency }}</em>
    </h3>
  </div>
</body>

// 2.3 Displaying the First Image
// index.thml
<body ng-controller="StoreController as store">
  <div class="list-group">
    <div class="list-group-item" ng-repeat="product in store.products">
      <h3>
        {{product.name}}
        <em class="pull-right">{{product.price | currency}}</em>
      </h3>
      <div class="gallery">
        <img ng-src="{{ product.images[0] }}"/>
      </div>
    </div>
  </div>
</body>

// 2.4 Display All Thumbnails
// index.html
<body class="list-group" ng-controller="StoreController as store">
  <div class="list-group-item" ng-repeat="product in store.products">
    <h3>{{product.name}} <em class="pull-right">{{product.price | currency}}</em></h3>
    <div class="gallery">
      <div class="img-wrap">
        <img ng-src="{{product.images[0]}}" />
      </div>
      <ul class="img-thumbnails clearfix">
        <li class="small-image pull-left thumbnail" ng-repeat="image in product.images" >
          <img ng-src="{{image}}" />
        </li>
      </ul>
    </div>
  </div>
</body>

// 2.5 No Images, No Gallery
// index.html
<div class="gallery" ng-show="product.images.length">
  <img class="img img-circle img-thumbnail center-block" ng-src="{{product.images[0]}}" />
  <ul class="clearfix">
    <li class="small-image pull-left thumbnail" ng-repeat="image in product.images"> <img ng-src="{{image}}" /> </li>
  </ul>
</div>

// 2.7 Tabs Inside Out
// app.js
app.controller('TabController', function(){
  this.tab = 1;

  this.setTab = function(newValue){
    this.tab = newValue;
  };

  this.isSet = function(tabName){
    return this.tab === tabName;
  };
});

// 2.8 Using TabController
// index.html
<section class="tab" ng-controller="TabController as tab">
  <ul class="nav nav-pills">
    <li ng-class="{ active:tab.isSet(1) }">
      <a href ng-click="tab.setTab(1)">Description</a></li>
    <li ng-class="{ active:tab.isSet(2) }">
      <a href ng-click="tab.setTab(2)">Specs</a></li>
    <li ng-class="{ active:tab.isSet(3) }">
      <a href ng-click="tab.setTab(3)">Reviews</a></li>
  </ul>
  <div ng-show="tab.isSet(1)">
    <h4>Description</h4>
    <blockquote>{{ product.description }}</blockquote>
  </div>
  <div ng-show="tab.isSet(2)">
    <h4>Specs</h4>
    <blockquote>Shine: {{ product.shine }}</blockquote>
  </div>
  <div ng-show="tab.isSet(3)">
    <h4>Reviews</h4>
    <blockquote></blockquote>
  </div>
</section>


// 2.9 Creating Gallery Controller
// app.js
app.controller('GalleryController', function(){
  this.current = 0;
  this.setCurrent = function(newGallery){
    this.current = newGallery || 0;
  };
});

// 2.10 Using Gallery Controller
// index.html
<div class='gallery' ng-show="product.images.length" ng-controller="GalleryController as gallery">
  <img ng-src="{{product.images[gallery.current]}}" />
  <ul class="list-inline thumbs">
    <li class="thumbnail" ng-repeat="image in product.images">
      <img ng-src="{{image}}" />
    </li>
  </ul>
</div>

// 3.2 Displaying Reviews should seem repetitive
// index.html
<h4>Reviews</h4>
<li ng-repeat="review in product.reviews">
  <blockquote>
    <strong>{{ review.stars }} Stars</strong>
    {{ review.body }}
    <cite class="clearfix">— {{ review.author }}</cite>
  </blockquote>
</li>

// 3.3 Create a Review Form
// index.html
<h4>Submit a Review</h4>
<fieldset class="form-group">
  <select class="form-control" ng-model="review.stars" ng-options="stars for stars in [5,4,3,2,1]"  title="Stars">
    <option value="">Rate the Product</option>
  </select>
</fieldset>
<fieldset class="form-group">
  <textarea ng-model="review.body" class="form-control" placeholder="Write a short review of the product..." title="Review"></textarea>
</fieldset>
<fieldset class="form-group">
  <input ng-model="review.author" type="email" class="form-control" placeholder="jimmyDean@example.org" title="Email" />
</fieldset>
<fieldset class="form-group">
  <input type="submit" class="btn btn-primary pull-right" value="Submit Review" />
</fieldset>

// 3.4 Review Live Preview!
// index.html
<blockquote>
  <strong>{{review.stars}} Stars</strong>
  {{ review.body }}
  <cite class="clearfix">-{{ review.author }}</cite>
</blockquote>

// 3.6 Creating Review Controller
// app.js
app.controller('ReviewController', function() {
  this.review = {};

  this.addReview = function(product) {
    product.reviews.push(this.review);

    this.review = {};
  };
});

// 3.7 Using Review Controller
// index.html
<form name="reviewForm" ng-controller="ReviewController as reviewCtrl" ng-submit="reviewCtrl.addReview(product)">

  <!--  Live Preview -->
  <blockquote>
    <strong>{{reviewCtrl.review.stars}} Stars</strong>
    {{reviewCtrl.review.body}}
    <cite class="clearfix">—{{reviewCtrl.review.author}}</cite>
  </blockquote>

  <!--  Review Form -->
  <h4>Submit a Review</h4>
  <fieldset class="form-group">
    <select ng-model="reviewCtrl.review.stars" class="form-control" ng-options="stars for stars in [5,4,3,2,1]" title="Stars">
      <option value="">Rate the Product</option>
    </select>
  </fieldset>
  <fieldset class="form-group">
    <textarea ng-model="reviewCtrl.review.body" class="form-control" placeholder="Write a short review of the product..." title="Review"></textarea>
  </fieldset>
  <fieldset class="form-group">
    <input ng-model="reviewCtrl.review.author" type="email" class="form-control" placeholder="jimmyDean@example.org" title="Email" />
  </fieldset>
  <fieldset class="form-group">
    <input type="submit" class="btn btn-primary pull-right" value="Submit Review" />
  </fieldset>
</form>

// 3.9 Form Validation
// index.html
<!--  Review Form -->
<form name="reviewForm" ng-controller="ReviewController as reviewCtrl" ng-submit="reviewForm.$valid && reviewCtrl.addReview(product)" novalidate>

  <!--  Live Preview -->
  <blockquote >
    <strong>{{reviewCtrl.review.stars}} Stars</strong>
    {{reviewCtrl.review.body}}
    <cite class="clearfix">—{{reviewCtrl.review.author}}</cite>
  </blockquote>

  <!--  Review Form -->
  <h4>Submit a Review</h4>
  <fieldset class="form-group">
    <select ng-model="reviewCtrl.review.stars" class="form-control" ng-options="stars for stars in [5,4,3,2,1]" title="Stars" required>
      <option value="">Rate the Product</option>
    </select>
  </fieldset>
  <fieldset class="form-group">
    <textarea ng-model="reviewCtrl.review.body" class="form-control" placeholder="Write a short review of the product..." title="Review"></textarea>
  </fieldset>
  <fieldset class="form-group">
    <input ng-model="reviewCtrl.review.author" type="email" class="form-control" placeholder="jimmyDean@example.org" title="Email" required/>
  </fieldset>
  <fieldset class="form-group">
    <input type="submit" class="btn btn-primary pull-right" value="Submit Review" />
  </fieldset>
</form>

// 3.10 Form Styling
// application.css
.ng-invalid.ng-dirty {
  border-color: red;
}

.ng-valid.ng-dirty {
  border-color: green;
}

// 3.11 Showing CreatedOn Date
// app.js
app.controller("ReviewController", function(){

  this.review = {};

  this.review.createdOn = Date.now();

  this.addReview = function(product){
    product.reviews.push(this.review);
    this.review = {};
  };
});

//index.html
<cite class="clearfix">—{{review.author}} on {{review.createdOn | date}}</cite>

// 4.2 Refactoring Description Tab
// index.html
<div ng-show="tab.isSet(1)" ng-include="product-description.html">
</div>

// product-description.html
<div>
  <h4>Description</h4>
  <blockquote>{{product.description}}</blockquote>
</div>

// 4.3 Creating an Element Directive
// app.js
app.directive('productDescription', function(){
  return {
    restrict: 'E',
    templateUrl: 'product-description.html'
  };
});

// index.html
<product-description ng-show="tab.isSet(1)"></product-description>

// 4.4 Creating an Attribute Directive
//index.html
<div ng-show="tab.isSet(2)" product-specs>
</div>

// product-speces.html
<h4>Specs</h4>
<ul class="list-unstyled">
  <li>
    <strong>Shine</strong>
    : {{product.shine}}</li>
  <li>
    <strong>Faces</strong>
    : {{product.faces}}</li>
  <li>
    <strong>Rarity</strong>
    : {{product.rarity}}</li>
  <li>
    <strong>Color</strong>
    : {{product.color}}</li>
</ul>

// app.js
app.directive("productSpecs", function(){
  return {
    restrict: 'A',
    templateUrl: 'product-specs.html'
  };
});

// 4.6 Refactoring Product Tabs
// index.html
<body ng-controller="StoreController as store">
  <!--  Store Header  -->
  <header>
    <h1 class="text-center">Flatlander Crafted Gems</h1>
    <h2 class="text-center">– an Angular store –</h2>
  </header>

  <!--  Products Container  -->
  <div class="list-group">
    <!--  Product Container  -->
    <div class="list-group-item" ng-repeat="product in store.products">
      <h3>{{product.name}} <em class="pull-right">{{product.price | currency}}</em></h3>

      <!-- Image Gallery  -->
      <div ng-controller="GalleryController as gallery"  ng-show="product.images.length">
        <div class="img-wrap">
          <img ng-src="{{product.images[gallery.current]}}" />
        </div>
        <ul class="img-thumbnails clearfix">
          <li class="small-image pull-left thumbnail" ng-repeat="image in product.images">
            <img ng-src="{{image}}" />
          </li>
        </ul>
      </div>

      <!-- Product Tabs  -->
      <product-tabs></product-tabs>
    </div>

  </div>
</body>

// app.js
app.directive("productTabs", function() {
  return {
    restrict: 'E',
    templateUrl: 'product-tabs.html',
    controller: function() {
      this.tab = 1;

      this.isSet = function(checkTab) {
        return this.tab === checkTab;
      };

      this.setTab = function(setTab) {
        this.tab = setTab;
      };
    },
    controllerAs: 'tab'
  };
});

// product-tabs.html
<section>
  <ul class="nav nav-pills">
    <li ng-class="{ active:tab.isSet(1) }">
      <a href ng-click="tab.setTab(1)">Description</a>
    </li>
    <li ng-class="{ active:tab.isSet(2) }">
      <a href ng-click="tab.setTab(2)">Specs</a>
    </li>
    <li ng-class="{ active:tab.isSet(3) }">
      <a href ng-click="tab.setTab(3)">Reviews</a>
    </li>
  </ul>

  <div ng-show="tab.isSet(1)" ng-include="'product-description.html'">
  </div>

  <div product-specs ng-show="tab.isSet(2)"></div>

  <product-reviews ng-show="tab.isSet(3)"></product-reviews>

</section>

// 4.7 Refactoring Product Gallery
// app.js
app.directive('productGallery', function() {
  return {
    restrict: 'E',
    templateUrl: 'product-gallery.html',
    controller: function() {
      this.current = 0;
      this.setCurrent = function(imageNumber){
        this.current = imageNumber || 0;
      };
    },
    controllerAs: 'gallery'
  };
});

// index.html
<body ng-controller="StoreController as store">
  <header>
    <h1 class="text-center">Flatlander Crafted Gems</h1>
    <h2 class="text-center">– an Angular store –</h2>
  </header>

  <div class="list-group">
    <div class="list-group-item" ng-repeat="product in store.products">
      <h3>{{product.name}} <em class="pull-right">{{product.price | currency}}</em></h3>

      <product-gallery></product-gallery>

      <product-tabs></product-tabs>

    </div>
  </div>
</body>

// product-gallery.html
<div ng-show="product.images.length">
  <div class="img-wrap">
    <img ng-src="{{product.images[gallery.current]}}" />
  </div>
  <ul class="img-thumbnails clearfix">
    <li class="small-image pull-left thumbnail" ng-repeat="image in product.images">
      <img ng-src="{{image}}" />
    </li>
  </ul>
</div>

// 5.2 Refactoring into a Module
// products.js
(function() {
  var app = new angular.module('store-directives', []);
  storeDirectives.directive("productDescription", function() {
    return {
      restrict: 'E',
      templateUrl: "product-description.html"
    };
  });

  storeDirectives.directive("productReviews", function() {
    return {
      restrict: 'E',
      templateUrl: "product-reviews.html"
    };
  });

  storeDirectives.directive("productSpecs", function() {
    return {
      restrict:"A",
      templateUrl: "product-specs.html"
    };
  });

  storeDirectives.directive("productTabs", function() {
    return {
      restrict: "E",
      templateUrl: "product-tabs.html",
      controller: function() {
        this.tab = 1;

        this.isSet = function(checkTab) {
          return this.tab === checkTab;
        };

        this.setTab = function(activeTab) {
          this.tab = activeTab;
        };
      },
      controllerAs: "tab"
    };
  });

  storeDirectives.directive("productGallery", function() {
    return {
      restrict: "E",
      templateUrl: "product-gallery.html",
      controller: function() {
        this.current = 0;
        this.setCurrent = function(imageNumber){
          this.current = imageNumber || 0;
        };
      },
      controllerAs: "gallery"
    };
  });
})();

// index.html
<head>
  <link rel="stylesheet" type="text/css" href="bootstrap.min.css" />
  <script type="text/javascript" src="angular.min.js"></script>
  <script type="text/javascript" src="app.js"></script>
  <script type="text/javascript" src="products.js"></script>
</head>

// app.js
var app = angular.module('gemStore', ['store-directives']);

// 5.4 Built-in Angular Services
// app.js
app.controller('StoreController',[ '$http', function($http) {
  var store = this;
  store.products = [];
  $http.get('/store-products.json').success(function(data) {
    store.products = data;
  });
}]);
