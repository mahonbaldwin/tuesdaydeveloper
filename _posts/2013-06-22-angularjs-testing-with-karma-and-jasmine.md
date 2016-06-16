---
layout: post
title: "AngularJS Testing with Karma and Jasmine"
date: 2013-06-22 13:35:48.0
author: mahon
categories: 
---
<a href="/uploads/2013/06/AngularJS-Shield-large.png"><img class="alignleft size-medium wp-image-677" src="/uploads/2013/06/AngularJS-Shield-large-284x300.png" alt="AngularJS-Shield-large" width="284" height="300" /></a>AngularJS is the best thing to happen to JavaScript since jQuery. It's what JavaScript development has always wanted to be. One of the key advantages to Angular is its dependency injection which is very advantageous when you want to unit test your code. There is one little quirk though... I can't for the life of me find a tutorial out there that shows how to do that unit testing.

Sure there are recommendations: use the <a title="Jasmine Overview" href="http://pivotal.github.io/jasmine/">Jasmine</a> test framework with the <a title="Karma" href="http://karma-runner.github.io/0.8/index.html">Karma</a> test runner; but there isn't a start to finish setup guide to make testing work. So I made one. I had to go all around the web finding out how to do this, which (if this is your first stop) you won't have to do.

If you notice any errors please let me know, but as far as I can tell this is the best way to unit test Angular with Karma and Jasmine.
<h3>Introduction</h3>
This tutorial will lead you through installation of all the tools you will need to run automated tests using Karma and Jasmine. I don't care if you're doing TDD or TAD, but for this example, we'll assume that you already have a file you want to test.
<h3>Install Karma</h3>
If you don't have <a title="Node JS" href="http://nodejs.org/">node.js</a> installed, download and install it. After you have it installed go to your terminal or command line and type:
<pre lang="text">npm install -g karma</pre>
<h3>File structure</h3>
The file structure is irrelevant, but for these tests it will look something like this:
<pre lang="text">Application
| angular.js
| angular-resource.js
| Home
  | home.js
| Tests
  | Home
    | home.tests.js
  | karma.config.js (will be created in the next step)
  | angular-mocks.js</pre>
* I'm not advocating this file structure, I simply show it for example sake.
<h3>Configure Karma</h3>
Create a configuration file by navigating to the directory you wish it to be in and typing the following command in your terminal:
<pre lang="text">karma init karma.config.js</pre>
You'll be asked a few questions including which testing framework you want to use, whether you want the files to be auto watched, and what files to include. For our tutorial we'll leave 'jasmine' as the default framework, let it autowatch files, and include the following files:
<pre lang="text">../*.js
../**.*.js
angular-mocks.js
**/*.tests.js</pre>
These are relative paths that include 1) any .js file in the parent directory, any .js file inside of any directory inside of the parent directory, <code>angular-mocks.js</code>, and any file within any directory (located in the current directory) that is formated <code>[name].tests.js</code> (which is how I like to delineate test file from other files).

Whatever files you choose, just be sure that you include angular.js, angular-mocks.js, and any other files that you'll need.
<h3>Start Karma</h3>
Now you are ready to start Karma. Again from the terminal type:
<pre lang="text">karma start karma.config.js</pre>
This will start any browsers you listed in the config file on your computer. Each browser will be connected to the Karma instance with it's own socket and you will see a list of active browsers that will tell you whether or not it is running tests. I wish that Karma would tell you a summary of the last result of your tests for each browser (15 out of 16 passed, 1 failed) but alas for that information you need to look at the terminal window.

An awesome thing about Karma is that you can test on any device connected to your network. Try pointing your phone's browser to Karma by looking at teh URL of one of the browser windows running the tests. It should look something like this: <code>http://localhost:9876/?id=5359192</code>. Point your phone, VM, or any other device with a browser to <code>[your network IP address]:9876/?id=5359192</code>. Because Karma is running an instance of node.js, your test machine is acting like a server and will send the tests to any browser that is pointed to it.
<h3>Make Basic Test</h3>
We are assuming that you already have a file to test. We'll say that your home.js file looks something like this:
<h4>home.js</h4>
<pre lang="javascript" line="1">'use strict';

var app = angular.module('Application', ['ngResource']);

app.factory('UserFactory', function($resource){
    return $resource('Users/users.json')
});

