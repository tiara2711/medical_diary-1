<template>
    <br>
    <div id="div1"></div>
</template>

<script>
import firebaseApp from '../firebase.js';
import { query, deleteDoc, getFirestore, doc, orderBy } from "firebase/firestore";
import { collection, getDocs,setDoc } from "firebase/firestore";
import { getAuth } from "firebase/auth";


const db = getFirestore(firebaseApp);

export default {
    mounted() {
      
        async function display(){
            const auth = getAuth();
            var user = auth.currentUser.email;
            function getRandomColor() {

            return 'rgba(' +
            (Math.floor(Math.random() * 56) + 200) + ', ' +
            (Math.floor(Math.random() * 56) + 200) + ', ' +
            (Math.floor(Math.random() * 56) + 200) +
            ',.2)';
            }
            let notecol = String(user) + '.notes'
            let z = await getDocs(query(collection(db, notecol),orderBy("timeStamp")))
            var currentDiv = document.getElementById("div1");
            z.forEach((docs)=> {
                let yy = docs.data()
                var title1=(yy.title)
                var content1=yy.content
                var date1=yy.dateandtime
                var timestamp1 = yy.timeStamp
                var newDiv = document.createElement("div");
                newDiv.id= "newdiv";
                newDiv.style.backgroundColor=getRandomColor();
                
                var newtitle = document.createElement("p");
                newtitle.append(title1)
                newtitle.id="title11"
                var newContent = document.createTextNode(content1);
                var newdate = document.createElement("p");
                newdate.id="date";
                newdate.append(date1)
                var br2 = document.createElement("br");
                newDiv.appendChild(newtitle)
                
                newDiv.appendChild(newContent)
                newDiv.appendChild(br2)
                newDiv.appendChild(newdate)
                
                var bu = document.createElement("button")
                bu.title="Delete"
                bu.className = "btn"
                bu.id="btn"
                bu.onclick = function() {
                    deleteNote(title1)
                }
                newDiv.appendChild(bu)

                var editbtn = document.createElement("button")
                editbtn.title="Edit"
                editbtn.id="ebtn"
                editbtn.onclick = function() {
                  updateNote(title1, content1, date1, timestamp1, newDiv)
                }
                newDiv.appendChild(editbtn)
                currentDiv.insertBefore(newDiv,currentDiv.firstChild);
                
            });
        }
    display(String(this.user)+'.notes')

    async function updateNote(note, content, date, timestamp, division) {
      const auth = getAuth();
      var user = auth.currentUser.email;    
      var z = note
      var a = content
      var b = date
      var c = timestamp
      var zz = division
      let notecol = String(user) + '.notes'
      await deleteDoc(doc(db,notecol,z))
      var con = document.createElement("container")
      con.id = "con"
      var newNotetitle = document.createElement("input");
      newNotetitle.id = "newnotetitle"
      newNotetitle.value=String(note);
      // newNotetitle.placeholder = "Enter new title"
      var newNotecontent = document.createElement("textarea")
      newNotecontent.id = "newnotecontent"
      // newNotecontent.placeholder = "Enter new description"
      newNotecontent.value= String(a)
      var savebtn = document.createElement("button");
      savebtn.textContent="Save"
      savebtn.id = "savebtn"
      var cancelbtn = document.createElement("button");
      cancelbtn.textContent="Cancel"
      cancelbtn.id = "cancelbtn"
      zz.innerHTML = "";
      con.appendChild(newNotetitle)
      con.appendChild(newNotecontent)
      con.appendChild(savebtn)
      con.appendChild(cancelbtn)
      zz.appendChild(con)
      savebtn.onclick = function() {
          savetodb();     
      }
      cancelbtn.onclick = function() {
        savetodb2(z, a, b, c);
      }
    }

    async function savetodb() {
        const auth = getAuth();
        var user = auth.currentUser.email; 
        var a = document.getElementById("newnotetitle").value
        var b = document.getElementById("newnotecontent").value
        var c = new Date().toJSON().slice(0, 10).replace(/-/g, '/')
        var d = new Date()
        
        try{
                let notecol = String(user) + '.notes'
                const docRef = await setDoc(doc(db, notecol, a), {
                title: a, content: b, dateandtime: c, timeStamp: d
                })
                console.log(docRef)
                
                
            }
        catch(error) {
                
                console.error("Error adding note", error);
            }
        document.getElementById("div1").innerHTML="";
        display();
      }

      async function savetodb2(a, b ,c, d) {
        const auth = getAuth();
        var user = auth.currentUser.email;
        try{
                let notecol = String(user) + '.notes'
                const docRef = await setDoc(doc(db, notecol, a), {
                title: a, content: b, dateandtime: c, timeStamp: d
                })
                console.log(docRef)
            }
        catch(error) {
                console.error("Error adding note", error);
            }
        document.getElementById("div1").innerHTML="";
        display()   
      }

    async function deleteNote(note) {
        const auth = getAuth();
        var user = auth.currentUser.email;
        var z = note;
        let notecol = String(user) + '.notes'
        await deleteDoc(doc(db,notecol,z))
        document.getElementById("div1").innerHTML = "";
        display();
    }

    },
  
    
}
</script>

