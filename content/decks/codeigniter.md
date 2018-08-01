+++
type="post"
title= "CodeIgniter"
tutorial_type='decks'
date= 2018-07-28T15:26:09+06:00
draft= false
weight= 1
authors= ["Polo Dev"]
categories= ["php", "back-end"]
tags= ["php", "codeigniter"]
dtags= ["php", "codeigniter"]
tutorialTypes=["decks"]
available_tutorialTypes= ["bangaldesh affairs", "decks", "international affairs", "math", "english", "bangla", "tutorial"]
+++

# 1 - setting up base url
~~~php
// file: application/config.php
$config['base_url'] = 'http://localhost:8000';
~~~

# 2 - to load a view
~~~php
// file: controllers > Properties.php
class Properties extends CI_Controller
{
  public function index()
  {
    $this->load->view('layouts/header');
    $this->load->view('layouts/navbar');
    $this->load->view('properties/index')
    $this->load->view('layouts/footer');
  }
}
~~~

# 3 - automatic url in codeigniter
~~~bash
index.php/{cotroller}/{method}/[param1]/[param2]
~~~

# 4 - passing value in to the view
~~~php
$data['user_name'] = 'Bernard';
$data['status_group'] = ['all', 'available', 'unavailable'];
$this->load->view('properties/index', $data)
~~~

# 5 - controller with params
how codeIgniter making route automatically
~~~php
public function show($id)
{
  $data['id'] = $id;
  $this->load->view('properties/show', $data)
}
~~~

# 6 - model
model should hold the business role. eg. In laravel model we hidden some properties using `protected hidden`

~~~php
// file: models/Property.php
class Property extends CI_MODEL
{
  public function __construct()
  {
    parent::__construct();
  }
  public function get()
  {
    return "4 bedroom 2 story house"
  }
}
// using model in controller
// controllers/properties
$this->load->model('Property');
$data['name'] = $this->Property->get(); //"4 bedroom 2 story house"
$this->load->view('properties/index', $data);
~~~

# 7 - url helper
~~~php
// file: controllers/properties.php function: index
$this->load->helper('url');
// view file
echo base_url('assets/images/imagename.jpg');
echo site_url('properties/show/1'); // for href
~~~

# 8 - autoload helper globally like `url`
~~~php
// file: config/autoload.php
$autoload['helper'] = ['url']
~~~

# 9 - database config
~~~php
// file: config/database.php
$db['default'] = [
  .....,
  'username' => $_ENV['DB_USER'],
  'password' => $_ENV['PASSWORD'],
  'database' => $_ENV['DB_NAME'],
  ......
]
~~~

# 10 - connecting database
~~~php
//file: models/Property.php
public function connection_test()
{
  $this->load->database('default', true)
}
// testing connection from controller
// file: controllers/Properties.php
public function db_test
{
  $this->load->model('Property');
  $this->Property->connection_test();
}
// passing database credential using $_ENV super global for testing
// env DB_USER=root DB_PASSWORD=root  DB_DATABASE=ci_course php -S localhost:8080
~~~

# 11 - autoload model globally instead of single function
~~~php
// file: config/autoload.php
$autoload['model'] = ['Property'] // case sensitive
~~~

# 12 - how to connect to database using model
~~~php
// file: models/Propery.php
// loading connection to whole model
public function __construct
{
  parent::__construct();
  $this->db = $this->load->database('default', true);
}
// testing to get database version
public version get_version()
{
  return $this->db->query('SELECT VERSION()');
}
// file: controllers/Properites.php
$version = $this->Property->get_version();
$data['version'] = $version->conn_id->server_info;
$this->load->view('properties/index', $data);
~~~

# 13 - getting all row from database table
~~~php
// model file: models/Property.php
public function all()
{
  $result_set = $this->db->get('poperties');
  return $result_set->result_array();
}
// controller file: controllers/Properties.php
$data['properties'] = $this->Property->all();
$this->load->view('properties/index', $data);
// views - now i can iterate in properties
~~~
# 14 - getting single row like laravel show function
~~~php
// file: models/Propery.php
public function get($id)
{
  $where['id'] => $id;
  $this->db->get_where('properties', $where);
}
// file: controllers/Properties.php
public function edit($id)
{
  $data['property'] = $this->Property->get($id);
  $this->load->view('properties/edit', $data);
}