app.controller('MainCtrl', function($scope, UserFactory) {
    $scope.text = 'Hello World!';
    $scope.users = UserFactory.get();
});</pre>
Inside of home.tests.js we can create our tests cases. We'll start out with the simpler of the two: <code>$scope.text</code> should equal 'Hello World!'. To test this we must mockout our <code>Application</code> module and the <code>$scope </code> variable. We'll do this in the Jasmine beforeEach function so that we'll have a fresh controller and scope at the beginning of each test.
<h4>home.tests.js</h4>
<pre lang="javascript" line="1">'use strict';

describe('MainCtrl', function(){
    var scope;//we'll use this scope in our tests

    //mock Application to allow us to inject our own dependencies
    beforeEach(angular.mock.module('Application'));
    //mock the controller for the same reason and include $rootScope and $controller
    beforeEach(angular.mock.inject(function($rootScope, $controller){
        //create an empty scope
        scope = $rootScope.$new();
        //declare the controller and inject our empty scope
        $controller('MainCtrl', {$scope: scope});
    });
    // tests start here
});</pre>
You'll see in the code example that we are injecting our own scope so that we can verify information off of it. <strong>Also, do not forget to mock out the module itself as on line 7!</strong> We are now ready to do our tests:
<h4>home.tests.js</h4>
<pre lang="javascript" line="15">    // tests start here
    it('should have variable text = "Hello World!"', function(){
        expect(scope.text).toBe('Hello World!');
    });</pre>
If you run this test it should run in any browsers looking at Karma and pass.
<h3>Make $resource Request</h3>
Now we're ready to test the <code>$resource</code> request. To make this request we need to use <code>$httpBackend </code>with is a mocked out version of Angular's <code>$http</code>. We'll create another variable called <code>$httpBackend</code> and in our second <code>beforeEach</code> block we'll inject <code>_$httpBackend_</code> and assign the new variable to <code>_$httpBackend_</code>. We'll then tell <code>$httpBackend</code> how to respond to requests.
<pre lang="javascript" line="10">        $httpBackend = _$httpBackend_;
        $httpBackend.when('GET', 'Users/users.json').respond([{id: 1, name: 'Bob'}, {id:2, name: 'Jane'}]);</pre>
And our tests:
<h4>home.tests.js</h4>
<pre lang="javascript" line="20">    it('should fetch list of users', function(){
            $httpBackend.flush();
            expect(scope.users.length).toBe(2);
            expect(scope.users[0].name).toBe('Bob');
        });</pre>
<h3>All Together</h3>
<h4>home.tests.js</h4>
<pre lang="javascript" line="1">'use strict';

describe('MainCtrl', function(){
    var scope, $httpBackend;//we'll use these in our tests

    //mock Application to allow us to inject our own dependencies
    beforeEach(angular.mock.module('Application'));
    //mock the controller for the same reason and include $rootScope and $controller
    beforeEach(angular.mock.inject(function($rootScope, $controller, _$httpBackend_){
        $httpBackend = _$httpBackend_;
        $httpBackend.when('GET', 'Users/users.json').respond([{id: 1, name: 'Bob'}, {id:2, name: 'Jane'}]);

        //create an empty scope
        scope = $rootScope.$new();
        //declare the controller and inject our empty scope
        $controller('MainCtrl', {$scope: scope});
    });
    // tests start here
    it('should have variable text = "Hello World!"', function(){
        expect(scope.text).toBe('Hello World!');
    });
    it('should fetch list of users', function(){
        $httpBackend.flush();
        expect(scope.users.length).toBe(2);
        expect(scope.users[0].name).toBe('Bob');
    });
});</pre>
<h3>Tips</h3>
<ul>
	<li>Karma will run all tests in all files, if you only want to run a subset of tests change <code>describe</code> or <code>it</code> to <code>ddescribe</code> or <code>iit</code> to run the respective tests. If there are tests that you do not want to test change <code>describe</code> or <code>it</code> to <code>xdescribe</code> or <code>xit</code> to ignore that set of code.</li>
	<li>I would also suggest reading through the <a title="Jasmine Documentation" href="http://pivotal.github.io/jasmine/">Jasmine documentation</a> to know what methods are available to you.</li>
	<li>You also have the option to run your tests in an html file on the page. The code for our example would look something like this:
<h4>home.runner.html</h4>
<pre lang="html" line="1"><!-- include your script files (notice that the jasmine source files have been added to the project) --><script src="../jasmine/jasmine-1.3.1/jasmine.js" type="text/javascript"></script><script src="../jasmine/jasmine-1.3.1/jasmine-html.js" type="text/javascript"></script><script src="../angular-mocks.js" type="text/javascript"></script><script src="home.tests.js" type="text/javascript"></script><!-- use Jasmine to run and display test results --><script type="text/javascript">// <![CDATA[
        var jasmineEnv = jasmine.getEnv();
        jasmineEnv.addReporter(new jasmine.HtmlReporter());
        jasmineEnv.execute();
    
// ]]></script></pre>
</li>
</ul>

<div class='archived comments'>

<div class='comment'>Nice job and thanks.
I think there should be an additional ')' in the first home.tests.js line 14, making it <b>}));</b> rather than <b>});</b>  <div class='by'>Grant on 2013-08-04 13:05:16.0  </div></div>
<div class='comment'>great job!  <div class='by'>peterr on 2013-08-20 03:16:16.0  </div></div>
<div class='comment'>Thanks! I think that should be "npm -g install karma", not "npm install -g karma".  <div class='by'>Alec McEachran on 2013-09-04 22:54:49.0  </div></div>
<div class='comment'>It appears that angular-mocks.js only defines angular.mock.module when Jasmine is being used. I don't understand why it would exclude the use of Mocha.  <div class='by'>Mark Volkmann on 2013-09-08 15:58:09.0  </div></div>
<div class='comment'>So does karma-jasmine ship with jasmine itself, or do I have to install that separately using `npm`??  <div class='by'>Rudolf Olah on 2013-09-10 16:50:12.0  </div></div>
<div class='comment'>Do we actually need karma if we are performing tests using browser with all jasmine files included?

