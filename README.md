CMS-Source-code
===============

CMS

// index.php in de main folder

<?php
 
require( "config.php" ); // configuratie includen behalve als het faalt
$action = isset( $_GET['action'] ) ? $_GET['action'] : "";
 
switch ( $action ) {
  case 'archive':
    archive();
    break;
  case 'viewArticle':
    viewArticle();
    break;
  default:
    homepage();
}
 
 
function archive() {
  $results = array();
  $data = Article::getList(); // haalt het lijst op met de gegeven archive
  $results['articles'] = $data['results'];
  $results['totalRows'] = $data['totalRows'];
  $results['pageTitle'] = "Article Archive | Widget News";
  require( TEMPLATE_PATH . "/archive.php" );
}
 
function viewArticle() { // 
  if ( !isset($_GET["articleId"]) || !$_GET["articleId"] ) {
    homepage();
    return;
  }
 
  $results = array();
  $results['article'] = Article::getById( (int)$_GET["articleId"] );
  $results['pageTitle'] = $results['article']->title . " | Widget News";
  require( TEMPLATE_PATH . "/viewArticle.php" );
}
 
function homepage() {
  $results = array();
  $data = Article::getList( HOMEPAGE_NUM_ARTICLES );
  $results['articles'] = $data['results'];
  $results['totalRows'] = $data['totalRows'];
  $results['pageTitle'] = "Widget News";
  require( TEMPLATE_PATH . "/homepage.php" );
}
 
?>

========================================================================================

//config.php

<?php
ini_set("display_errors", false); // set on TRUE waneer website live is! ( debugging only )
date_default_timezone_set("Europe/Amsterdam"); // http://www.php.net/manual/en/timezones.php
define( "DB_DSN", "mysql:host=localhostldbname=cms" );
define( "DB_USERNAME", "username" );
define( "DB_PASSWORD", "password" );
define( "CLASS_PATH", "Classes" );
define( "TEMPLATE_PATH", "templates" );
define( "HOMEPAGE_NUM_ARTICLES", 5 );
define( "ADMIN_USERNAME", "admin" ); // wachtwoord nog aanpassen + inlognaam voor admin!
define( "ADMIN_PASSWORD", "root" ); // voor nu gewoon admin en root
require( CLASS_PATH . "/Artile.php" );

function handleException( $exception ) {
	echo "Sorry, a problem occurred. Please try later.";
	error_log( $exception->getMessage());
}

set_exception_handler( 'handleException');

?>
====================================================================================================

