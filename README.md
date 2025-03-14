# test
const express = require('express');
const mysql = require('mysql');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const PORT = 3000;

app.use(cors());
app.use(bodyParser.json());


const db = mysql.createConnection({
    host: 'localhost',
    user: 'root', 
    password: '', 
    database: 'test_db' 
});

db.connect(err => {
    if (err) {
        console.error('error:', err);
        return;
    }
    console.log('connected');
});

app.post('/users', (req, res) => {
    const { name, age, email } = req.body;

if (!name || !age || !email) {
        return res.status(400).json({ message: 'يرجى ملء جميع الحقول المطلوبة' });
    }

const sql = 'INSERT INTO users (name, age, email) VALUES (?, ?, ?)';
    db.query(sql, [name, age, email], (err, result) => {
        if (err) {
            console.error('error':', err);
            return res.status(500).json({ message: 'حدث خطأ أثناء الإدخال' });
        }
        res.json({ message: 'add', id: result.insertId });
    });
});

app.get('/users', (req, res) => {
    db.query('SELECT * FROM users', (err, results) => {
        if (err) {
            console.error('error':', err);
            return res.status(500).json({ message: 'something wrong' });
        }
        res.json(results);
    });
});

app.listen(PORT, () => {
    console.log('working': http://localhost:${PORT}`);
});