I kind of found the fact that karma opens browser completely redundant - as it doesn't really do anything apart from using it's console - all the relevant information is displayed in the terminal anyway.  <div class='by'>Mark on 2013-10-12 08:26:23.0  </div></div>
<div class='comment'>Also - you haven't mentioned what the users.json file should contain - I'm constantly getting the following test failure:

TypeError: Object # has no method 'push'  <div class='by'>Mark on 2013-10-12 08:45:56.0  </div></div>
<div class='comment'>@Mark 
You can use a headless browser such as PhantomJS with karma to avoid a browser window opening.  <div class='by'>mr on 2013-10-14 22:57:51.0  </div></div>
<div class='comment'>Check out our Jasmine testing videos at http://blog.neosavvy.com.  <div class='by'>Trevor Ewen on 2013-11-04 14:44:32.0  </div></div>
<div class='comment'>How could we do AngularJS Testing with Karma for https sites?  <div class='by'> on 2013-11-06 09:12:39.0  </div></div>
<div class='comment'>I tried running a simple test. But its failing with below error:
C:\Users\502245602\combo&gt;karma start src/main/webapp/karma.config.js
INFO [karma]: Karma v0.10.4 server started at http://localhost:9876/
INFO [launcher]: Starting browser Chrome
INFO [Chrome 30.0.1599 (Windows 7)]: Connected on socket auZijvn9LzbH_2K0QDcv
Chrome 30.0.1599 (Windows 7) Sanity Test Sanity test Jasmine" FAILED
        expect undefined toEqual "Hi"
      ..../src/main/webapp/test/spec/controllers/welcome.test.js:15:3: expected "Hi" but was undefined
Chrome 30.0.1599 (Windows 7): Executed 1 of 1 (1 FAILED) ERROR (0.38 secs / 0.024 secs)


my test looks like below:


describe('Sanity Test', function() {
	var scope;
	beforeEach(angular.mock.module('serviceApp'));
	beforeEach(angular.mock.inject(function($rootScope, $controller) {
		scope = $rootScope.$new();
		$controller('welcomeController', {
			$scope : scope
		});
	}));


	it('Sanity test Jasmine"', function() {
		scope.text = 'Hi';		
		expect('Hi').toEqual('Hi');
	});
});

any help...  <div class='by'>Priyabrat on 2013-11-13 20:35:45.0  </div></div>
<div class='comment'><code>var complement = "Great Job!";</code>  <div class='by'> on 2013-11-19 15:32:46.0  </div></div>
<div class='comment'>@Priyabrat your expect should be:

expect(scope.text).toEqual('Hi');  <div class='by'>Brandon R Staley on 2013-12-04 10:02:49.0  </div></div>
<div class='comment'>Thanks you for this tutorial. It helps me a lot!!  <div class='by'>Tobi on 2013-12-17 17:31:53.0  </div></div>
<div class='comment'>Could not get html runner working:

