```javascript
let Students = [
    {Name : 'Rakesh', Subject: 'JavaScrit'},
    {Name : 'Priyanka', Subject: 'Marketing Cloud'}
  ]

function enrollStudent(student){
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
