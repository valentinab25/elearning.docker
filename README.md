# Deployment recipe for the e-learning platform

# Local installation instructions

1. Clone this repository locally
2. Copy template files:

	- `docker-compose.override.tpl.yml` to `docker-compose.override.yml` and map nginx ports.
	- `etc/nginx.elearning.tpl.conf` to `nginx.elearning.conf` and setup hostname

3. Clone the elearning repository `git clone https://github.com/informea/elearning.git docroot` and create a `config.php`

```php
<?php  // Moodle configuration file

unset($CFG);
global $CFG;
$CFG = new stdClass();

$CFG->dbtype    = 'mysqli';
$CFG->dblibrary = 'native';
$CFG->dbhost    = 'db';
$CFG->dbname    = 'elearning';
$CFG->dbuser    = 'root';
$CFG->dbpass    = 'root';
$CFG->prefix    = 'mdl_';
$CFG->dboptions = array (
  'dbpersist' => 0,
  'dbport' => '',
  'dbsocket' => '',
);

$CFG->wwwroot   = 'http://TODO';
$CFG->dataroot  = '/var/www/repository/elearning/';
$CFG->admin     = 'admin';

$CFG->directorypermissions = 0777;

$CFG->passwordsaltmain = 'secret-salt';

$CFG->xsendfile = 'X-Accel-Redirect';
$CFG->xsendfilealiases = array(
     '/dataroot/' => $CFG->dataroot,
);

require_once(dirname(__FILE__) . '/lib/setup.php');

// There is no php closing tag in this file,
// it is intentional because it prevents trailing whitespace problems!
```

4. Get a copy of the data repository containing the files and put them in `repository/` dir here.
5. Start the docker stack and visit your website.