<style>
#newdiv{
  opacity: 1;
  display: flex;
  flex-direction: column;
  align-items: left;
  text-align: left;
    margin:0 auto;
  padding: 30px;
  justify-content: space-between;
  width: 60%;
  left: 0;
  top: 0;
  font-family: 'Noto Sans';
  font-size: 20px;
  height: auto;
  margin-bottom: 15px;
  box-sizing: border-box;
  overflow-wrap: break-word;
  position:relative;
}
#date{
    margin: 0;
  font-family: 'Noto Sans';
  font-size: 18px;
  
  position:absolute;
    bottom:0;
    right:0;
    margin-bottom: 3px;
    margin-right: 5px;
}
#title11{
  font-size: 21px;
  font-weight: bold;
  color: #D1D5DB;
  text-decoration: underline;
}
#btn{
    background:url(../assets/close-button.png);
    height:16px;
    width: 16px;
    border: none;
    position:absolute;
    top: 0;
    right:0;
    margin-top: 7px;
    margin-right:10px;
    
    transition: background-color 0.5s;
    
}
#ebtn{
  background: url(../assets/pencil.png);
  height:16px;
  width:16px;
  border: none;
  position: absolute;
  top: 0;
  right:0;
  margin-top:7px;
  margin-right: 35px;
  transition: background-color 0.5s;

}

#ebtn:hover{
  background-color:lightgray;
}
#ebtnfade:focus,
#ebtnfade:active {
  background-color:green;
  transition: none;
}


#btn:hover {
  background-color:red;
}

#btnfade:focus,
#btnfade:active {
  background-color: red;
  transition: none;
}

#con {
  width: 100%;
  margin: auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  
}
#newnotetitle {
  padding: 0 10px;
  min-height: 50px;
  box-sizing: border-box;
  outline: none;
  border: 2px solid #000;
  font-weight: bold;
  font-size: 18px;
  font-family: 'Noto Sans';
  border-bottom-color:rgb(202, 202, 202) ;
  
  
}

#newnotecontent {
  padding: 0 10px;
  min-height: 70px;
  margin-bottom: 5px ;
  box-sizing: border-box;
  outline: none;
  border: 2px solid #000;
  font-size: 18px;
  font-family: 'Noto Sans';
  border-top-style:none;
  text-align: left;
  
  
}
#savebtn {
 
  align-items: center;
  display: block;
  background-color: #373E47;
  border: 1px solid rgb(209,213,219);
  border-radius: .5rem;
  box-sizing: border-box;
  color:#ADBAC7;
  border: 1px solid #444c56;
  font-family: "Inter var",ui-sans-serif,system-ui,-apple-system,system-ui,"Segoe UI",Roboto,"Helvetica Neue",Arial,"Noto Sans",sans-serif,"Apple Color Emoji","Segoe UI Emoji","Segoe UI Symbol","Noto Color Emoji";
  justify-content: center;
  font-weight: 600;
  line-height: 0.25rem;
  position: relative;
  margin: 0 auto;
  text-align: center;
  text-decoration: none #D1D5DB solid;
  text-decoration-thickness: auto;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  cursor: pointer;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  width: 70px;
  height:30px

}
#savebtn:hover,
#savebtn:focus {
  border-color: rgba(0, 0, 0, 0.15);
  box-shadow: rgba(0, 0, 0, 0.1) 0 4px 12px;
  color: white;
}

#savebtn:hover {
  transform: translateY(-1px);
}

#savebtn:active {
  background-color: rgb(202, 202, 202);
  border-color: rgba(0, 0, 0, 0.15);
  box-shadow: rgba(0, 0, 0, 0.06) 0 2px 4px;
  color: rgba(0, 0, 0, 0.65);
  transform: translateY(0);
}
#cancelbtn {
  align-items: center;
  display: block;
  background-color: #373E47;
  border: 1px solid rgb(209,213,219);
  border-radius: .5rem;
  box-sizing: border-box;
  color:#ADBAC7;
  border: 1px solid #444c56;
  font-family: "Inter var",ui-sans-serif,system-ui,-apple-system,system-ui,"Segoe UI",Roboto,"Helvetica Neue",Arial,"Noto Sans",sans-serif,"Apple Color Emoji","Segoe UI Emoji","Segoe UI Symbol","Noto Color Emoji";
  justify-content: center;
  font-weight: 600;
  line-height: 0.25rem;
  position: relative;
  margin: 0 auto;
  text-align: center;
  text-decoration: none #D1D5DB solid;
  text-decoration-thickness: auto;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  cursor: pointer;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  width: 70px;
  height:30px

  
}
#cancelbtn:hover,
#cancelbtn:focus {
  border-color: rgba(0, 0, 0, 0.15);
  box-shadow: rgba(0, 0, 0, 0.1) 0 4px 12px;
  color: white;
}

#cancelbtn:hover {
  transform: translateY(-1px);
}

#cancelbtn:active {
  background-color:rgb(202, 202, 202);
  border-color: rgba(0, 0, 0, 0.15);
  box-shadow: rgba(0, 0, 0, 0.06) 0 2px 4px;
  color: rgba(0, 0, 0, 0.65);
  transform: translateY(0);
}

</style>

