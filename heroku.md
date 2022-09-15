# Heroku



Heroku Tutorials

Prepare an account Sign up on website first Set up Installing Git Installing on Linux $ sudo apt install git-all Installing on Windows https://git-scm.com/download/win Install the Heroku Command Line Interface (CLI) Linux $ sudo snap install heroku --classic Windows https://cli-assets.heroku.com/heroku-x64.exe Login $ heroku login -i Prepare the app $ cd yourprojectname $ git init $ git status $ git add \<filename/folder> ; To reset: $ git reset ( option) $ git commit -m "reason to commit" Deploy the app $ heroku create ; Skip if created before $ git push heroku master $ heroku logs ; View logs $ heroku open ; Open deployed app in browser

Extras Must add start command in package.json in the first time ... "scripts": { "test": "echo "Error: no test specified" && exit 1", "start": "node app.js" }, ... Make any change to app $ git status ; View any change, new files $ git add ... ; Add changed/new items $ git commit -m "reason to commit" $ git push heroku master View git remote url $ git remote -v ; Optional Run command on heroku server directory $ heroku run ls

```
Mongodb online
	mongodb+srv://minhtaile2712:<password>@cluster0-wyhfl.mongodb.net/test?retryWrites=true&w=majority
Mongodb local
	mongodb://localhost/blog_site
	
Made app
https://agile-falls-11402.herokuapp.com/ | https://git.heroku.com/agile-falls-11402.git
```
