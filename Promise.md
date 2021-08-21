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
