# 🚀 Deploying a Simple Web App on a Linux VM

### (HTML + CSS + Express.js backend with 2 endpoints)

## 🎯 Objectives

By the end of this lab, students will:

* Deploy a simple web application inside a Linux VM
* Serve static HTML/CSS files using Express.js
* Build two backend endpoints
* Run and test the application inside the VM
* Understand how frontend and backend connect

Works on **Ubuntu (VMware)**.

---

# 🧰 1. Prerequisites

Inside the VM, install Node.js + npm + gedit :

```
sudo apt update
sudo apt install gedit nodejs npm -y
node -v
npm -v
```

Create a project folder:

```
mkdir simple-webapp
cd simple-webapp
```

---

# 🏗️ 2. Project Structure

Your project will look like:

```
simple-webapp/
 ├── server.js
 ├── package.json
 └── public/
        ├── index.html
        └── style.css
```

Create the frontend folder:

```
mkdir public
```

---

# 🎨 3. Frontend Files (HTML + CSS)

## 📄 `public/index.html`
```
sudo gedit index.html
```
```
<!DOCTYPE html>
<html>
<head>
  <title>My Simple App</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Welcome to My Web App</h1>
  <p>This is a basic HTML and CSS page served by Express.js.</p>
</body>
</html>
```

## 🎨 `public/style.css`
```
sudo gedit style.css
```
```
body {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 50px;
}

h1 {
  color: #007bff;
}
```

---

# ⚙️ 4. Backend — Express.js Server

Initialize a new Node project:

```
cd ..
npm init -y
```

Install Express:

```
npm install express
```

---

## 🖥️ Create `server.js`

```
const express = require("express");
const app = express();
const port = 3000;

// Serve static files from the public folder
app.use(express.static("public"));

// Endpoint 1
app.get("/hello", (req, res) => {
  res.json({ message: "Hello from the backend!" });
});

// Endpoint 2
app.get("/time", (req, res) => {
  res.json({ time: new Date().toISOString() });
});

// Start the server
app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
```

---

# ▶️ 5. Run the Application

Run the server:

```
node server.js
```

If successful, you will see:

```
Server running on http://localhost:3000
```

Now test in your VM browser:

* **Frontend:**
  [http://localhost:3000](http://localhost:3000)

* **Endpoint 1:**
  [http://localhost:3000/hello](http://localhost:3000/hello)

* **Endpoint 2:**
  [http://localhost:3000/time](http://localhost:3000/time)

---

# 🔁 6. Keep the App Running in Current Terminal

Since you’re not using pm2, just keep the terminal open.

To stop the server:

```
CTRL + C
```

To restart the server:

```
node server.js
```

---

# 🌍 7. Access From Host Machine (Optional)

If you want access from **Windows host**:

1. In VMware **Network Adapter → NAT**
2. Allow port inside VM:

   ```
   sudo ufw allow 3000
   ```
3. Check your VM IP:

   ```
   ip a
   ```
4. Visit in your host browser:

   ```
   http://<VM-IP>:3000
   ```

---

# 🧪 8. Lab Tasks

### ✔️ Task 1

Create folder `simple-webapp`.

### ✔️ Task 2

Create HTML + CSS inside `/public`.

### ✔️ Task 3

Install Express and create 2 endpoints:

* `/hello`
* `/time`

### ✔️ Task 4

Serve static files.

### ✔️ Task 5

Run and test everything in the VM browser.

### ✔️ Task 6

Stop and restart the server using CTRL+C and `node server.js`.

---

# 🎉 End of Lab

You now deployed a simple web app (frontend + backend) inside a Linux VM — a core skill for **Cloud**, **DevOps**, and **Backend Engineering**.

