#####
Views
#####

A view is simply a web page, or a page fragment, like a header, footer,
sidebar, etc. In fact, views can flexibly be embedded within other views
(within other views, etc., etc.) if you need this type of hierarchy.

Views are never called directly, they must be loaded by a
:doc:`controller <controllers>`. Remember that in an MVC framework, the
Controller acts as the traffic cop, so it is responsible for fetching a
particular view. If you have not read the
:doc:`Controllers <controllers>` page you should do so before
continuing.

Using the example controller you created in the
:doc:`controller <controllers>` page, let's add a view to it.

Creating a View
===============

Using your text editor, create a file called blogview.php, and put this
in it::

	<html>
	<head>
	<title>My Blog</title>
	</head>
	<body>
		<h1>Welcome to my Blog!</h1>
	</body>
	</html>
	
Then save the file in your application/views/ folder.

Loading a View
==============

To load a particular view file you will use the following function::

	$this->load->view('name');

Where name is the name of your view file. Note: The .php file extension
does not need to be specified unless you use something other than .php.

Now, open the controller file you made earlier called blog.php, and
replace the echo statement with the view loading function::

	<?php
	class Blog extends CI_Controller {

		function index()
		{
			$this->load->view('blogview');
		}
	}
	?>

If you visit your site using the URL you did earlier you should see your
new view. The URL was similar to this::

	example.com/index.php/blog/

Loading multiple views
======================

CodeIgniter will intelligently handle multiple calls to
$this->load->view from within a controller. If more than one call
happens they will be appended together. For example, you may wish to
have a header view, a menu view, a content view, and a footer view. That
might look something like this::

	<?php

	class Page extends CI_Controller {

	   function index()
	   {
	      $data['page_title'] = 'Your title';
	      $this->load->view('header');
	      $this->load->view('menu');
	      $this->load->view('content', $data);
	      $this->load->view('footer');
	   }

	}
	?>

In the example above, we are using "dynamically added data", which you
will see below.

Storing Views within Sub-folders
================================

Your view files can also be stored within sub-folders if you prefer that
type of organization. When doing so you will need to include the folder
name loading the view. Example::

	$this->load->view('folder_name/file_name');

Adding Dynamic Data to the View
===============================

Data is passed from the controller to the view by way of an **array** or
an **object** in the second parameter of the view loading function. Here
is an example using an array::

	$data = array(
	               'title' => 'My Title',
	               'heading' => 'My Heading',
	               'message' => 'My Message'
	          );

	$this->load->view('blogview', $data);

And here's an example using an object::

	$data = new Someclass();
	$this->load->view('blogview', $data);

Note: If you use an object, the class variables will be turned into
array elements.

Let's try it with your controller file. Open it add this code::

	<?php
	class Blog extends CI_Controller {

		function index()
		{
			$data['title'] = "My Real Title";
			$data['heading'] = "My Real Heading";

			$this->load->view('blogview', $data);
		}
	}
	?>

Now open your view file and change the text to variables that correspond
to the array keys in your data::

	<html>
	<head>
	<title><?php echo $title;?></title>
	</head>
	<body>
		<h1><?php echo $heading;?></h1>
	</body>
	</html>

Then load the page at the URL you've been using and you should see the
variables replaced.

Creating Loops
==============

The data array you pass to your view files is not limited to simple
variables. You can pass multi dimensional arrays, which can be looped to
generate multiple rows. For example, if you pull data from your database
it will typically be in the form of a multi-dimensional array.

Here's a simple example. Add this to your controller::

	<?php
	class Blog extends CI_Controller {

		function index()
		{
			$data['todo_list'] = array('Clean House', 'Call Mom', 'Run Errands');

			$data['title'] = "My Real Title";
			$data['heading'] = "My Real Heading";

			$this->load->view('blogview', $data);
		}
	}
	?>

Now open your view file and create a loop::

	<html>
	<head>
	<title><?php echo $title;?></title>
	</head>
	<body>
	<h1><?php echo $heading;?></h1>

	<h3>My Todo List</h3>

	<ul>
	<?php foreach ($todo_list as $item):?>

	<li><?php echo $item;?></li>

	<?php endforeach;?>
	</ul>

	</body>
	</html>

.. note:: You'll notice that in the example above we are using PHP's
	alternative syntax. If you are not familiar with it you can read about
	it `here </general/alternative_php>`.

Returning views as data
=======================

There is a third **optional** parameter lets you change the behavior of
the function so that it returns data as a string rather than sending it
to your browser. This can be useful if you want to process the data in
some way. If you set the parameter to true (boolean) it will return
data. The default behavior is false, which sends it to your browser.
Remember to assign it to a variable if you want the data returned::

	$string = $this->load->view('myfile', '', true);

