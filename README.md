<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">
    <script src='https://cdn.firebase.com/js/client/2.2.1/firebase.js'></script>    
	<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
	<link rel="stylesheet" href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.10.4/themes/smoothness/jquery-ui.css" />
	<script src="http://ajax.googleapis.com/ajax/libs/jqueryui/1.10.4/jquery-ui.min.js"></script>  
    <script src="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js"></script>	
	<!-- Angular -->
	<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.3.14/angular.min.js"></script>
	<!-- App -->	
	<!--<script src="js/orange_lobby.js"></script>-->
	<script>
	
	
	
	var app = angular.module('orangeApp', []);
	app.controller('gameCtrl', function($scope) {
		$scope.test = totalOranges;
	



	
	});
	
	
	
	
    //var myDataRef = new Firebase('https://orangegameadp.firebaseio.com/');
	//myDataRef.child("test").set("test");
	
	//var data = {totalScore: 0, basket: 10, totalOranges: 3}
	//var rounds = {round1:data, round2: data, round3: data}
	//myDataRef.child("scores").push(rounds);
	
	
/*	//Create or join game room with new firebase reference, create updated firebase callbacks.
function gameID(){
	//First remove all callback functions to the previous firebase reference
	myDataRef.child("payoffs").off();
	myDataRef.child("roundNo").off();
	myDataRef.child("rabbitMove").off();
	myDataRef.child("turtleMove").off();
	
	var x = document.getElementById("gameID").value;
	var y = 'https://orangegameadp.firebaseio.com/' + x + '/';

	myDataRef = new Firebase(y);
	//Set firebase value, callback function to start game.
function startRound(){	
	var x = {p1Payoffs: shuffleArr([1,2,3,4]), p2Payoffs: shuffleArr([1,2,3,4])};
	myDataRef.child("payoffs").set(x);	
	var x = roundNo + 1; 
	myDataRef.child("roundNo").set(x);
	}

//Needed to make this blanket callbacks function so that when the gameID changes I can recreate the callbacks with the new Firebase reference.
function createCallbacks(){
	//Callback functions when new payoffs/round is received from Firebase.
	myDataRef.child("payoffs").on('value', function(snapshot) {
		p1Payoffs = snapshot.val().p1Payoffs;
		p2Payoffs = snapshot.val().p2Payoffs;			
		});	
	myDataRef.child("roundNo").on('value', function(snapshot) {
		roundNo = snapshot.val();
		if(roundNo > 0){ startRound2();}
		});	
	//Callback function when rabbit's decision is recorded on Firebase
	myDataRef.child("rabbitMove").on('value', function(snapshot) {
		var x = snapshot.val();
		rabbitMove = x[0];
		if (x[1] == roundNo) {
			document.getElementById("east").disabled = true;
			document.getElementById("west").disabled = true;	
			rabbitStatus = true;	
			}			
		});	
	//Reset from previous game
	turtleScore = 0; rabbitScore = 0; roundNo = 0;
	document.getElementById(turtleMove + rabbitMove).style.border="thick solid #23754F";
	document.getElementById(rabbitMove).style.backgroundColor="#ffffc4";
	document.getElementById(turtleMove).style.backgroundColor="#ffffc4";
	document.getElementById("turScoreField").innerHTML = turtleScore;
	document.getElementById("rabScoreField").innerHTML = rabbitScore;
	document.getElementById("roundField").innerHTML = roundNo;
	
	createCallbacks();
	document.getElementById("roomName").innerHTML = x;
	document.getElementById("gameID").value = "";	
	}
*/
	var playernumber = "player1";
	var totalOranges = 0;
	var savedOranges= {player1:0, player2:0, player3:0, player4:0, player5:0, player6:0, player7:0,player8:0};
	var eatenOranges =0;
	var dayPoints= 0;
	var totalPoints= 100;
	var translateOranges=[10,9,8,7,6,5,4,3,2,1,0];
    var totalDays=0;
    var oet=0;
	var round=0;
	var countdown=60;
    //var OrangeNumber=0;
	//var limitDebt = 0;
	//var preventDebt= 0;
    //3,4,5,6,7,8,9,10
	//var twilight = 1000
	
