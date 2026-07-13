
<html lang="fr">
<head>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Mon Organisateur Pro</title>

<style>

*{
box-sizing:border-box;
font-family:Arial;
margin:0;
padding:0;
}

body{
background:#eef2f7;
padding:20px;
}


/* CONNEXION */

#login{
position:fixed;
top:0;
left:0;
width:100%;
height:100%;
background:#0f172a;
display:flex;
justify-content:center;
align-items:center;
z-index:10;
}


.login-box{

background:white;
padding:30px;
border-radius:15px;
width:320px;
text-align:center;
box-shadow:0 10px 30px #0005;

}


.login-box input{

margin-top:15px;

}



button{

background:#2563eb;
color:white;
border:none;
padding:10px;
border-radius:8px;
cursor:pointer;
margin-top:8px;

}


button:hover{

background:#1d4ed8;

}


.container{

display:grid;
grid-template-columns:repeat(auto-fit,minmax(300px,1fr));
gap:20px;

}


.card{

background:white;
padding:20px;
border-radius:20px;
box-shadow:0 10px 25px #0002;

}


h1{

text-align:center;
margin-bottom:25px;

}


h2{

color:#2563eb;
margin-bottom:15px;

}


input,textarea{

width:100%;
padding:10px;
border-radius:8px;
border:1px solid #ccc;
margin-bottom:10px;

}


textarea{

height:150px;

}


li{

list-style:none;
background:#eff6ff;
padding:10px;
margin:8px 0;
border-radius:8px;

}


.delete{

background:#dc2626;
float:right;

}


.money{

background:#ecfdf5;

}


.total{

font-size:22px;
font-weight:bold;
color:green;

}

</style>


</head>

<body>



<!-- CONNEXION -->

<div id="login">

<div class="login-box">

<h2>🔐 Code secret</h2>

<input type="password" id="code" placeholder="Entrer le code">

<button onclick="login()">
Entrer
</button>

<p id="error"></p>

</div>

</div>



<h1>
📋 Mon Organisateur Personnel PRO
</h1>



<div class="container">



<div class="card">

<h2>✅ Tâches</h2>

<input id="taskInput" placeholder="Nouvelle tâche">

<button onclick="addTask()">
Ajouter
</button>

<ul id="taskList"></ul>

</div>





<div class="card">

<h2>📅 Rendez-vous</h2>

<input type="date" id="date">

<input id="meeting" placeholder="Rendez-vous">

<button onclick="addMeeting()">
Ajouter
</button>

<ul id="meetingList"></ul>


</div>






<div class="card">

<h2>📝 Notes</h2>

<textarea id="notes"
placeholder="Mes notes..."></textarea>


</div>






<div class="card money">


<h2>💰 Mon Argent</h2>


<input id="argent"
type="number"
placeholder="Ajouter argent">


<button onclick="addMoney()">
Ajouter
</button>



<h3>
Total :
<span class="total" id="total">
0
</span> €
</h3>


</div>






<div class="card">


<h2>💳 Mes crédits / dettes</h2>


<input id="creditName"
placeholder="Nom du crédit">


<input id="creditAmount"
type="number"
placeholder="Montant">


<button onclick="addCredit()">
Ajouter crédit
</button>


<ul id="creditList"></ul>



<h3>
Total crédits :
<span id="creditTotal">
0
</span> €
</h3>


</div>



</div>



<script>


// CODE SECRET
// Le code par défaut est 1234

let secret="1234";


function login(){

let c=document.getElementById("code").value;


if(c==secret){

document.getElementById("login").style.display="none";

}

else{

document.getElementById("error").innerHTML="❌ Mauvais code";

}

}





let data=JSON.parse(localStorage.getItem("organisateurPro")) || {

tasks:[],
meetings:[],
notes:"",
argent:0,
credits:[]

};



function save(){

localStorage.setItem(
"organisateurPro",
JSON.stringify(data)
);

}



// TACHES


function addTask(){

let t=document.getElementById("taskInput");


if(t.value=="")return;


data.tasks.push(t.value);

t.value="";

save();

showTasks();

}


function showTasks(){

let l=document.getElementById("taskList");

l.innerHTML="";


data.tasks.forEach((x,i)=>{

l.innerHTML+=`

<li>
${x}

<button class="delete"
onclick="delTask(${i})">
X
</button>

</li>`;

});

}


function delTask(i){

data.tasks.splice(i,1);

save();

showTasks();

}




// RENDEZ VOUS


function addMeeting(){

let d=date.value;

let m=meeting.value;


if(!d||!m)return;


data.meetings.push(d+" - "+m);


meeting.value="";


save();

showMeeting();

}



function showMeeting(){

let l=document.getElementById("meetingList");

l.innerHTML="";


data.meetings.forEach((x,i)=>{


l.innerHTML+=`

<li>

${x}

<button class="delete"
onclick="delMeeting(${i})">
X
</button>

</li>`;

});

}



function delMeeting(i){

data.meetings.splice(i,1);

save();

showMeeting();

}



// NOTES


notes.oninput=function(){

data.notes=this.value;

save();

};



// ARGENT


function addMoney(){

let a=Number(argent.value);


data.argent+=a;


argent.value="";


save();

showMoney();

}


function showMoney(){

total.innerHTML=data.argent;

}



// CREDIT


function addCredit(){

let n=creditName.value;

let a=Number(creditAmount.value);


if(!n||!a)return;


data.credits.push({

name:n,

amount:a

});


creditName.value="";

creditAmount.value="";


save();

showCredits();

}



function showCredits(){

let l=document.getElementById("creditList");

l.innerHTML="";


let totalCredit=0;


data.credits.forEach((c,i)=>{


totalCredit+=c.amount;


l.innerHTML+=`

<li>

${c.name} : ${c.amount} €

<button class="delete"
onclick="delCredit(${i})">
X
</button>


</li>`;


});


creditTotal.innerHTML=totalCredit;


}



function delCredit(i){

data.credits.splice(i,1);

save();

showCredits();

}




// CHARGEMENT

showTasks();

showMeeting();

showMoney();

showCredits();


notes.value=data.notes;


</script>


</body>
</html>
