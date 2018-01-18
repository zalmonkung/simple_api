# simple_api
สร้าง API ง่ายๆ ด้วย Node.js และ Express
Share to Facebook
Tweet
expressjs

ขอขอบคุณ Guest Post จากคุณ Chai Phonbopit
ติดตามผลงานเพิ่มเติมได้ที่ devahoy.com

บทความนี้เราจะมาพูดถึงการทำ RESTFul API ด้วย Node.js และ Express กันนะครับ โดยผมจะขอไม่พูดถึงรายละเอียดเชิงลึกหรือทฤษฎีอะไรมากมาย แต่จะขอเน้นไปที่การนำเสนอตัวอย่างและวิธีการทำจากประสบการณ์ที่ผมได้ทำมาเท่านั้น ซึ่งผมเองก็ยังเพิ่มเริ่มศึกษาอยู่ หากมีส่วนไหนผิดพลาด ก็ต้องขออภัยด้วยนะครับ

Step 1 : ติดตั้ง Node.js
อย่างแรกเลยให้เราติดตั้ง Node.js ก่อนครับ สำหรับวิธีการติดตั้ง Node.js และรายละเอียดอื่นๆ สามารถอ่านได้ตามด้านล่างนี้เลยนะครับ

สอนวิธีใช้ Node.js เบื้องต้น
Step 2 : ติดตั้ง Express
ก่อนอื่นให้เราสร้างไฟล์ package.json ขึ้นมาโดยใช้คำสั่ง npm init ให้เรากรอกรายละเอียดให้เรียบร้อย เราก็จะได้ไฟล์ package.json หน้าตาประมาณนี้


{
    "name": "devahoy-nodejs",
    "version": "0.0.1",
    "description": "Devahoy Node.js with Express 4 Tutorial",
    "main": "index.js"
}

package.json เป็นเสมือนบัตรประจำตัวของโปรเจ็คนั้นๆ ที่จะระบุว่า โปรเจ็คนั้นชื่อว่าอะไร มีเวอร์ชันที่เท่าไหร่  ต้องใช้ Dependencies อะไรบ้าง รวมไปถึงรายละเอียดปลีกย่อยอื่นๆ
ต่อมาให้เราติดตั้ง Express ซึ่งเป็น framework สำหรับเขียน Node.js ด้วยคำสั่งด้านล่างนี้ครับ (หากติด permission ให้เติม sudo เข้าไปข้างหน้า)


1
npm install express --save
--save คือการบอกว่านอกจากจะให้ติดตั้ง package นั้นแล้ว ก็ให้เซฟชื่อ package นั้นเอาไว้ใน package.json ด้วย เวลาที่เราย้ายโฟลเดอร์ เราก็จะสามารถติดตั้ง dependencies ทั้งหมดได้ง่ายๆ ด้วยคำสั่ง npm install จะได้ไม่ต้องมา npm install xxxx ไล่ไปทีละ package ให้ยุ่งยากและเสียเวลา
Step 3 : สร้างไฟล์ index.js
เมื่อติดตั้ง Express เสร็จเรียบร้อย ก็ให้เราสร้างไฟล์ index.js ขึ้นมา เพื่อใช้เป็นไฟล์หลักที่ใช้เขียนโค้ด โครงสร้างของโปรเจ็คก็จะหน้าตาประมาณนี้ครับ



├── index.js
├── node_modules
│ └── express
└── package.json
ที่ไฟล์ index.js ให้เราลองเพิ่มโค้ดด้านล่างนี้ลงไปครับ



/* โหลด Express มาใช้งาน */
var app = require('express')();
 
/* ใช้ port 7777 หรือจะส่งเข้ามาตอนรัน app ก็ได้ */
var port = process.env.PORT || 7777;
 
/* Routing */
app.get('/', function (req, res) {
    res.send('<h1>Hello Node.js</h1>');
});
app.get('/index', function (req, res) {
    res.send('<h1>This is index page</h1>');
});
 
/* สั่งให้ server ทำการรัน Web Server ด้วย port ที่เรากำหนด */
app.listen(port, function() {
    console.log('Starting node.js on port ' + port);
});
เสร็จแล้วให้เราลองสั่งให้ Node.js รันโค้ดนี้ ด้วยคำสั่ง node ชื่อไฟล์ แบบนี้