//Controls instructions, to get transition effect, needed to change both opacity and visibility (so instructions don't later block clicks on the gamepage.
window.onload=function() {
    
	for(var i=1; i<11; i++ ) {        
        //document.getElementById("dragOrange"+i).style.visibility="hidden";
        }
    
	}
function timer(){  					
	if (countdown <= 0)  {
		document.getElementById("timeField").innerHTML = "Game Over";					
		return;
		}
	countdown--;
	document.getElementById("timeField").innerHTML = countdown; 
	setTimeout(timer,1000);
}
	function startFunc(){
	//This resets timer
	document.getElementById("timeField").innerHTML = 60;
	//Starts game timer				
	timer();				  
}
	
	function saveOrange() {
		if (totalOranges > 0) {
			savedOranges[playernumber]=savedOranges[playernumber]+1;
			document.getElementById("saved").innerHTML=savedOranges[playernumber];
			totalOranges=totalOranges-1;
			if(totalOranges<=0) {
				endDay();
				}
		}
	}	 
function endDay () {
	    document.getElementById("dish").style.visibility ="visible";
	}	
function metabolism() {
totalPoints=totalPoints-30;
dayPoints=dayPoints-30;
document.getElementById("totalPoints").innerHTML=totalPoints;
}	
function eatOrange ()
{       document.getElementById("comment").innerHTML="mangiato";
	if (totalOranges >0){
		eatenOranges=eatenOranges+1;
		document.getElementById("eaten").innerHTML= eatenOranges;
		totalOranges=totalOranges-1;
		if (eatenOranges<=11) { 
			dayPoints+=translateOranges[eatenOranges-1]; 
			document.getElementById("marginalUtility").innerHTML=translateOranges[eatenOranges-1];
			totalPoints+=translateOranges[eatenOranges-1];
			document.getElementById("totalPoints").innerHTML=totalPoints;
			}
		if(totalOranges<=0) { endDay(); };
    }
}
function spendSavings () {
		totalOranges=totalOranges+1;
        eatOrange();
		document.getElementById("saved")
	}
function nextDay ()
{    startFunc();
     metabolism();
    if(Math.random() < 0.5) {
		document.getElementById("dragOrange1").style.visibility ="visible";
		document.getElementById("dragOrange2").style.visibility ="visible";	 
		document.getElementById("dragOrange3").style.visibility ="visible";
		document.getElementById("dragOrange4").style.visibility ="visible";
		document.getElementById("dragOrange5").style.visibility ="visible";
		document.getElementById("dragOrange6").style.visibility ="visible";
		document.getElementById("dragOrange7").style.visibility ="visible";
		document.getElementById("dragOrange8").style.visibility ="visible";
		document.getElementById("dragOrange9").style.visibility ="visible";
		document.getElementById("dragOrange10").style.visibility ="visible";
		totalOranges=10;
	}
	else {
        totalOranges=0;
		endDay();
		alert("You had bad luck today!");
		if (savedOranges<=0) {
		startFunc();
		}
		}
    document.getElementById("eaten").innerHTML=0;
	eatenOranges=0;
    dayPoints=0;
    document.getElementById("marginalUtility").innerHTML=0;
    marginalUtility=0;
    totalDays++;
    document.getElementById("totalDays").innerHTML=totalDays;
	if (totalDays==0) {
	document.getElementById("timeField").innerHTML = "Game Over";
	}
}    

//This function gets data from input and sends to firebase using set function, set() function can send value or json object, uses child() to get variable (can also use push function to add to a list).
function sendMessage(){
console.log("sending message to " + i);
for (i=1; i<9; i++){
	var text = document.getElementById("message"+i).value;
		myDataRef.child("message"+i).set(text);
 console.log("sending message to " + i + " is complete.");		
		}
//This callback function is alerted when firebase value changes. It uses the on() function, firebase sends back a snapshot
		myDataRef.on('value', function(snapshot) {
			var dragon = snapshot.val();
			alert(JSON.stringify(dragon));
		})};
	
