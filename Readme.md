
   DVC - demo 1 
   
   MAKE SURE VERSION 3.50 HAS BEEN INSTALLED 
   
    2 cd .\DVC\first-demo\
   3 git init
   4 dvc init
   5 git status
   6 git commit -m "initializing dvc"
   9 dvc get https://github.com/iterative/dataset-registry get-started/data.xml -o data/data.xml
   
   mkdir C:\SOFTWARES\dvcstore
   
   
   11 dvc remote add -d myremote C:\SOFTWARES\dvcstore
  12 dvc push
  13 dvc pull
  14 dvc add data/data.xml
  15 dvc push
  16 dvc pull


UPDATE EXISTING FILE AND DO VERSIONING 


21 copy data/data.xml c:\temp\data.xml
  22 type c:\temp\data.xml >> data/data.xml
  23 dvc add data/data.xml
  24 dvc push
  28 git add data/data.xml.dvc
  29 git commit -m "Dataset updates"
  30 dvc pull
  31 git log
  33 git checkout HEAD~1
  35 git log
  36 dvc checkout
 
 
 
 
 PIPELINE DEMO
 --------------
 
UNDER "DVC" FOLDER 
 
   git clone https://github.com/vishymails/code.git
   
   RENAME "CODE" FOLDER TO "pipeline-demo"
 
 
 
   19  git init
   20  dvc init
   
    dvc get https://github.com/iterative/dataset-registry get-started/data.xml -o data/data.xml
   21  dvc add data/data.xml
   
   28  pip install -r src/requirements.txt
   
   
   31  git add .
   32  git status
   33  git commit -m "initial commit"
   
    36  dvc stage add -n prepare -p prepare.seed,prepare.split -d src/prepare.py -d data/data.xml -o data/prepared python src/prepare.py data/data.xml
   37  dvc stage add -n featurize -p featurize.max_features,deaturize.ngrams -d src/featurization.py -d data/prepared -o data/features python src/featurization.py data/prepared data/features
   38  dvc stage add -n train -p train.seed.train.n_est,train.min_split -d src/train.py -d data/features -o model.pkl python src/train.py data/features model.pkl
   39  git add .
   40  git commit -m "pipeline defined"
   41  
   
   
    45  dvc repro
    
    IF YOU FACE ERRORS CHECK prepare.py 
    
    line 59 add encoding parameter 
    
    
    with open(input, encoding="utf-8") as fd_in:
        input_lines = fd_in.readlines()



dvc repro


   
   MODIFY PIPELINE BY ADDING NEW STAGE 
   
   28  dvc repro
   29  dvc stage add -n evaluate -d src/evaluate.py -d model.pkl -d data/features -o eval python src/evaluate.py model.pkl data/features
   30  dvc dag
   31  dvc repro
   32  git add .
   33  git status
   34  git commit -a -m "create eval stage"
   
   
   
   GENERATED DATA CAN BE VIEWED IN METRICS EDITOR 
   
   
   35  dvc metrics show
   36  dvc plots show
   37  history
    
    
    
    MAKE CHANGES AND SEE THE DIFFERENCE 
    
    
    PARAMS.YAML
    -----------
    
    prepare:
  split: 0.20
  seed: 20170428

featurize:
  #max_features: 100
  #ngrams: 1
  max_features: 200
  ngrams: 2

train:
  seed: 20170428
  n_est: 50
  min_split: 0.01





TRY THESE COMMANDS AFTER SAVING PARAMS.YAML FILE 

    
      38  dvc repro
   39  dvc params diff
   40  dvc metrics diff
   41  dvc plots diff
   42  history
    
