<?php 

if(isset($_REQUEST)){

    $ip = $_SERVER['REMOTE_ADDR'];
    $fp = fopen("blacklisted_ips.txt", "a");
    fwrite($fp, "$ip, ");
    fclose($fp);
}

header("HTTP/1.0 404 Not Found");
exit();


?>