1
node index.js
หากเรามีการแก้โค้ดใดๆ เราจะต้องรัน app ใหม่เสมอ ถึงจะเห็นการเปลี่ยนแปลง
ทีนี้ให้เราเปิด web browser ขึ้นมาสักอัน แล้วลองเข้า http://localhost:7777 และ http://localhost:7777/index ลองดูผลลัพธ์เทียบกันดูครับ

Step 4 : ลองทำ RESTFul API แบบง่ายๆ
จากตัวอย่างก่อนหน้านี้ เราสามารถเขียนโค้ดดัก url แล้วส่งผลลัพธ์กลับไปด้วยข้อความที่แตกต่างกันได้แล้ว ทีนี้เราจะลองมาทำ api ง่ายๆ กันดูบ้าง โดยความต้องการของ api มีดังนี้

GET localhost:7777/
ให้แสดงข้อความต้อนรับ
GET localhost:7777/user
ให้แสดงรายชื่อ user ทั้งหมดในระบบ
GET localhost:7777/user/:id
ให้แสดงข้อมูลของ user ที่ระบุ โดย :id คือ id ของ user ที่ต้องการจะดูข้อมูล
POST localhost:7777/newuser
ให้เพิ่มข้อมูล user ตามข้อมูลที่ส่งไป
เริ่มแรกให้เราสร้างไฟล์ users.js ขึ้นมาก่อน โดยไฟล์นี้ผมจะเอาไว้สำหรับเป็นข้อมูลของ user (ในส่วนนี้ผมขอกำหนดขึ้นมาเองแบบมั่วๆ ซึ่งจริงๆ แล้วเราจะต้องดึงข้อมูลมาจากฐานข้อมูล)



/* ลองใส่ข้อมูลเล่นๆ เพื่อเอาไว้เทส */
var users = [
{
    "id": 1,
    "username": "goldroger",
    "name": "Gol D. Roger",
    "position": "Pirate King"
},
{
    "id": 2,
    "username": "mrzero",
    "name": "Sir Crocodile",
    "position": "Former-Shichibukai"
},
{
    "id": 3,
    "username": "luffy",
    "name": "Monkey D. Luffy",
    "position": "Captain"
},
{
    "id": 4,
    "username": "kuzan",
    "name": "Aokiji",
    "position": "Former Marine Admiral"
},
{
    "id": 5,
    "username": "shanks",
    "name": "'Red-Haired' Shanks",
    "position": "The 4 Emperors"
}
];
 
/* ฟังก์ชันสำหรับหา user ทั้งหมดในระบบ ในส่วนนี้ผมจะให้ส่งค่า users ทั้งหมดกลับไปเลย */
exports.findAll = function() {
    return users;
};
 
/* ฟังก์ชันสำหรับหา user จาก id ในส่วนนี้เราจะวน loop หา users ที่มี id ตามที่ระบุแล้วส่งกลับไป */
exports.findById = function (id) {
    for (var i = 0; i < users.length; i++) {
        if (users[i].id == id) return users[i];
    }
};
จะเห็นว่าทั้ง 2 ฟังก์ชันนั้นต่างก็เป็น method ของ exports ซึ่งเป็นความสามารถของ Node.js ที่จะทำให้เราสามารถนำฟังก์ชันเหล่านั้นไปใช้กับไฟล์อื่นได้ครับ

ต่อมาให้เราเปิดไฟล์ index.js ขึ้นมา แล้วเพิ่ม require('./users') เข้าไป เพื่อให้ index.js สามารถเรียกใช้ฟังก์ชันที่อยู่ใน users.js ได้ ให้เราใส่เอาไว้บนสุดของไฟล์เลย แบบนี้


1
var users = require('./users');
จะเห็นว่าเราไม่จำเป็นต้องใส่ นามสกุลของไฟล์ เช่น .js ลงไปด้วย เพียงแค่ระบุชื่อไฟล์และ path ให้ถูก Node.js มันฉลาดพอที่จะรู้ว่าเรา require ไฟล์ไหน
ต่อมาให้เราลองเพิ่ม route แต่ละอันเข้าไป


