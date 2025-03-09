# evidencias-

const express = require("express")
const cors = require("cors")
const app= express()
const mysql = require("mysql")
app.use(cors())
app.use(express.json())

const connection = mysql.createConnection({
    host: "db4free.net",
    user: "estudiantesweb",
    password:"admin12345",
    database:"cursoweb",
    port: 3306
})
connection.connect(err=>{
    if(err) throw err;
    console.log("conectado a msql")
})

app.get("/", (req,res)=>{
    res.send("servidor online")
})


app.get("/listado", (req,res)=>{
    res.send(producto)  
});


let producto = [ 
    {id: 1, nombre:"audifono", precio:50},
    {id:2, nombre:"cama", precio:40},
    {id: 3, nombre:"sofa", precio:30},
] //post para agregar

app.delete("/producto/:id", (req,res)=>{
    const  {id} = req.params
    producto= producto.filter(p=>p.id !=id) //p=productos
    res.json({mensaje:"producto eliminado", response:producto}) // response significa respuesta 
})

app.post("/nuevoUsuario", (req,res)=> {
    const {name, last_name, identification, email, phone }=req.body ; // destructuracion 
    connection.query("INSERT INTO student (name, last_name, identification, email, phone) VALUES(?,?,?,?,?)",
    [name, last_name, identification, email, phone], (err, result)=>{
        

    })
    
    res.send("se añadio correctamente")
})

app.get('/list', (req, res) => {
    connection.query('SELECT * FROM student', (req, resp) =>{
        res.send(resp)
    })
})

app.get("/G-estudent", (req,res)=>{
    const {identification} = req.body;
    connection.query('SELECT * FROM student WHERE identification = ?',[identification], (req,result)=>{
        res.send(result);
    })
    
})

app.put('/U-person/:id', (req,res)=>{ 
    const {id} = req.params;
    const{email} = req.body;
    connection.query('UPDATE student set email where id = ?',
        [email, id], (err,result) =>{
            res.json({mensaje: "cambio concretado"})
        })
    })

app.delete('/delete-user/:id', (req, res) => {
    const { id } = req.params;
    connection.query('DELETE FROM student WHERE id=?', 
    [ id], (err) => {
         if (err) throw err;
            res.json({message: 'Estudiante eliminado'});
        });
    });

app.listen(3000, ()=>{
    console.log(" el servidor corriendo por el puerto 3000")
})