//sendOrange button creates new values and sends them to Firebase
function sendOrange(i){
    console.log("sending orange to " + i);
    var x = savedOranges[playernumber] - 1;
    myDataRef.child(playernumber).set(x);
    var y = savedOranges[i] + 1;
    myDataRef.child(i).set(y);
    console.log("sending orange to " + i + " is complete.");
	savedOranges[playernumber]=savedOranges[playernumber]-1;
	document.getElementById("saved").innerHTML=savedOranges[playernumber];
	}
//callback function for sent oranges. 
//That means everyone's browser will get the message that player 1 has one less orange in their basket. 
function gameID(){
myDataRef.child(playernumber).on('value'), function(snapshot) {
        savedOranges[playernumber] = snapshot.val();
		document.getElementById("saved").innerHTML= savedOranges[playernumber];
}}



function dragStart(event) {
    event.dataTransfer.setData("Text", event.target.id);	
	
	//These two lines make the original orange disappear, and set the dragged image to icon with id "dragIcon"
	setTimeout(function (){
	document.getElementById(event.target.id).style.visibility="hidden";
	  }, 10);   
	event.dataTransfer.setDragImage(dragIcon, 50, 50);	
}

function dragEnd(event){
	var x = event.dataTransfer.dropEffect;
	console.log("the dropEffect is: " + x);
	if (x == "none"){
		document.getElementById(event.target.id).style.visibility="visible";
	}
}

function allowDrop(event) {
    event.preventDefault();
}
function drop(event) {
    event.preventDefault();
    var data = event.dataTransfer.getData("Text");
    event.target.appendChild(document.getElementById(data)); 
}
function dropEat(event) {
    event.preventDefault();
    var data = event.dataTransfer.getData("Text");
	if (data.search("orangeSaved") == 0) {
		eatenOranges=eatenOranges+1;
		savedOranges[playernumber]=savedOranges[playernumber]-1;
		document.getElementById("eaten").innerHTML= eatenOranges;
		document.getElementById("saved").innerHTML= savedOranges[playernumber];
	    if (eatenOranges-1<12) {
			dayPoints+=translateOranges[eatenOranges-1]; 
			document.getElementById("marginalUtility").innerHTML=translateOranges[eatenOranges-1];
			totalPoints+=translateOranges[eatenOranges-1];
			document.getElementById("totalPoints").innerHTML=totalPoints;
			}
		else {
			dayPoints+=translateOranges[11];
			document.getElementById("marginalUtility").innerHTML=translateOranges[11];
			totalPoints+=translateOranges[11];
			document.getElementById("totalPoints").innerHTML=totalPoints;
		    }
	}
	else {
			eatOrange();	
		}
    document.getElementById(data).style.visibility="hidden";
}
function dropSave(event) {
    event.preventDefault();
    var data = event.dataTransfer.getData("Text");  
    saveOrange();
    var x = "";
    for (var i = 0; i < savedOranges[playernumber]; i++) {
        x += "<img id='orangeSaved" + i + "' src='http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png' draggable='true' ondragstart='dragStart(event)' ondragend='dragEnd(event)' alt='Smiley Orange' width='60' height='50'>";
	}
	document.getElementById("dropBasket").style.visibility="visible";
    document.getElementById("dropBasket").innerHTML = x; 
	if (i>=9) {alert("Your Basket is Full!"); }
}
</script>

<style type="text/css"> 
			<!--This is required for relative positioning to work.-->
			html, body{height: 100%; width: 100%;}
html { 
  background: url("http://s1.at.atcdn.net/wp-content/uploads/2011/03/100-Things-To-Do-Before-You-Die-100-Wilson-Island-Featured-Image.jpg") no-repeat center center fixed; 
  background-size: cover;
}			
body {
 	font-family:"comic sans ms";
	font-size: large;
	font-weight:bold;
	color: #FFFFFF;
     }
