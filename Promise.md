```javascript
let Students = [
    {Name : 'Rakesh', Subject: 'JavaScrit'},
    {Name : 'Priyanka', Subject: 'Marketing Cloud'}
  ]

function enrollStudent(student){
    //return resolve and reject
    // return new Promise((resolve, reject) =>{
    return new Promise(function(resolve, reject){
        const err = false;
        if(err === false){
            console.log('resolved');
            setTimeout(function(){
                Students.push(student);
                console.log('Student enrolled');
                resolve();
            }, 8000)
        }else{
            console.log('not resolved');
            reject();
        }
    })

  
}
function DisplayStudent(){
  setTimeout(function(){
        Students.forEach(function(student){
        console.log(student.Name);
    });
    }, 4000)
    
}

enrollStudent({Name:'Roushan',  Subject:'Colour'}).then(function(){
    DisplayStudent();
});
//enrollStudent({ Name: 'Roushan', Subject: 'Colour' }).then(()=>DisplayStudent());
//enrollStudent({ Name: 'Roushan', Subject: 'Colour' }).then(DisplayStudent);

//DisplayStudent(); 
```

```javascript
let Students = [
    { Name: 'Rakesh', Subject: 'JavaScrit' },
    { Name: 'Priyanka', Subject: 'Marketing Cloud' }
]

function enrollStudent(student) {
    return new Promise((resolve, reject) =>{
        const err = false;
        if (err === false) {
            console.log('resolved');
            setTimeout(function () {
                Students.push(student);
                console.log('Student enrolled'); 
                const con = 'value fetched from backend class';
                resolve(con);
            }, 8000)
        } else {
            console.log('not resolved');
            reject();
        }
    })


}
function DisplayStudent() {
    setTimeout(function () {
        Students.forEach(function (student) {
            console.log(student.Name);
            
        });
    }, 4000);
   

}

//enrollStudent({ Name: 'Roushan', Subject: 'Colour' }).then(response=>{
enrollStudent({ Name: 'Roushan', Subject: 'Colour' }).then(function(response){
    DisplayStudent();
    setTimeout(function () {console.log(response);}, 5000);
    
});



//DisplayStudent(); 
```

```javascript
function resolveAfter2Seconds(x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}

async function f1() {
  var x = await resolveAfter2Seconds(10);
  console.log(x); // 10
}

f1();

```

```javascript
function doSomething(msg){ 
  return new Promise(resolve =>{
    
    setTimeout(() =>{
      console.log(msg);
        setTimeout(()=>{
          resolve(msg+msg);
        }, 3000);
         
    }, 1000);
 
  });
}

async function f1(){
  var rak = await doSomething('rakesh');
  console.log('rak->'+rak);
  var sin = await doSomething('singh');
  console.log('sin->'+sin);
}

f1();
```

```javascript
function doSomething(msg){ 
  return new Promise((resolve, reject) => {
      setTimeout(
        () => {
          try {
            throw new Error('bad error');
            console.log(msg);
            resolve();
          } catch(e) {
            reject(e);
          }
        }, 
        1000);
    }) 
}
    
doSomething("1st Call")
  .then(() => doSomething("2nd Call"))
  .then(() => doSomething("3rd Call"))
  .catch(err => console.error(err.message));  
  
Another way:
async function doSomethingManyTimes() {
  try {
    await doSomething("1st Call");
    await doSomething("2nd Call");
    await doSomething("3rd Call");
  } catch (e) {
    console.error(e.message);
  }
}
      
doSomethingManyTimes();  
  

```