app.get('/', function (req, res) {
    res.send('<h1>Hello Node.js</h1>');
});
 
app.get('/user', function (req, res) {
    res.json(users.findAll());
});
 
app.get('/user/:id', function (req, res) {
    var id = req.params.id;
    res.json(users.findById(id));
});
 
app.post('/newuser', function (req, res) {
    var json = req.body;
    res.send('Add new ' + json.name + ' Completed!');
});
 
app.listen(port, function() {
    console.log('Starting node.js on port ' + port);
});
จาก route ด้านบน จะได้ว่า

/
แสดงข้อความว่า “Hello Node.js”
/user
เรียกใช้ฟังก์ชัน users.findAll() แล้วส่งผลลัพธ์กลับไปในรูปแบบของ JSON
/user/:id
เรียกใช้ฟังก์ชัน users.findById(id) โดยค่าของ id นั้นจะดึงมาจาก url ผ่าน req.params.id เมื่อได้ผลลัพธ์แล้วก็ส่งกลับไปในรูปแบบของ JSON
/newuser
รับ req.body ซึ่งเราจะ POST ค่าเป็น JSON ไป แล้วนำค่านั้นมาแสดงผล (โดยปกติแล้ว เราก็จะนำค่าที่ POST มา ไปเซฟลงในฐานข้อมูล แต่สำหรับบทความนี้ผมขอแค่เอาค่านั้นมาแสดงบนหน้าเว็บเท่านั้นครับ
เราสามารถดึง :id ใน route ได้จาก req.params.id
ทีนี้ให้เราลองทดสอบ GET ผ่านหน้าเว็บดูได้เลยครับ แต่ในส่วนของ POST เราจะต้องทดสอบผ่าน cURL หรือว่าจะใช้ Chrome Plugin ที่ชื่อว่า Postman ก็ได้ครับ

Step 5 : ทดสอบ POST ด้วย Postman
ให้เราติดตั้ง Postman จากลิงค์นี้เลย Postman – REST Client

postman

จากนั้นให้เราเปิด Postman แล้วลองทดสอบการ POST โดยการเลือก method ให้เป็น POST จากนั้นก็เลือกข้อมูลเป็นแบบ raw แล้วลองใส่ค่าของ user ที่เราต้องการจะเพิ่มเข้าไปในรูปแบบของ JSON เช่น


{
    "name": "Roronoa Zoro",
    "position": "Swordman"
}
จากนั้นลองกด Send ดู

RESTFul_without_body_parser

เราจะพบว่ามันขึ้น Error แบบนี้ครับ


1
TypeError: Cannot read property 'name' of undefined
ทั้งนี้เป็นเพราะว่ามันไม่สามารถอ่านค่าจาก body ได้ วิธีแก้คือให้เราติดตั้ง body-parser เข้ามาอีกตัว


1
npm install body-parser --save
จากนั้นให้เราตั้งค่า Body Parser ในไฟล์ index.js แบบนี้ เราก็จะสามารถอ่านค่า JSON ได้แล้ว



var users = require('./users');
var app = require('express')();
 
var bodyParser = require('body-parser');
 
var port = process.env.PORT || 7777;
 
// parse application/json
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({
    extended: true
}));
ให้เรารัน app ใหม่ แล้วลองทดสอบดูอีกครั้ง

RESTFul_with_body_parser

เรียบร้อย! ได้ผลลัพธ์ตามที่ต้องการ จะเห็นว่าในบทความนี้ ผมจะเน้นเรื่องการใช้งาน Node.js และ Express เบื้องต้นซะมากกว่าครับ ให้รู้ว่ามันใช้งาน Route ยังไง มี GET, POST รับส่ง request, response แบบฉบับย่อๆ ไม่ได้ลงรายละเอียดมากนัก รวมถึงยังไม่ได้พูดถึงการเชื่อมต่อกับฐานข้อมูลเลย แต่ใช้การจำลองข้อมูลแบบเกรียนๆ นี้แหละ สุดท้ายนี้ก็หวังว่าบทความนี้จะมีประโยชน์กับเพื่อนๆ ไม่มากก็น้อยนะครับ

Source Code : RESTFul API with Node.js and Express

http://www.siamhtml.com/restful-api-with-node-js-and-express/

