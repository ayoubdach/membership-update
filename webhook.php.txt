<?php
// === CONFIG ===
$botToken = '8134569625:AAGcGs0mA29azqMoP95V6XPBm8dzXgL_tBk';
$bannedFile = __DIR__ . 'banned_ips.txt';

// === Read Telegram Update ===
$update = json_decode(file_get_contents("php://input"), true);

if (!$update) {
    exit("No update");
}

// If button pressed (callback)
if (isset($update['callback_query'])) {
    $callback = $update['callback_query'];
    $chatId   = $callback['message']['chat']['id'];
    $msgId    = $callback['message']['message_id'];
    $data     = $callback['data']; // e.g. ban_1.2.3.4
    $ip       = str_replace("ban_", "", $data);

    // Load banned list
    $banned = file_exists($bannedFile) ? file($bannedFile, FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES) : [];

    if (in_array($ip, $banned)) {
        // Already banned
        $text = "⚠️ IP <code>$ip</code> is already banned!";
    } else {
        // Ban new IP
        file_put_contents($bannedFile, $ip . PHP_EOL, FILE_APPEND | LOCK_EX);
        $text = "🚫 IP <code>$ip</code> has been banned!";
    }

    // Answer callback (popup small msg)
    file_get_contents("https://api.telegram.org/bot$botToken/answerCallbackQuery?" . http_build_query([
        'callback_query_id' => $callback['id'],
        'text' => $text,
        'show_alert' => true
    ]));

    // Also edit the message to show that it’s banned
    file_get_contents("https://api.telegram.org/bot$botToken/editMessageText?" . http_build_query([
        'chat_id' => $chatId,
        'message_id' => $msgId,
        'text' => $callback['message']['text'] . "\n\n❌ Banned IP: <code>$ip</code>",
        'parse_mode' => 'HTML'
    ]));
}
?>