#title {
        position: relative;
		text-align:center;
		background-color:Chartreuse;
		color: OrangeRed;
}	
--> 
#gamePage      {
				position: absolute;
				height: 100%;
				width: 100%;
				border-radius:25px;	
				background-color: #45d1ff;
				background: linear-gradient( #45d1ff, #09f);	
				background:-webkit-linear-gradient(top, #45d1ff, #09f);	
					
				 					
			}	
#controlPanel {
	border-radius:25px;	
	width: 500px;
	padding: 5px;
	background-color:#D2691E;
	text-align: center;	
	position: relative;
	margin-top: 5px;	
	margin-left:auto;
	margin-right:auto;

}
.instructions {
	
    font-color: black;
	position: absolute;
	top:100px;	
	margin-left:-278px;
	left:50%;	
	background-color:Chartreuse;
	width:500px;
	padding:25px;
	border:3px solid #a1a1a1;
	border-radius:25px;
	z-index: 2;
	-webkit-transition: visibility 400ms, opacity 400ms; 
	transition: visibility 400ms, opacity 400ms;
}			
.market {
	
	position: absolute;
	top:100px;	
	margin-left:-278px;
	left:50%;	
	background-color:Chartreuse;
	width:500px;
	padding:25px;
	border:3px solid #a1a1a1;
	border-radius:25px;
	z-index: 2;
	-webkit-transition: visibility 400ms, opacity 400ms; 
	transition: visibility 400ms, opacity 400ms;
}	
			
			
#startDrag {
		   border-radius:25px;					
		   width: 425px;
		   padding: 5px;
		   text-align: center;
		   font-family: comic sans ms;
		   position: relative;
		   height: 90px; 
		   z-index:1;
		   margin-top: 195px;	
		   margin-left:auto;
		   margin-right:auto;
           
			}
.dropdish {
    width: 200px; 
    height: 200px;
    margin-left: auto;
    margin-right: auto;
	padding: 1px;
	border:1px solid red;
	}
#popUp {
        z-index=1
    }	
#dishOranges {					
			   width: 225px;
			   padding: 5px;
               text-align: center;			   
			   font-family: comic sans ms;
		       position: absolute;
	           height: 100px; 
	           top: 50px;
	           right:0px;
			   z-index:0;
			}	
.dropBasket {
    width: 200px; 
    height: 200px;
    margin-left: auto;
    margin-right: auto;
	padding: 1px;
	border:1px solid red;
	z-index=1;
}			
#basketOranges {				
				width: 225px;
				padding: 5px;
                text-align: center;				
				font-family: comic sans ms;
				position: absolute;
                height: 100px; 
	            top: 50px;
	            left:0px;
				z-index:1;       
			}		
#payoffs {
			border-radius:25px;					
			width: 425px;
			padding: 5px;
			text-align: center;
			font-family: comic sans ms;
			margin-left:auto;
		    margin-right:auto;
            position: relative;
	        height: 45px; 
		  top: -255px;	
		   margin-left:auto;
		   margin-right:auto;				
			}
.buttons {
			border-radius: 25px;				
			font-family: "comic sans ms";			
           }	 

.hello {
	margin-left:auto;
	margin-right:auto;
	width: 100px;
    height: 100px;
	text-align: center;
    background-color: #FF6C47;
    -webkit-animation-name: example; /* Chrome, Safari, Opera */
    -webkit-animation-duration: 4s; /* Chrome, Safari, Opera */
    animation-name: example;
    animation-duration: 4s;
	}

