# quasar-vuejs-gcp
A sample build and deply of quasar vuejs GCP(Google Cloud Platform) build and deploy. 
Pls check build type before you use. Quasar build is default (SSR, PWA or other types for which things may vary. One may use
# quasar cli, vuejs brief dev env details
        "quasar": "^1.0.0"
        "node": ">= 8.9.0",
        "npm": ">= 5.6.0",
# Part 1
Assuming quasar dev is working 
try
quasar build in your soruce root 
- to build the distribution package (default spa)

# ---- Part 2
login in google cloud
Create a Project (e.g. myprojectspa )
Create a Bucket 
 - let us say "MyBucket"
 - select location -> region
 - select location -> asia-south1(Mumbai) in my case
 - default store (Standard)
 - ACL (Uniform)

Once created, MyBucket will appear in 
In the Bucket browser list 
Upon selection you will get Permission Tab on Right
Click Add Member button
# Select "allUsers"
# Select role ( Storage -> Storage Object Viewer)

# ------- Part 3
Open the Bucket created MyBucket

Upload spa folder (created from quasar build, names may be different)
Upload a app.yaml file (spa + app.yaml on same folder level)

# Following is content of app.yaml file ( runtime can be nodejs10 etc, pls check GCP docs)
runtime: python27
api_version: 1
threadsafe: true
handlers:
  # change spa into your own dist or build directories
  # Serve all static files with urls ending with a file extension
- url: /(.*\..+)$ 
  static_files: spa/\1
  upload: spa/(.*\..+)$  # catch all handler to index.html
- url: /.*
  static_files: spa/index.html
  upload: spa/index.html  
- url: /
  static_dir: spa  

# ---------- 4
# Open Cloud Shell
create a folder say spa
now sync from bucket
# gsutil rsync -r gs://mybucket ./spa
# cd spa
ls spa ===> should list the spa(folder) app.yaml
# gcloud app deploy
link https://[projectname].appspot.com will be displayed 
along with 
# gcloud app browse 
to start your app (Feel free to explore)

Hope you enjoyed using this. Any suggestion please drop in.
--- 
NOTE: Above procedure may not be the best but this got me up and running


