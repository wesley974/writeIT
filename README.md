# Windows Firewall Analyser
## Script powershell

```powershell
Need to correct this part
```

# Deploying a Static Website with Surge CLI on Windows
## Node.js

Step 1: Install Surge CLI

First, ensure you have [Node.js](https://nodejs.org/) installed. Then, open your command prompt and install Surge globally:

```bash
npm install -g surge
```
Step 2: Prepare Your HTML Folder

Create a folder named <b>html</b> on your desktop containing your website files:

```
C:\Users\YourName\Desktop\html
    ├── index.html
    ├── about.html
    └── styles.css
```

Step 3: Deploy Your Site

Navigate to your <b>html</b> folder in the command prompt:

```
cd C:\Users\YourName\Desktop\html
```

Deploy your site with Surge:

```
surge
```

Follow the instructions.

Step 4: Update Your Site

To update your site, modify the files in your <b>html</b> folder and run:

```
surge
```

Surge will update the deployment.

Step 5: Remove Your Site

To remove your site, run:

```
surge teardown example.surge.sh
```

Replace example.surge.sh with your domain.

That's it! Your static site is now online!