Uncaught TypeError: Cannot set property 'mock' of undefined angular-mocks.js:17
Uncaught TypeError: undefined is not a function home.runner.html:16  <div class='by'>Dmitri on 2013-12-28 02:04:22.0  </div></div>
<div class='comment'>Also the test did not pass:

Chrome 31.0.1650 (Mac OS X 10.8.5) MainCtrlTest should have variable text = "Hello World!" FAILED
	ReferenceError: $httpBackend is not defined
	    at null. (/Users/dmitrizaitsev/Dropbox/Priv/APP/Testers/generated-app-test/test/spec/home.tests.js:21:26)
	    at Object.invoke (/Users/dmitrizaitsev/Dropbox/Priv/APP/Testers/generated-app-test/app/bower_components/angular/angular.js:3697:17)
	    at workFn (/Users/dmitrizaitsev/Dropbox/Priv/APP/Testers/generated-app-test/app/bower_components/angular-mocks/angular-mocks.js:2102:20)
	Error: Declaration Location
	    at Object.window.inject.angular.mock.inject [as inject] (/Users/dmitrizaitsev/Dropbox/Priv/APP/Testers/generated-app-test/app/bower_components/angular-mocks/angular-mocks.js:2087:25)
	    at null. (/Users/dmitrizaitsev/Dropbox/Priv/APP/Testers/generated-app-test/test/spec/home.tests.js:19:29)
	    at /Users/dmitrizaitsev/Dropbox/Priv/APP/Testers/generated-app-test/test/spec/home.tests.js:3:1
	TypeError: Cannot read property 'text' of undefined
	    at null. (/Users/dmitrizaitsev/Dropbox/Priv/APP/Testers/generated-app-test/test/spec/home.tests.js:32:21)
Chrome 31.0.1650 (Mac OS X 10.8.5): Executed 2 of 2 (1 FAILED) (0.031 secs / 0.028 secs)

