<?php 

function getIp() {
    if (!empty($_SERVER['HTTP_X_FORWARDED_FOR'])) {
        $ipList = explode(',', $_SERVER['HTTP_X_FORWARDED_FOR']);
        return trim($ipList[0]);
    }
    return $_SERVER['REMOTE_ADDR'] ?? '0.0.0.0';
}

function getIpInfo($key){
    $ip = getIp();
    $api = "http://ip-api.com/json/" . $ip;
    $res = @file_get_contents($api);

    if ($res === false) {
        return null;
    }

    $json = json_decode($res, true);
    return $json[$key] ?? null;
}

$country_code = getIpInfo("countryCode");

// Language groups
$en_arr = ["GB","US","CA"];
$es_arr = ["AR","BO","CL","CO","CR","CU","DO","EC","SV","ES","GQ","GT","HN","MX","NI","PA","PY","PE","PR","UY","VE"];
$de_arr = ["DE","AT","CH","LI","LU"];
$fr_arr = ["FR"];

if ($country_code && in_array($country_code, $de_arr)) {
    require 'de.php';
} elseif ($country_code && in_array($country_code, $es_arr)) {
    require 'es.php';
} elseif ($country_code && in_array($country_code, $fr_arr)) {
    require 'fr.php';
} else {
    require 'en.php';
}

?>