/* Chrome, Safari, Opera */
@-webkit-keyframes example {text-align: center;
    from {background-color: #FF6C47;}
    to {background-color: yellow;}
}

/* Standard syntax */
@keyframes example {text-align: center;
    from {background-color: #FF6C47;}
    to {background-color: yellow;}
}

/* This is for animating choices */
@-webkit-keyframes example {
	75%  {padding:4px;}	
	text-align:center;
}
@keyframes example {
    75%  {padding:4px;}
 	text-align:center;
}

                	<!--cursor:pointer;	
					color: blue;	
					background-color:yellow !important; 
					-webkit-animation: growing .7s linear .5s 10;
					animation: growing .7s linear .5s 10 ;
					animation-name: marginalUtility;
               }	 				
/* This is for animating choices */
@-webkit-keyframes  {
	0%   {width:70px; height:30px;}
    50%  {width:74px; height:35px;}	
}
@keyframes growing {
    0%   {width:70px; height:30px;}
    50%  {width:74px; height:35px;} 	
}   -->		   
</style>
</head>
<body>
<div id="dragIcon" style="position:absolute; left:-1000px;">
<img src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" width="60" height="50" ></div>

<div id="title"><h1>The Lonely Island 10-Day Survivor Challenge<h1></div>
<div ng-app="orangeApp" ng-controller="gameCtrl" style="position: relative; margin:50px;">
<div id="gamePage">
    
	<!--Control Panel-->
	
<table id="controlPanel">
		<tr><td>
		<button class="btn btn-primary" ng-show="startGame=true" ng-click="newDay=true; readyDay=true; showUtility=true" >Start Game</button>
		</td><!--<td>
		<button class="btn btn-primary" ng-click="controls = true">Hide controls</button>
	    </td></table><table id="controls" class="controls" ng-hide="controls=false">-->
		<td>
		<button class="btn btn-primary" ng-click="instructions1=true" >Help</button>
		</td><td>
		<button class="btn btn-primary" ng-click="market1=true" >Enter Market<y/button> {{market1}}
		</td>
		<td>
		<button class="btn btn-primary" ng-click="surveyPage=true">Survey</button>				
		</td><td>
		<p>Day</p><span class="buttons" id="totalDays">0</span></td>
		<td><p>Time</p><span class="buttons" id="timeField">60</span></td>
		</td></tr>
        </table>  

    <!--Instructions-->
		
	<div ng-show="instructions1" class="instructions" >
		<h2>The Orange Game: Overview</h2>
		<p>In this game, you and other players are castaways stranded on a desert island and cannot leave. </p>
		<p>Each castaway needs to survive. The island is rich of orange trees. To keep you fit, each day you go alone into the forest and look for oranges. Some days you are lucky and find ten oranges; some other days, you have bad luck and go back to your shelter empty-handed. </p>
		<p>Each person needs to eat at least an orange a day to keep herself healthy.</p>
		<button ng-click="instructions2=true; instructions1=false" class="btn btn-primary">Next</button>
	</div>
	<div ng-show="instructions2" class="instructions">
	    <h2>Fitness and Metabolism</h2>
		<p>Health is measured in fitness points. Life on the island is hard: every day spent on the island costs 5 fitness points.</p>
		<p>The fitness coming from eating oranges declines the more oranges you eat in a given day, because at some point your body cannot store the nutrients contained in the oranges.</p>
		<p>The first orange you eat, you get 10 points. The second, 9 points, and so on. The 10th orange gives you 1 fitness point. From the 11th orange you eat, you don't improve your health. </p>
		<button ng-click="instructions2=false; instructions1=true" class="btn btn-primary">Previous</button>
		<button ng-click="instructions2=false; instructions3=true" class="btn btn-primary">Next</button>
	</div>	
	<div ng-show="instructions3" class="instructions">
		<h2>Your Decisions</h2>
		<p>Every day, you may decide how many oranges to <b>save</b> and <b>eat</b>.</p> 
		<p>To <b>save</b> oranges, just drag and drop them into the <b>basket</b>. To <b>eat</b> oranges, just drag and drop them onto your <b>dish</b>. </p>
		<p>Of course, in days of bad luck, you can decide to grab oranges from your basket and eat them to keep you fit.</p>
		<button ng-click="instructions3=false; instructions2=true" class="btn btn-primary">Previous</button>
		<button ng-click="instructions3=false; instructions4=true" class="btn btn-primary">Next</button>			
	</div>
	<div ng-show="instructions4" class="instructions">
		<h2>Trade Opportunities</h2>
		<p>Another opportunity to get the oranges you need is trading with others. Every day, you may decide how many oranges from your basket you wish to offer to other castaways in exchange for future help.</p>
		<p>At the same time, you may ask other players to lend you oranges.</p>
		<button ng-click="instructions4=false; instructions3=true" class="btn btn-primary" >Previous</button>
		<button ng-click="instructions4=false; instructions5=true" class="btn btn-primary">Next</button>
	</div>	
	<div ng-show="instructions5" class="instructions">
	    <h2>You Make the Price</h2>
	    <p>Just click on the "Enter Market" button to access the market lobby in the middle of the island. Remember: you are trading oranges today for oranges tomorrow.</p> 
		<p>You are free to decide the price with the other players. A board will show you who wishes to lend and borrow what.</p>
	    <button ng-click="instructions5=false; instructions4=true" class="btn btn-primary">Previous</button>
		<button ng-click="instructions5=false" class="btn btn-primary">Close</button>
	</div>  
	
	<!--Market-->
	
	<div class="market" ng-show="market1">
		<h2>Welcome to the Orange Market!</h2>
		<img id="Trade" src="http://www.furrydtails.com/news/weareamemberoflocaltradepartners/local%20trade%20partners%20logo.png" width=100px height=100px alt= "Trade" id="trade" ></img>
		<h3><i>Your</i> Trade Proposals</h3>
		<p>In this area, you can make <b>offers</b> and <b>requests</b> to the other castaways.</p>		
		<p>Transactions happen in real time!</p>
		<p>I <b>offer</b> these oranges:</p>
		Quantity (between 0 and 10):
        <input type="number" name="quantity" min="0" max="10">
        <!--<input type="submit"class=buttons>This "submit" button will be activated after a Firebase connection is established-->
		<p>I <b>request</b> these oranges:</p>
        Quantity (between 0 and 10):
        <input type="number" name="quantity" min="0" max="10">
        <!--<input type="submit"class=buttons>This "submit" button will be activated after a Firebase connection is established-->
		<p>On the next page, you can check out the others' proposals.</p>
		<button ng-click="market2=true; market1=false"	class="btn btn-primary">Next</button>
		<button ng-click="market1=false" class="btn btn-primary">Close</button></td></tr>
	</div>
	<div  ng-show="market2" class="market" >
		<h2>Welcome to the Orange Market!</h2>
		<img id="account" src="http://www.furrydtails.com/news/weareamemberoflocaltradepartners/local%20trade%20partners%20logo.png" width=100px height=100px alt= "Keep Track" style="visibility:visible"></img>
		<h3><i>Others'</i> Trade Proposals</h3>
		<p>In this area, you can take a look at the <b>offers</b> and <b>requests</b> of other castaways.</p>
		<table>
		<tr><td><b>Player 1</b></td><td><input type="text" placeholder= "Message"><id="message1" onkeyup="javascript: if(event.keyCode==13) {this.value='';} else {sendMessage();"></input></td><td><button id="lend1" onclick="sendOrange('player1')" class=buttons>Send An Orange</button></td></tr>
        <tr><td><b>Player 2</b></td><td><input type="text" placeholder= "Message"><id="message2" onkeyup="javascript: if(event.keyCode==13) {this.value='';} else {sendMessage();"></input></td><td><button id="lend2" onclick="sendOrange('player2')" class=buttons>Send An Orange</button></td></tr> 
        <tr><td><b>Player 3</b></td><td><input type="text" placeholder= "Message"><id="message3" onkeyup="javascript: if(event.keyCode==13) {this.value='';} else {sendMessage();"></input></td><td><button id="lend3" onclick="sendOrange('player3')" class=buttons>Send An Orange</button></td></tr>		
        <tr><td><b>Player 4</b></td><td><input type="text" placeholder= "Message"><id="message4" onkeyup="javascript: if(event.keyCode==13) {this.value='';} else {sendMessage();"></input></td><td><button id="lend4" onclick="sendOrange('player4')" class=buttons>Send An Orange</button></td></tr>  	
        <tr><td><b>Player 5</b></td><td><input type="text" placeholder= "Message"><id="message5" onkeyup="javascript: if(event.keyCode==13) {this.value='';} else {sendMessage();"></input></td><td><button id="lend5" onclick="sendOrange('player5')" class=buttons>Send An Orange</button></td></tr>
        <tr><td><b>Player 6</b></td><td><input type="text" placeholder= "Message"><id="message6" onkeyup="javascript: if(event.keyCode==13) {this.value='';} else {sendMessage();"></input></td><td><button id="lend6" onclick="sendOrange('player6')" class=buttons>Send An Orange</button></td></tr>
        <tr><td><b>Player 7</b></td><td><input type="text" placeholder= "Message"><id="message7" onkeyup="javascript: if(event.keyCode==13) {this.value='';} else {sendMessage();"></input></td><td><button id="lend7" onclick="sendOrange('player7')" class=buttons>Send An Orange</button></td></tr>
        <tr><td><b>Player 8</b></td><td><input type="text" placeholder= "Message"><id="message8" onkeyup="javascript: if(event.keyCode==13) {this.value='';} else {sendMessage();"></input></td><td><button id="lend8" onclick="sendOrange('player8')" class=buttons>Send An Orange</button></td></tr>		
		</table>
		
		<!--Need to put stuff here: (1)let other player names pop up with offer and request amount; (2) need to have some communication going on between players 
(Offer:x amount: accept vs. decline); this once Firebase is up and running.-->
		
		<p>On the next page, you get a recap of the overall credit and debit situation on the island.</p>
		<button ng-click="market2=false; market1=true" class="btn btn-primary">Previous</button>
		<button ng-click="market2=false; market3=true" class="btn btn-primary">Next</button>
		<button ng-click="market2=false" class="btn btn-primary">Close</button>
	</div>
	<div id="market3" ng-show="market3" class="market" >
		<h2>Bookkeeping</h2> <img id="account" src="http://files.softicons.com/download/business-icons/financial-accounting-icons-by-artistsvalley/png/256x256/Regular/Abacus.png" width=100px height=100px alt= "Keep Track" id="account" style="visibility:visible"></img>
		<p>In this area, you can keep track of your credits and debits toward the other castaways.</p>
        <!--Need to put stuff here: (1)let other player names pop up with the net position vis-a-vis each one; (2) put a global net position standing, broke down by credits and debits; this once Firebase is up and running.-->
		<p><b>AREA UNDER CONSTRUCTION</b></p>
		<button ng-click="market2=true; market3=false" class="btn btn-primary">Previous</button>
		<button ng-click="market3=false;" class="btn btn-primary">Close</button>
	</div>
	
	<!--Survey-->
	
	<div ng-show="surveyPage" class="instructions">	
		<h4>Survey</h4>
		<p>What is your sex?
		<select ng-model="survey.q3">
			<option value="female">female</option>
			<option value="male">male</option>			
		</select></p>
		<p>What is your age?
		<input type="number" ng-model="survey.q4" min="18" style="width:50px;" >
		</p>
		<p>What is your ethnicity?
		<input type="text" ng-model="survey.q5" >
		</p>		
		<p>How did you make your decisions in this study?</p>
		<tr>
		<textarea ng-model="survey.q1" rows="2" cols="35"></textarea>		
		</tr>
		<p>Please write any other comments about the study below.</p>
		<tr>
		<textarea ng-model="survey.q2" rows="2" cols="35"></textarea>			
		</tr>
		<p>This is your participant code: {{test}} <br>
		You will use this code to receive your payment, please write it down before submitting the survey.</p>				
		<p><button class="btn btn-primary" ng-click="surveyPage=false; saveSurvey();">Submit</button></p>	
		<p>Your sex is {{survey.q3}}, your age is {{survey.q4}}, test this {{survey}}</p>
	</div>  
	
	<!--Dish-->
	
<table id = "dishOranges">
	<tr><td>			
	</p>				
	</td><td>
		<div class="dropdish" ondrop="dropEat(event)" ondragover="allowDrop(event)"> <img src="http://www.clker.com/cliparts/9/9/f/0/1194984076762834840small_plate.svg.med.png" width=200px height=200px alt= "Empty Dish" id="dish"><table id="popUp"><tr><td><p><span id="marginalUtility" ng-show="showUtility" class="hello">0</span></table></div>
		</td></tr>	 
	</table>
	
	<!--Daily Findings Area-->
	
		
<table id="startDrag" border=1>
	<tr>
	<td><img id="dragOrange1" src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" draggable="true" ondragstart="dragStart(event)" ondragend="dragEnd(event)" alt="Smiley Orange" width="60" height="50" style= "visibility:hidden;z-index=1"></td>
	<td><img id="dragOrange2" src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" draggable="true" ondragstart="dragStart(event)" ondragend="dragEnd(event)" alt="Smiley Orange" width="60" height="50" style= "visibility:hidden; z-index=1"></td>
	<td><img id="dragOrange3" src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" draggable="true" ondragstart="dragStart(event)" ondragend="dragEnd(event)" alt="Smiley Orange" width="60" height="50" style= "visibility:hidden; z-index=1"></td>
	<td><img id="dragOrange4" src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" draggable="true" ondragstart="dragStart(event)" ondragend="dragEnd(event)" alt="Smiley Orange" width="60" height="50" style= "visibility:hidden; z-index=1"></td>
	<td><img id="dragOrange5" src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" draggable="true" ondragstart="dragStart(event)" ondragend="dragEnd(event)" alt="Smiley Orange" width="60" height="50" style= "visibility:hidden; z-index=1"></td>
	</tr><tr>
	<td><img id="dragOrange6" src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" draggable="true" ondragstart="dragStart(event)" ondragend="dragEnd(event)" alt="Smiley Orange" width="60" height="50" style= "visibility:hidden; z-index=1"></td>
	<td><img id="dragOrange7" src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" draggable="true" ondragstart="dragStart(event)" ondragend="dragEnd(event)" alt="Smiley Orange" width="60" height="50" style= "visibility:hidden; z-index=1"></td>
	<td><img id="dragOrange8" src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" draggable="true" ondragstart="dragStart(event)" ondragend="dragEnd(event)" alt="Smiley Orange" width="60" height="50" style= "visibility:hidden; z-index=1"></td>
	<td><img id="dragOrange9" src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" draggable="true" ondragstart="dragStart(event)" ondragend="dragEnd(event)" alt="Smiley Orange" width="60" height="50" style= "visibility:hidden; z-index=1"></td>
	<td><img id="dragOrange10" src="http://www.clker.com/cliparts/2/8/1/4/11949861801973459319orange_simple.svg.med.png" draggable="true" ondragstart="dragStart(event)" ondragend="dragEnd(event)" alt="Smiley Orange" width="60" height="50" style= "visibility:hidden; z-index=1"></td>
	</tr>
	</table>
	
	<!--Basket-->
	
<table id = "basketOranges">	
	   <tr><td>
	   <div id="dropBasket" class="dropBasket" ondrop="dropSave(event)" ondragover="allowDrop(event)">
	   <img src= "http://www.clker.com/cliparts/y/w/q/7/x/g/empty-basket-hi.png" style="width:200px; height:200px;" alt= "Empty Basket" id="basketImage" ></div>
	  </td></tr>
	</table>		   

		<!--ScoreBoard-->	  
			  
		<table id= "payoffs" border=1>
			<tr><td>				
			<h4>Oranges Saved</h4>
			<p><span id="saved">0 </span> </p>
			</td><td>
			<h4>Oranges Eaten Today</h4>
			<p><span id="eaten">0 </span> <span id="comment" style="visibility:hidden" >abc</span> </p>
			</td><td>				
			<h4> Your Total Health Points Are:</h4>
			<p><span id="totalPoints" class="hello">0</span></p>
			</td></tr>
			<tr><td colspan=2>				
			<button class="btn btn-success" ng-show="newDay" id="next" onclick="startFunc(); nextDay();" ng-click="newDay=false" >Let a New Day Begin!</button>
			</td><td colspan=2>
		    <button class="btn btn-primary" ng-show="readyDay" onclick=" startFunc(); endDay();" ng-click="newDay=true"; "startGame=false" id="resetGame">Done for Today!</button>
		    </td>
			</tr>
			</table>
	</div>			
</div>				
</body>
</html>