However, dropping the _ miraculously  works:

        function($rootScope, $controller, $httpBackend){
            $httpBackend.when('GET', 'Users/users.json').respond([{id: 1, name: 'Bob'}, {id:2, name: 'Jane'}]);
...

Chrome 31.0.1650 (Mac OS X 10.8.5): Executed 2 of 2 SUCCESS (0.039 secs / 0.035 secs)  <div class='by'>Dmitri on 2013-12-28 02:10:28.0  </div></div>
<div class='comment'>How to test multiple controller, whether i have to declare 'var scope' for each controller ?  <div class='by'>Imran Khatri on 2013-12-31 23:48:43.0  </div></div>
<div class='comment'>How to test multiple controller, whether i have to declare 'var scope' for each controller?  <div class='by'>Imran Khatri on 2013-12-31 23:49:24.0  </div></div>
<div class='comment'>Really nice!

I spend days to make my tests work with karma/jasmine, and this is the best article that i found  <div class='by'>Ariel Moraes on 2014-01-21 12:12:39.0  </div></div>
<div class='comment'>@Dmitri

About the undefined mock,
make sure your config/karma.conf.js 
is pointing at the right file location.

Example:

files : [
      'public/js/lib/angular/angular.js',
      'public/js/lib/angular/angular-*.js',
      'test/lib/angular/angular-mocks.js',
      'public/js/**/*.js',
      'test/unit/**/*.js'
    ]  <div class='by'>Dude on 2014-02-15 13:37:50.0  </div></div>
<div class='comment'>good one!!  <div class='by'> on 2014-02-19 04:49:56.0  </div></div>
<div class='comment'>Nice tutorial. Thanks!  <div class='by'>Franco on 2014-02-19 10:41:14.0  </div></div>
<div class='comment'>I was unable to get this example to work with a fresh install, even after fixing the above errors.
I'm using the latest versions of all tools, and Angular is v1.2.15
The first error was:
        Error: [$injector:modulerr] Failed to instantiate module ngResource due to:
        Error: [$injector:nomod] Module 'ngResource' is not available! You either misspelled the module name or forgot to load it. If registering a module ensure that you specify the dependencies as the second argument.

So I simplified the example further to:
appl.js
=====
<code>
'use strict';
var app = angular.module('Application');
app.controller('MainCtrl', function($scope) {
    $scope.text = 'Hello World!';
});
</code>
--------
appl.test.js
==========
<code>
'use strict';
describe('MainCtrl', function() {
    var scope;//we'll use this scope in our tests
    beforeEach(angular.mock.module('Application'));
    //mock the controller for the same reason and include $rootScope and $controller
    beforeEach(angular.mock.inject(function($rootScope, $controller){
        scope = $rootScope.$new();
        $controller('MainCtrl', {$scope: scope});
    }));
    it('should have variable text = "Hello World!"', function() {
       expect(scope.text).toBe('Hello World!');
    });
});
</code>
-------------
but this fails with:
  Uncaught Error: [$injector:nomod] Module 'Application' is not available! You either misspelled the module name or forgot to load it. If registering a module ensure that you specify the dependencies as the second argument.

Any help is greatly appreciated...  <div class='by'>P Snider on 2014-03-25 19:04:18.0  </div></div>
<div class='comment'>Hey P Snider, I had the same problem-- it looks like the home.js file isn't being included in the Karma config file (that's why it couldn't find the Application Module), so in karma.config.js, I added '../Home/*.js' to the files array.  <div class='by'>Will on 2014-04-03 19:30:32.0  </div></div>
<div class='comment'>Great tutorial.

Couple of addition to it will make a real life testing suite:

1: PhantomJS integration for headless ( no opening browsers anymore) testing.
2: Coverage, for testing code coverage in the application.
3: Switching auto-run to false and doing on demand tests. (depends across type of projects and developers)

All in all a great place to start karma-jasmine-angular unit testing !!!!  <div class='by'>Abhi on 2014-04-21 13:34:30.0  </div></div>
<div class='comment'>Note that it is now recommended to globally install <strong>karma-cli</strong> (<code>npm install -g karma-cli</code>) which will take care of fetching the appropriate <strong>karma</strong>.

Thus you can install a different local version specific to each project and <strong>karma-cli</strong> will pick the appropriate one.  <div class='by'>Aymeric Beaumet on 2014-04-24 04:12:15.0  </div></div>
<div class='comment'>To run the command "karma init karma.config.js" I had to install the package karma-cli (npm install -g karma-cli)  <div class='by'>Gianni Bossini on 2014-05-09 03:55:26.0  </div></div>
<div class='comment'>Good tutorial here as well for those interested:

https://docs.angularjs.org/tutorial  <div class='by'>Treize on 2014-05-23 10:55:57.0  </div></div>
<div class='comment'>aaa  <div class='by'>Asawari Ranjit Patil on 2014-06-20 06:38:56.0  </div></div>
<div class='comment'>Great tutorial. Exactly what I needed!  <div class='by'>KJ Price on 2014-06-25 11:48:16.0  </div></div>
<div class='comment'>Great article. Thanks!

But I think that the whole point of factories is to isolate the dependancies, so your controller doesn't care uf the users are created by http request or its a simply hardcoded array. I think that the most useful way to test this funcionality is to stub the userfactory with an array or a mock promise that resolves returning the users array. What do you  think?  <div class='by'>Vinícius Oyama on 2014-06-28 12:48:16.0  </div></div>
<div class='comment'>Nice  <div class='by'>Karthee on 2014-07-13 03:53:56.0  </div></div>
<div class='comment'>Hi, I've tried exactly the same, but I'm always getting these following Errors:

<code>minErr/&lt;@/home/michael/webui-ng/src/client/app/bower_components/angular/angular.js:78:5
	loadModules/&lt;@/home/michael/webui-ng/src/client/app/bower_components/angular/angular.js:3859:1
	forEach@/home/michael/webui-ng/src/client/app/bower_components/angular/angular.js:325:7
	loadModules@/home/michael/webui-ng/src/client/app/bower_components/angular/angular.js:3824:5
	createInjector@/home/michael/webui-ng/src/client/app/bower_components/angular/angular.js:3764:3
	workFn@/home/michael/webui-ng/src/client/app/bower_components/angular-mocks/angular-mocks.js:2150:9
	
	TypeError: scope is undefined in /home/michael/webui-ng/src/client/test/hello.js (line 17)
	@/home/michael/webui-ng/src/client/test/hello.js:17:9</code>

My idea is, that the problem has something to do with the requirements array in angular.module('Application', ['ngRessource']), because when letting this empty the test passes. I'm pretty stuck at this problem at the moment, do you have an idea what it could be?  <div class='by'>Michael on 2014-07-17 09:41:06.0  </div></div>
<div class='comment'>For people who just want to simply test on a web page, such as in the home.runner.html example above, and using Jasmine 2.X: you'll need the <i>boot.js</i> file declared after <i>jasmine.js</i> and <i>jasmine-html.js</i>.

If not, you'll get nothing but a couple of reference errors in the browser console.  <div class='by'>User20140804 on 2014-08-04 09:36:19.0  </div></div>
<div class='comment'>Line 17 in the second home.tests.js is missing a closing single-quote, which generates syntax errors if the code is copy/pasted directly.

The line is:
expect(scope.text).toBe('Hello World!);

It should be:
expect(scope.text).toBe('Hello World!');

:-)  <div class='by'>Paul B. Hartzog on 2014-10-16 23:42:19.0  </div></div>
<div class='comment'>1. If my unit test is calling function which is in controller and that function is calling a service to fetch the details. Will it call service(StationService)? 
2. My Karma unit test is not able to inject StationService and not able to call service. 

My Code.
/// controller
var policyControllers = angular.module('policyControllers', []);
policyControllers.controller('StationListController', ['$translate', '$scope','$rootScope','$state', 'StationService', 'StationListExportService', function ($translate, $scope, $rootScope, $state, StationService, StationListExportService) {
...
$scope.getFilterDetails = function(StationService, filterDetails ){

	StationService.get(filterDetails).$promise.then(function (filteredDetails) {
			console.log(" Web services Result - ", JSON.stringify(filteredDetails));			
		},function(error) {
			console.log(" Error ");
		});
	};
	




	
///Service	
var policyServices = angular.module('policyServices', ['ngResource']);
policyServices.factory('StationService', ['$resource', function($resource) {
	return $resource(policyConfig.mock? 'modules/policymanager/services/mock/stations.json': 'http://10.132.240.25:7640/policy/api/v1/stationpolicy/stations',{},{
		get:{method: 'POST',isArray: false, url:'modules/policymanager/services/mock/stations.json'}
	});
}]);



/// Unit test
describe('station filter', function(){

    var scope;
    var ctrl;
    var translate, scope, rootScope, state;
    var StationService, StationListExportService;

    beforeEach(module('policyServices'));
    beforeEach(module('policyControllers'));



    beforeEach(inject(function(_StationService_, _StationListExportService_, $rootScope, $controller, $translate, $state) {

        StationService = _StationService_;
        StationListExportService = _StationListExportService_;
        translate = $translate;
        rootScope = $rootScope;
        state = $state;
        scope = $rootScope.$new();
        ctrl = $controller('StationListController', {$scope: scope});

    }));




    it('Stations Inject test case', inject(['StationService',function(StationService){

        var data = {"recency":"","countries":[],"policies":[],"stations":[{"stationName":"Test"}],"status":"ready","regions":[]};
        
        scope.getFilterDetails(StationService, data);
        
      ///  Getting StationService is undifiened 
    
    }]));  <div class='by'>Amit on 2015-02-03 08:09:50.0  </div></div>
<div class='comment'>For anyone experiencing an issue where $httpBackend.flush() returns an error that it's looking for an object but an array was found, try changing this line:

`$httpBackend.when('GET', 'Users/users.json').respond([{ id: 1, name: 'Bob' }, { id: 2, name: 'Jane' }]);` 

to this:

`$httpBackend.when('GET', 'Users/users.json').respond(200, { data: [{ id: 1, name: 'Bob' }, { id: 2, name: 'Jane' }] });`

and subsequently change the following lines to test the new `data` object:

`$httpBackend.flush();`
`expect(scope.users.data.length).toBe(2);`
`expect(scope.users.data[0].name).toBe('Bob');`

Otherwise, great post!  <div class='by'>Aaron Jessen on 2015-04-03 10:20:08.0  </div></div>
<div class='comment'>P.S. Remove the ` from the lines from the code in my previous comment (I thought the comment system might be like StackOverflow where ` creates code blocks).  <div class='by'>Aaron Jessen on 2015-04-03 10:22:24.0  </div></div>
<div class='comment'>To make this code sample work , I had to :
1 - add the missing parenthesis in "All Together, home.tests.js", line 17 : it should be "}));" instead of "});" (as noted by Grant above)
2 - change "Userfactory.get()" by "UserFactory.query()" in home.js, otherwise I got a "Error in resource configuration. Expected response to contain an object but got an array"

Hope it'll help others  <div class='by'>Michael Zilbermann on 2015-05-12 14:23:51.0  </div></div>
<div class='comment'>You've help me. Thank you, Aaron Jessen!  <div class='by'> on 2015-07-02 00:19:19.0  </div></div>
</div>