~~~
# 15 - update
// file: models/Propery.php
~~~php
public function update($id, $new_data)
{
  $where['id'] = $id;
  $this->db->update('properties', $new_data, $where);
}
// file: controllers/Properties.php
public function edit($id)
{
  if ($_POST)
  {
    $new_data['name'] = $this->input->post('name');
    $new_data['description'] = $this->input->post('description');
    $this->Property->update($id, $new_data);
    redirect('properties/index');
  }
}
~~~

# 16 - redirect
~~~php
redirect('properties/index')
~~~

# 18 - autoload session
~~~php
// file: config/autoload.php
$autoload['libraries'] = ['session'];
// file: config/config.php
$config['sess_save_path'] = sys_get_temp_dir()

// file controllers/Properties.php
$session_data['selected_filter'] = $this->input->get('filter');
$this->session->set_userdata($session_data);
// pass variable to the view
$data['selected_filter'] = $this->session->selected_filter;
$this->load->view('layouts/footer', $data);
// file: views/properties/index.php
if (empty($selected_filter))
{
  // some htmml
}
~~~

# 19 - manipulating browser using `output` class
~~~php
public function kml_export()
{
  $this->output->set_content_type('application/xml')
  $this->load->view('properties/kml_xml')
}
~~~

# 20 - force content to download
~~~php
$this->output->set_content_type('application/octet-stream')
$this->load->view('properties/kml_xml')
~~~

# 21 - change the file download name
~~~php
$this->output->set_content_type('application/octet-stream')
header('Content-Disposition: inline; filename="real_estate_kml_export.kml"');
$this->load->view('properties/kml_xml')
~~~

# 22 - showing an image
~~~php
$image = file_get_contents('assets/images/imagename.jpg');
$this->output->set_content_type('jpg')->set_output($image);
~~~


# 23 - helper for file upload
~~~php
// file: controllers/Properties.php
$this->laod->helper('form') // this is the perfect example to load manual helper. since we don't need form in every where
// file: views/properties.php // instead of form tag
echo form_open_multipart('')
~~~

# 24 - file uploading
~~~php
public function doupload()
{
  $config['upload_path'] = './uploads/';
  $config['allowed_types'] = 'gif|jpg|png'
  $this->load->library('upload', $config);
  $this->upload->do_upload('image_file');
  $data = $this->upload->data();
  return $data['file_name']
}
public function edit()
{
  if($_POST)
  {
    $image = false;
    if($_FILES)
    {
      $image = $this->doupload();
    }
    if ($image) {
      $new_data['image'] = $image;
    }
  }
  $this->Property->update($id, $new_data);
}
~~~

# 25 - form validation - never trust end user.
~~~php
$this->load->library('form_validation');
$this->form_validation->set_rules('name','Name', 'required');
$this->form_validation->set_rules('description','Description', 'required');
if($this->form_validation->run()) {  } // if form is validate we will update
~~~
showing errors in form
~~~phtml
<?php if(form_error('name')): ?>
  <div class='alert alert-danger'><?php echo form_error('name') ?> </div>
<?php endif; ?>
~~~

# 26 - persist old value
~~~php
if(!$this->form_validation->run())
{
  // overwrite name and description key by input value
  $data['property']['name'] = $this->input->post('name');
  $data['property']['description'] = $this->input->post('description');
  // now passign $data to view
}
~~~

# 27 - log for debug
generated log file will be available inside logs/log-date-formatted.php
~~~php
// file: config/config.php
$config['log_threshold'] = 4; // 4 = all message
$config['log_threshold'] = 2; // 2 = debug message
//file controllers/Properties.php
log_message('debug', 'My first Log');
log_message('debug', 'From parameters' . print_r( $_POST, true )) // print_r 2nd parameter for return it
~~~

# 28 - Using composer in codeigniter
First we need to create a library and added to autoload library
~~~php
// file: libraries/Composer_loader.php
class Composer_loader
{
  public function __construct()
  {
    include './vendor/autoload.php';
  }
}
// file: config/autoload.php
$autoload['libraries'] = ['Composer_loader'];
~~~
























