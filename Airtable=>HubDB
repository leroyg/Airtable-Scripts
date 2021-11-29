
let table = base.getTable("Employee Directory"); //Defining the Table that will be used
let result = await table.selectRecordsAsync(); //pulling in the data from the table
//
let config = input.config(); //Defining the configuration of the input
let view = table.getView("Employees"); //Defining the view

let APIKEY = "asdfasdfasdfasd"; //Defining the API Key
let tableid = "2664332"; //Defining the TableID, This will be found in the URL when you go to the HubDB.

//Setting up the variable that will be used.
//These are input variables that came from the trigger.
var name = config.name;

var path = name.toLowerCase().replace(" ", "-");

console.log(path)

var title = config.title;

var headshot = config.headshot;

console.log(headshot)

var department = config.department;



var linkedin = config.linkedin;

var work_phone = config.work_phone;

var hsid = config.HSID;

var place_of_work = config.place_of_work;

var biography = config.biography;

var quote = config.quote;

var email = config.email;

var departmentid = 0;

var locationid = 0;

var updatedrecord=0;








//Gets the data from the HubDB and runs the idfinder function to find the ID's of the location and department.

function idfinderstart(){


fetch('https://api.hubapi.com/cms/v3/hubdb/tables/'+tableid+'?hapikey='+APIKEY, {
method: 'GET',


})
.then(response => response.json())

.then(json => idfinder(json))

//This function finds the ID's of the location and department, these will then be used to update the record,
//Because the Work Location Field and the Department are single-select you need to find the ID of the option.
function idfinder(tabledetails){

console.log(tabledetails)

for (let column of tabledetails.columns){

if(column.name == "team"){

for(option of column.options){



if (option.name == department[0].slice(3)){
departmentid = option.id;
console.log("Found the Department ID")
}
// else {console.log("The Department Name doesn't exist in HubDB")}
}
}

if(column.name == "work_location"){

for(option of column.options){

if (option.name == place_of_work){
locationid = option.id;
console.log("Found the location ID")
}
// else {console.log("The location Name doesn't exist in HubDB")}
}
}


}
}
}

idfinderstart();


//This function is used to check if the record exists in the HubDB. If it does, it will update the record. If it doesn't, it will create a new record.
fetch('https://api.hubapi.com/hubdb/api/v2/tables/'+tableid+'/rows/draft?hapikey='+APIKEY)
  .then(response => response.json())
  .then(data => checker(data));


function checker(data){

for (let row of data.objects){

if(row.name == name) 
{

updatedrecord=1;

fetch('https://api.hubapi.com/cms/v3/hubdb/tables/2663217/rows/'+row.id+'/draft?hapikey=b3c497ba-c5b8-4e68-bdc2-ffea6a60cebf', {
method: 'PATCH',
body: JSON.stringify({
"name":name,
"path":path, 
"values":{
"headshot":{
 "url": headshot[0],
  "type": "image"
},
"job_title":title,
//The work_location looks different because it is a single-select. To update it you need to use the id of the option, not the name.
"work_location":{
"id":locationid,
"type":"option"
},
//another single-select.
"team":{
  "id":departmentid,
  "type":"option"
},
"biography":biography,
"quote":quote,
"linkedin_url":linkedin,
"phone":work_phone,
"hsid":hsid
}
}),
headers: {
"Content-type": "application/json; charset=UTF-8"
}
})
.then(response => response.json())

.then(json => console.log(json))
console.log("updated")
}

}

//If the record doesn't exist, it will create a new record.
if (updatedrecord == 0){

console.log("The record doesn't exist in HubDB, creating it now")

fetch('https://api.hubapi.com/cms/v3/hubdb/tables/2663217/rows?hapikey=b3c497ba-c5b8-4e68-bdc2-ffea6a60cebf', {
method: 'POST',
body: JSON.stringify({
"name":name,
"path":path

}),
headers: {
"Content-type": "application/json; charset=UTF-8"
}
})
.then(response => response.json())

.then(json => console.log(json))


console.log("New Record Created")

}
}
